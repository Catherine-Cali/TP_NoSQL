# <span style="color:#d9534f">Redis — Fiche</span>

## <span style="color:#0275d8">I. Introduction</span>
Redis est une base de données clé-valeur stockant les données en mémoire, ce qui lui permet d’être extrêmement rapide.
L’objectif est d’apprendre à manipuler Redis : ajouter, modifier et supprimer des clés, utiliser les différentes structures de données et comprendre son fonctionnement général.

---

## <span style="color:#5bc0de">1. Manipulations de base</span>

### Créer une clé
```redis
SET mykey myvalue
```
Ajoute une clé avec une valeur.

### Lire une valeur
```redis
GET mykey
```
Lit la valeur d’une clé.

### Supprimer une clé
```redis
DEL mykey
```
Supprime la clé (1 = succès, 0 = inexistante).

### CRUD
- Create → SET  
- Read → GET  
- Update → SET (écrase)  
- Delete → DEL  

### Persistance
Redis stocke en RAM.  
Pour éviter la perte de données : activer RDB, AOF ou les deux.

---

## <span style="color:#5cb85c">Compteurs</span>

```redis
SET visitors 0
INCR visitors
DECR visitors
```
Redis garantit la cohérence grâce à son exécution mono-thread.

---

## <span style="color:#f0ad4e">Durée de vie d'une clé (TTL)</span>

```redis
TTL mykey
EXPIRE mykey 60
```
- TTL >= 0 : secondes restantes  
- -1 : pas d'expiration  
- -2 : clé inexistante  

---

## <span style="color:#d9534f">Mémoire saturée</span>

Politiques d'éviction :
- volatile-lru  
- allkeys-lru  
- volatile-ttl  
- noeviction (refuse les écritures)

---

# <span style="color:#0275d8">2. Structures de données</span>

---

# <span style="color:#5bc0de">A. Listes</span>

```redis
LPUSH myList "E1"
RPUSH myList "E1"
LRANGE myList 0 -1
LPOP myList
RPOP myList
```

Résumé :
- LPUSH : ajoute au début  
- RPUSH : ajoute à la fin  
- LPOP : retire le premier  
- RPOP : retire le dernier  
- LLEN : longueur  
- LMOVE : déplace un élément  
- LRANGE : extrait une plage  
- LTRIM : coupe la liste  

---

# <span style="color:#5cb85c">B. Ensembles (Set)</span>

```redis
SADD mySet "A"
SMEMBERS mySet
SREM mySet "A"
SUNION mySet1 mySet2
SINTER mySet1 mySet2
```

Résumé :
- SADD : ajoute  
- SREM : supprime  
- SISMEMBER : vérifie  
- SMEMBERS : affiche  
- SCARD : taille  
- SUNION : union  
- SINTER : intersection  
- SDIFF : différence  

---

# <span style="color:#f0ad4e">C. Sets ordonnés (ZSET)</span>

```redis
ZADD myZSet 100 "Alice"
ZRANGE myZSet 0 -1 WITHSCORES
ZREVRANGE myZSet 0 -1
ZRANK myZSet "Alice"
```

ZSET = éléments uniques triés automatiquement par score.

---

# <span style="color:#d9534f">D. Hash</span>

```redis
HSET user:1 username "alice"
HMSET user:1 username "alice" age 20 email "a@mail.com"
HGET user:1 age
HGETALL user:1
HINCRBY user:1 age 1
HVALS user:1
```

Résumé :
- HSET : ajoute/modifie un champ  
- HMSET : ajoute plusieurs champs  
- HGET : récupère un champ  
- HGETALL : récupère tout  
- HINCRBY : incrémente  
- HVALS : valeurs  

---

# <span style="color:#0275d8">E. Pub/Sub</span>

```redis
SUBSCRIBE news
PSUBSCRIBE news.*
PUBLISH news "Hello"
```

Résumé :
- SUBSCRIBE : écouter un canal  
- PSUBSCRIBE : écouter un motif  
- UNSUBSCRIBE : arrêter  
- PUNSUBSCRIBE : arrêter motif  
- PUBLISH : envoyer un message  
- PUBSUB CHANNELS : lister canaux  
- PUBSUB NUMSUB : nb abonnés  
- PUBSUB NUMPAT : nb patterns  

---

# <span style="color:#5cb85c">Autres commandes utiles</span>

```redis
KEYS *
SELECT 1
```

---

# <span style="color:#d9534f">Reprise sur panne</span>
Sans persistance activée, Redis perd les dernières données non sauvegardées.

---

# <span style="color:#5bc0de">Conclusion</span>
Redis permet de manipuler des structures rapides et flexibles (List, Set, Sorted Set, Hash), gère la concurrence simplement et propose un système Pub/Sub efficace.  
Sa rapidité vient du stockage en RAM, mais nécessite une configuration adaptée pour garantir la persistance.


