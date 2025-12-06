# MongoDB – Réplication et Tolérance aux Pannes

## 1. Introduction

La réplication dans MongoDB permet de dupliquer automatiquement les données sur plusieurs serveurs.
Elle garantit la continuité du service, la haute disponibilité et la sécurité des données en cas de panne.

Un ensemble de serveurs répliqués s'appelle un Replica Set.

Objectifs principaux :

* disponibilité continue
* protection contre la perte de données
* basculement automatique après incident
* lecture distribuée
* résilience du système

---

## 2. Architecture d'un Replica Set

Un Replica Set est composé de plusieurs nœuds contenant chacun une copie des données.

| Type de nœud | Fonction            | Lecture           | Écriture | Devenir Primary |
| ------------ | ------------------- | ----------------- | -------- | --------------- |
| Primary      | Serveur principal   | Oui               | Oui      | Oui             |
| Secondary    | Réplique du Primary | Oui (si autorisé) | Non      | Oui             |
| Arbiter      | Vote uniquement     | Non               | Non      | Non             |

Caractéristiques :

* le Primary reçoit toutes les écritures
* les Secondary répliquent continuellement le Primary
* l'Arbiter ne stocke aucune donnée mais participe aux votes

---

## 3. Fonctionnement de la réplication MongoDB

1. Le client écrit sur le Primary.
2. Le Primary journalise l'opération et renvoie l'acquittement au client.
3. L'opération est ensuite répliquée vers les Secondaries.

La réplication est asynchrone.
Ce fonctionnement implique qu'un Secondary peut avoir un léger retard et donc renvoyer des données non mises à jour.

Mode par défaut : cohérence forte
Les lectures et écritures passent par le Primary afin de garantir une donnée toujours à jour.

---

## 4. Mise en place d'un Replica Set

### Création des répertoires de données

```bash
mkdir disque1 disque2 disque3
```

### Lancement des serveurs avec réplication activée

```bash
mongod --replSet monreplicaset --port 27018 --dbpath disque1/
mongod --replSet monreplicaset --port 27019 --dbpath disque2/
mongod --replSet monreplicaset --port 27020 --dbpath disque3/
```

### Initialisation et ajout des membres

```bash
mongosh --port 27018
```

```js
rs.initiate()
rs.add("localhost:27019")
rs.add("localhost:27020")
```

---

## 5. Commandes essentielles d'administration

```js
rs.status()          // Etat du cluster, roles et santé des nœuds
rs.isMaster()        // Détermine si l'on est sur Primary ou Secondary
rs.config()          // Affiche la configuration du Replica Set
rs.stepDown()        // Force le Primary à céder sa place (élection)
rs.addArb()          // Ajout d'un arbitre
```

Champs importants à lire dans rs.status():

* stateStr : PRIMARY / SECONDARY / ARBITER
* health : 1 = actif / 0 = hors-ligne
* optimeDate : date de dernière réplication
* priority : priorité lors d'une élection

---

## 6. Politique de lecture ReadPreference

Par défaut, lecture uniquement sur Primary.

Modifier le comportement de lecture :

```js
db.getMongo().setReadPref("primary")
db.getMongo().setReadPref("secondary")
db.getMongo().setReadPref("primaryPreferred")
db.getMongo().setReadPref("secondaryPreferred")
db.getMongo().setReadPref("nearest")
```

Attention à la lecture sur les Secondaries : risque de données obsolètes en raison de la réplication asynchrone.

---

## 7. Gestion des pannes et failover automatique

Lorsqu'un Primary tombe, MongoDB déclenche automatiquement une élection.
Un Secondary devient alors Primary si :

* il dispose d'une copie à jour des données
* il obtient la majorité des votes
* sa priorité est suffisante

Si la majorité n'est pas atteignable :

* le Replica Set devient disponible uniquement en lecture
* aucune écriture n'est autorisée

Une élection ne permet qu'un seul Primary actif à la fois afin d'éviter les conflits.

---

## 8. Scénarios de panne et résultats attendus

| Situation            | Comportement du cluster                 |
| -------------------- | --------------------------------------- |
| Primary hors service | Election d'un nouveau Primary           |
| Pas de majorité      | Blocage des écritures                   |
| Partition réseau     | Seule la portion majoritaire fonctionne |
| Secondary en retard  | Risque de lecture obsolète              |
| Arbitre présent      | Facilite l'élection en cas de panne     |

Un nombre impair de nœuds est recommandé afin de limiter les égalités de vote.

---

## 9. Configurations avancées

### Ajouter un arbitre

```js
rs.addArb("localhost:27021")
```

Utile pour éviter les blocages de vote lorsque les serveurs sont en nombre pair.

---

### Secondary avec délai (slaveDelay) pour récupération

```js
cfg = rs.conf()
cfg.members[1].slaveDelay = 120
rs.reconfig(cfg)
```

Intérêt :

* récupération après suppression accidentelle
* protection contre une corruption instantanée des données

---

### Secondary caché (hidden) pour backup ou analytics

```js
cfg = rs.conf()
cfg.members[2].hidden = true
cfg.members[2].priority = 0
rs.reconfig(cfg)
```

Avantage : n'interfère pas avec le trafic client.

---

### Changer la priorité d'un nœud pour favoriser son élection

```js
cfg = rs.conf()
cfg.members[i].priority = 2
rs.reconfig(cfg)
```

---

### Suivre la réplication et le retard

```js
rs.printReplicationInfo()
rs.printSecondaryReplicationInfo()
```

Points à surveiller :

* retard de replication (replication lag)
* erreurs de heartbeat
* stabilité réseau
* capacité du journal (oplog)

---

## 10. Points essentiels à retenir

* Un seul Primary actif à tout moment.
* Les Secondary ne peuvent pas accepter d'écritures.
* La réplication est asynchrone, d'où risque de lecture non à jour.
* Perte de majorité = plus d'écriture possible.
* Un Arbitre vote mais ne stocke aucune donnée.
* Nombre impair de nœuds recommandé pour faciliter les élections.

