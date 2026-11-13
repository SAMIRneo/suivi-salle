---
name: a11y-audit
description: Audit d'accessibilité (WCAG 2.2 AA) du HTML, CSS et JS d'un site. À utiliser pour toute revue de site, composant UI, formulaire ou navigation, et dès que l'utilisateur mentionne accessibilité, a11y, lecteur d'écran, contraste ou clavier.
license: MIT
---

# Audit d'accessibilité (WCAG 2.2 AA)

## Checklist
- **Structure** : un seul `<h1>`, titres sans saut de niveau, landmarks (`header`, `nav`, `main`, `footer`), `lang` sur `<html>`, lien « aller au contenu ».
- **Images** : `alt` pertinent ; `alt=""` si décorative ; icônes-boutons avec `aria-label`.
- **Formulaires** : chaque champ a un `<label for>`, erreurs annoncées (`aria-describedby`, `role="alert"`), `autocomplete` correct.
- **Clavier** : tout est utilisable au clavier, ordre de tabulation logique, focus visible (jamais `outline: none` sans remplacement), pas de piège au focus dans les modales.
- **Contraste** : texte 4.5:1 minimum (3:1 pour grand texte et composants UI).
- **Contenu dynamique** : `aria-live` pour les mises à jour, modales avec `role="dialog"` et `aria-modal`.
- **Mouvement** : respecter `prefers-reduced-motion`, rien ne clignote plus de 3 fois par seconde.
- **Cibles tactiles** : 24×24 px minimum (idéalement 44×44).
- **ARIA** : préférer le HTML natif ; pas d'ARIA qui contredit la sémantique native.

## Vérifications automatiques
`npx @axe-core/cli <url>` ou `npx lighthouse <url> --only-categories=accessibility`. Les outils attrapent environ un tiers des problèmes : relire aussi le code à la main.
