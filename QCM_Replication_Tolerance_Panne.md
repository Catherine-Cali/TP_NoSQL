# QCM – Réplication et Tolérance aux Pannes avec MongoDB

> Clique sur « Voir la réponse » pour afficher la correction.

---

## 1. Concepts généraux

### Q1. Un Replica Set MongoDB est principalement utilisé pour

- [ ] A. Le partitionnement horizontal des données (sharding)  
- [ ] B. La réplication des données sur plusieurs serveurs  
- [ ] C. La compression des documents  
- [ ] D. La journalisation applicative  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  
Un Replica Set sert à répliquer les données sur plusieurs serveurs pour la haute disponibilité.

</details>

---

### Q2. Dans un Replica Set, le Primary est

- [ ] A. Le nœud qui stocke les logs uniquement  
- [ ] B. Le nœud qui reçoit toutes les écritures  
- [ ] C. Un nœud de secours qui ne sert qu’en cas de panne  
- [ ] D. Un nœud qui ne participe qu’aux votes  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>

---

### Q3. Les Secondaries, par défaut

- [ ] A. Acceptent les écritures  
- [ ] B. Ne contiennent pas de données  
- [ ] C. Répliquent les données du Primary  
- [ ] D. Peuvent devenir Primary en cas d’élection  

<details>
<summary>Voir la réponse</summary>

Réponses correctes : C, D  

</details>

---

### Q4. Un Arbiter dans un Replica Set

- [ ] A. Stocke une copie complète des données  
- [ ] B. Participe aux votes mais ne stocke pas de données  
- [ ] C. Est toujours Primary  
- [ ] D. Est uniquement utilisé pour la sauvegarde  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>

---

### Q5. MongoDB utilise une réplication

- [ ] A. Synchrone  
- [ ] B. Asynchrone  
- [ ] C. Semi-synchrone  
- [ ] D. Sans réplication  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>

---

## 2. Cohérence, lectures et écritures

### Q6. La cohérence forte dans MongoDB signifie que

- [ ] A. Les données lues proviennent uniquement des Secondaries  
- [ ] B. Les données lues peuvent être en retard  
- [ ] C. Les lectures se font sur le Primary et renvoient la version la plus récente  
- [ ] D. Les écritures sont réparties aléatoirement  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : C  

</details>

---

### Q7. Par défaut, MongoDB autorise les écritures

- [ ] A. Sur tous les nœuds du Replica Set  
- [ ] B. Uniquement sur le Primary  
- [ ] C. Uniquement sur les Secondaries  
- [ ] D. Sur les Secondaries et l’Arbiter  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>

---

### Q8. Lire sur un Secondary est risqué car

- [ ] A. Le Secondary ne contient pas les données  
- [ ] B. La réplication est asynchrone, donc possible retard de données  
- [ ] C. Le Secondary ne sait pas traiter les requêtes  
- [ ] D. Cela supprime les données sur le Primary  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>

---

### Q9. Le paramètre `readPreference: "primary"` signifie

- [ ] A. Lecture uniquement sur les Secondaries  
- [ ] B. Lecture d’abord sur les Secondaries puis sur le Primary  
- [ ] C. Lecture uniquement sur le Primary  
- [ ] D. Lecture sur le nœud le plus proche  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : C  

</details>

---

### Q10. Quel mode de lecture permet de lire uniquement sur les Secondaries ?

- [ ] A. `primary`  
- [ ] B. `secondary`  
- [ ] C. `primaryPreferred`  
- [ ] D. `nearest`  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>

---

## 3. Commandes et configuration

### Q11. La commande `rs.initiate()` sert à

- [ ] A. Arrêter le Replica Set  
- [ ] B. Initialiser un Replica Set  
- [ ] C. Ajouter un Secondary  
- [ ] D. Lancer une élection manuelle  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>

---

### Q12. Pour ajouter un nouveau nœud à un Replica Set on utilise

- [ ] A. `rs.add()`  
- [ ] B. `rs.newNode()`  
- [ ] C. `rs.insertNode()`  
- [ ] D. `rs.join()`  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : A  

</details>

---

### Q13. Pour afficher l’état actuel du Replica Set (rôles, santé, etc.), on utilise

- [ ] A. `rs.info()`  
- [ ] B. `rs.status()`  
- [ ] C. `rs.config()`  
- [ ] D. `db.serverStatus()`  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>

---

### Q14. Pour savoir si le nœud actuel est Primary ou Secondary, on peut utiliser

- [ ] A. `rs.isMaster()`  
- [ ] B. `rs.role()`  
- [ ] C. `db.isPrimary()`  
- [ ] D. `db.role()`  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : A  

</details>

---

### Q15. Pour forcer le Primary à céder sa place (bascule manuelle), la commande est

- [ ] A. `rs.down()`  
- [ ] B. `rs.stepDown()`  
- [ ] C. `rs.elect()`  
- [ ] D. `rs.primaryOff()`  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>

---

## 4. Arbitre, priorités, nœuds spéciaux

### Q16. Pourquoi ajoute-t-on souvent un Arbiter dans un Replica Set ?

- [ ] A. Pour augmenter la capacité de stockage  
- [ ] B. Pour améliorer les performances en lecture  
- [ ] C. Pour obtenir un nombre impair de votes  
- [ ] D. Pour stocker les sauvegardes complètes de la base  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : C  

</details>

---

### Q17. Un Secondary avec `hidden = true` est principalement utilisé pour

