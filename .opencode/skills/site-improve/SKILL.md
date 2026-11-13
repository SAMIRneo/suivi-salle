---
name: Site Improve
description: Améliorer ou ajouter des fonctionnalités au site Suivi salle (PWA one-page) en respectant son architecture unique index.html. À utiliser dès que l'utilisateur demande d'ajouter, modifier ou refactoriser une partie du site.
---

# Améliorer Suivi salle

## Contraintes du projet (à respecter absolument)

1. **Fichier unique** : tout (HTML + CSS + JS) vit dans `index.html`. Ne pas créer de
   fichiers `.css` ou `.js` séparés sauf demande explicite.
2. **CSP inline** : la Content-Security-Policy (ligne 8) autorise `'unsafe-inline'`
   pour scripts et styles. Ne pas introduire de dépendances externes (CDN, fonts
   distantes, analytics) sans modifier la CSP et en discuter — la promesse produit
   est « 100 % privé, aucune donnée ne quitte le téléphone ».
3. **Zéro backend** : toutes les données passent par `localStorage`. Ne jamais
   introduire d'appel réseau utilisateur.
4. **Dark mode** : toute couleur ajoutée doit avoir sa déclinaison dans les deux
   blocs `@media (prefers-color-scheme:dark)` et `:root[data-theme="dark"]`.
5. **Targets tactiles** : inputs et boutons ≥ 46–50px de min-height (norme du fichier).
6. **i18n** : tout libellé visible est en français, tutoiement (« tes charges »).

## Workflow

1. Lire la section concernée de `index.html` AVANT d'éditer (le fichier est dense :
   repérer le bloc CSS, la section HTML, et le bloc JS associé).
2. Proposer le plan de modification (quoi, où, impact sur les données stockées) —
   obtenir l'accord si la change touche le format de `localStorage`.
3. Si migration de données nécessaire : écrire une fonction de migration compatible
   avec les anciennes sauvegardes existantes des utilisateurs.
4. Après édition : vérifier visuellement (lancer un serveur local ou ouvrir le
   fichier), tester dark ET light mode, tester mobile (viewport 390px).
5. Mettre à jour le cache du service worker : incrémenter le nom du cache dans
   `sw.js` à CHAQUE modification du contenu, sinon les utilisateurs gardent
   l'ancienne version.
6. Lancer le check final avec la skill `launch-checklist`.
