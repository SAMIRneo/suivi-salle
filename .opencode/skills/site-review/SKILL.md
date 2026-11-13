---
name: site-review
description: Revue complète d'un site web (qualité, accessibilité, performance, SEO, sécurité). À utiliser dès que l'utilisateur demande de relire, auditer, noter, "reviewer" ou améliorer son site, ou parle de mise en production, même sans préciser l'aspect à vérifier. Coordonne a11y-audit, perf-audit, seo-audit et security-audit.
license: MIT
---

# Revue complète d'un site

## Workflow
1. Identifier la stack (package.json, framework, dossier de build) et 3 pages clés : accueil, page de conversion, page de contenu.
2. Charger puis appliquer `a11y-audit`, `perf-audit`, `seo-audit` et `security-audit`.
3. Lancer les outils disponibles : `npm run build`, `npm run lint`, `npm audit`, `npx lighthouse <url> --output=json`. Ne jamais prétendre avoir exécuté un outil qui ne l'a pas été.
4. Produire le rapport (format ci-dessous), puis corriger d'abord les points Critiques et re-vérifier.

## Format du rapport
- Score /100 par axe (accessibilité, performance, SEO, sécurité, qualité du code).
- Problèmes classés **Critique / Important / Mineur**.
- Pour chaque problème : `fichier:ligne`, pourquoi c'est un problème, correctif concret (extrait de code).
- Terminer par les 3 actions à plus fort impact.

## Règles
- Toute affirmation s'appuie sur une preuve (ligne de code, sortie d'outil), pas sur une supposition.
- Un correctif par problème, minimal, sans refactor non demandé.
- Demander confirmation avant de changer le design ou le contenu éditorial.