- [ ] A. Remplacer automatiquement le Primary  
- [ ] B. Être utilisé par les clients applicatifs en priorité  
- [ ] C. Faire des sauvegardes et des analyses sans impacter la production  
- [ ] D. Servir uniquement de cache  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : C  

</details>

---

### Q18. Modifier la priorité d’un nœud dans la configuration permet

- [ ] A. De changer sa taille de disque  
- [ ] B. D’augmenter sa mémoire  
- [ ] C. D’influencer sa probabilité de devenir Primary  
- [ ] D. De le rendre plus rapide en lecture  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : C  

</details>

---

### Q19. Un Secondary configuré avec `slaveDelay` (par exemple 120 secondes)

- [ ] A. Est toujours plus rapide que le Primary  
- [ ] B. Réplique les données avec un retard volontaire  
- [ ] C. Ne réplique jamais les suppressions  
- [ ] D. N’est plus éligible pour la réplication  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>

---

### Q20. La commande `rs.reconfig(cfg)` sert à

- [ ] A. Reconfigurer le Replica Set sans redémarrage  
- [ ] B. Redémarrer tous les nœuds automatiquement  
- [ ] C. Supprimer la configuration actuelle  
- [ ] D. Créer un nouveau Replica Set  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : A  

</details>

---

## 5. Résilience et tolérance aux pannes

### Q21. Si le Primary tombe et qu’une majorité de nœuds est encore joignable, MongoDB

- [ ] A. S’arrête complètement  
- [ ] B. Lance une élection pour choisir un nouveau Primary  
- [ ] C. Passe en mode lecture seule définitif  
- [ ] D. Supprime automatiquement le Replica Set  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>

---

### Q22. Que se passe-t-il si aucun nœud n’obtient la majorité lors d’une élection ?

- [ ] A. Un Primary est tout de même désigné au hasard  
- [ ] B. Les écritures sont bloquées, le Replica Set passe en lecture seule  
- [ ] C. Les Secondaries deviennent tous Primary  
- [ ] D. L’Arbiter devient automatiquement Primary  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>

---

### Q23. Un réseau instable peut provoquer

- [ ] A. Des bascules de Primary fréquentes  
- [ ] B. Une absence totale de réplication  
- [ ] C. Des partitions réseau  
- [ ] D. Des retards de réplication  

<details>
<summary>Voir la réponse</summary>

Réponses correctes : A, C, D  

</details>

---

### Q24. Une partition réseau dans un Replica Set signifie

- [ ] A. Les serveurs sont éteints  
- [ ] B. Les serveurs sont allumés mais ne peuvent plus tous communiquer entre eux  
- [ ] C. Le Primary est obligé de rester Primary  
- [ ] D. Le cluster est divisé en plusieurs groupes isolés  

<details>
<summary>Voir la réponse</summary>

Réponses correctes : B, D  

</details>

---

### Q25. Pourquoi est-il recommandé d’avoir un nombre impair de nœuds votants ?

- [ ] A. Pour faciliter le sharding  
- [ ] B. Pour éviter les égalités pendant les élections  
- [ ] C. Pour diminuer la consommation mémoire  
- [ ] D. Pour améliorer la compression des données  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>

---

## 6. Scénarios pratiques

### Q26. Vous avez 3 nœuds : 27017 (Primary), 27018 (Secondary), 27019 (Arbiter).  
Le Primary devient injoignable. Que se passe-t-il ?

- [ ] A. Le Replica Set s’arrête  
- [ ] B. Le Secondary 27018 devient Primary après élection  
- [ ] C. L’Arbiter devient Primary  
- [ ] D. Aucune élection ne peut avoir lieu  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>

---

### Q27. Un Secondary a plusieurs minutes de retard sur le Primary. Quelles conséquences ?

- [ ] A. Les lectures sur ce Secondary peuvent être obsolètes  
- [ ] B. Il ne pourra pas être élu Primary  
- [ ] C. Il perd toutes ses données  
- [ ] D. Il devient automatiquement Arbiter  

<details>
<summary>Voir la réponse</summary>

Réponses correctes : A, B  

</details>

---

### Q28. Un développeur lit depuis un Secondary et obtient une donnée obsolète.  
La raison la plus probable est que

- [ ] A. Le Secondary n’est pas configuré  
- [ ] B. La réplication est asynchrone et le Secondary a un retard  
- [ ] C. Le Primary ne fonctionne plus  
- [ ] D. L’Arbiter bloque la réplication  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>

---

### Q29. Dans une application critique, on veut que chaque écriture soit confirmée par au moins deux nœuds. Quel `writeConcern` utiliser ?

- [ ] A. `{ w: 1 }`  
- [ ] B. `{ w: 2 }`  
- [ ] C. `{ w: "majority" }`  
- [ ] D. `{ w: 0 }`  

<details>
<summary>Voir la réponse</summary>

Réponses possibles :  
- `{ w: 2 }` si le Replica Set a au moins 2 nœuds de données  
- `{ w: "majority" }` pour validation par la majorité des nœuds  

</details>

---

### Q30. MongoDB évite d’avoir plusieurs Primary en même temps car

- [ ] A. Ce n’est pas performant  
- [ ] B. Cela peut créer des divergences de données (split-brain)  
- [ ] C. Cela augmente trop l’usage CPU  
- [ ] D. Cela empêche la réplication  

<details>
<summary>Voir la réponse</summary>

Réponse correcte : B  

</details>
