# TP 7 : Notes de sécurité sur les actions tierces

## Inspection du dépôt `actions/setup-node`
* **Badge créateur vérifié :** Oui (`Verified creator` sur l'organisation GitHub `actions`).
* **Nombre d'étoiles :** ~5k étoiles (voir la page du dépôt au moment du relevé).
* **Dossier du code source :** `src/` (et `dist/` pour le code compilé exécuté par le runner).

---

## Réponses aux questions

### Q3. Pourquoi faut-il épingler une version (`@v4`) plutôt que `@main` ?
Pointer vers `@main` exécute en permanence le dernier commit de la branche principale du dépôt tiers. Cela expose le workflow à deux risques majeurs :
1. **Instabilité (Breaking changes) :** Une mise à jour non rétrocompatible poussée sur `main` peut casser le pipeline du jour au lendemain sans préavis.
2. **Sécurité (Supply chain attack) :** Si le compte d'un mainteneur est compromis et qu'un code malveillant est poussé sur `main`, il sera immédiatement exécuté dans le workflow avec accès aux secrets du dépôt. Épingler un tag de version (`@v4`), ou idéalement un hash de commit SHA complet, garantit l'immutabilité et la prévisibilité du code exécuté.

### Q4. Le badge « créateur vérifié » garantit-il qu'une action est sûre ? Justifiez.
**Non.** Le badge « créateur vérifié » confirme uniquement que GitHub a vérifié l'identité de l'organisation ou de l'entreprise qui publie l'action (appartenance du domaine). Il ne signifie en aucun cas que GitHub a audité le code source de l'action ni qu'elle est exempte de failles de sécurité, de bugs ou de dépendances vulnérables.

### Q5. Citez deux vérifications à faire avant d'utiliser, en production, une action d'un fournisseur inconnu.
1. **Auditer le code source et les permissions :** Lire le fichier `action.yml` et le code exécuté (`src/` ou `Dockerfile`) pour vérifier que l'action ne fait pas d'appels réseau suspects ou d'exfiltration de secrets/variables d'environnement.
2. **Vérifier la maintenance et la réputation du dépôt :** Contrôler la fréquence des mises à jour, le nombre d'étoiles/contributeurs, la gestion des issues de sécurité, et épingler l'action sur un **SHA de commit précis** plutôt que sur un tag modifiable.