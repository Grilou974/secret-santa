# 🎅 Secret Santa — mise en ligne (GitHub Pages + Firebase)

## 1. Créer la base Firebase (gratuit, ~5 min)

1. Va sur https://console.firebase.google.com → **Créer un projet** (ex. `secret-santa-lutins`). Tu peux désactiver Google Analytics.
2. Dans le menu de gauche : **Créer → Firestore Database → Créer une base de données**.
   - Emplacement : `eur3 (europe-west)`.
   - Mode : **production**.
3. Onglet **Règles** de Firestore → remplace tout par ceci, puis **Publier** :

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /secretsanta/{doc} {
      allow read, write: if true;
    }
  }
}
```

4. Roue dentée ⚙️ → **Paramètres du projet** → en bas, **Vos applications** → icône **</>** (Web) → donne un nom → **Enregistrer**.
   Firebase t'affiche un bloc `const firebaseConfig = { apiKey: ..., ... }`.

## 2. Coller la config dans le site

Ouvre `index.html` et cherche `FIREBASE_CONFIG`. Remplace les `"COLLE_ICI"` par les valeurs de Firebase (apiKey, authDomain, projectId, storageBucket, messagingSenderId, appId).

> La clé `apiKey` n'est pas un secret : elle est faite pour être publique dans un site web.

## 3. Mettre en ligne sur GitHub Pages

1. Crée un repo (ex. `secret-santa`) et mets `index.html` à la racine.
2. **Settings → Pages** → Source : `Deploy from a branch` → branche `main`, dossier `/ (root)` → **Save**.
3. Après 1–2 min, le site est sur `https://TON-PSEUDO.github.io/secret-santa/`.

## 4. Utilisation

- **Lien pour tout le monde** : `https://TON-PSEUDO.github.io/secret-santa/`
- **Lien organisateurs** (bouton pour retirer un inscrit et réinitialiser le tirage) :
  `https://TON-PSEUDO.github.io/secret-santa/?lutins`
  → garde-le pour toi et Corentin.

Les inscriptions se ferment automatiquement au premier tirage. Le tirage se fait via une transaction Firebase : même si deux personnes lancent la roue en même temps, il ne peut pas y avoir de doublon, et personne ne peut se tirer lui-même.

## Bon à savoir

- Le tirage est secret « entre gens de bonne foi » : quelqu'un de très motivé qui fouille la console Firebase ou les outils développeur du navigateur pourrait voir les résultats. Pour une soirée en famille, ça suffit largement.
- Pour refaire une édition (Noël 2027) : change `"noel-2026"` dans `index.html` (cherche `doc("noel-2026")`) et la base repart à zéro.
- Pour changer la liste des prénoms : cherche `const NAMES` dans `index.html`.
