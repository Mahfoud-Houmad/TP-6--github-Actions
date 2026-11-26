# Réponses - TP 8 : Stratégies GitHub Actions

### Q1. Combien de jobs sont générés ? Donnez la liste des combinaisons.
Il y a **6 jobs** générés :
1. `ubuntu-latest` + Python `3.11`
2. `ubuntu-latest` + Python `3.12`
3. `ubuntu-latest` + Python `3.13`
4. `windows-latest` + Python `3.11`
5. `windows-latest` + Python `3.12`
6. `windows-latest` + Python `3.13`

---

### Q2. Combien de jobs au total ?
Il y a **6 jobs au total** :
- Les 6 combinaisons initiales moins combinaison exclue (`windows-latest` + `3.11`)
- Et on ajoute une combinaison via `include` (`ubuntu-latest` + `3.14`)

---

### Q3. Que se passerait-il si vous tentiez d'exclure la combinaison 3.14 ? Pourquoi ?
La combinaison `3.14` **ne serait pas exclue**, parce que GitHub Actions évalue la directive `exclude` **uniquement sur les combinaisons générées par la matrice de base**, *avant* d'appliquer la directive `include`. Comme `3.14` n'existe pas dans la matrice initiale et n'est ajouté qu'à l'étape `include`, `exclude` n'a aucun effet sur lui.

---

### Q4. Quelle est la différence observée entre `fail-fast: true` et `fail-fast: false` ? Dans quel cas choisiriez-vous `true` ? `false` ?
  - Avec `fail-fast: true` : dès qu'un premier job Windows échoue, GitHub Actions annule immédiatement tous les autres jobs encore en cours d'exécution ou en attente.
  - Avec `fail-fast: false` : l'échec des jobs Windows n'interrompt pas les autres.
- **Quand choisir `true` :** Sur des pipelines fréquents ou coûteux, pour obtenir un retour rapide et économiser des minutes de calcul dès qu'une erreur bloquante est détectée.
- **Quand choisir `false` :** Lorsqu'on veut un diagnostic complet sur toutes les plateformes/versions afin de savoir exactement quels environnements sont impactés et lesquels fonctionnent.

---

### Q5. Donnez deux raisons d'utiliser `max-parallel` en entreprise.
1. **Éviter la saturation d'une ressource partagée externe :** Si les tests d'intégration sollicitent une même base de données de test, une API tierce limitée en requêtes ou un serveur de staging unique, limiter le parallélisme évite les conflits et les timeouts.
2. **Maîtriser la consommation des runners :** Sur des runners auto-hébergés  ou avec un quota limité de jobs simultanés sur l'organisation, `max-parallel` empêche un seul workflow de monopoliser toute la capacité de calcul au détriment des autres équipes.