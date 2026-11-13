---
name: seo-audit
description: Audit SEO technique et on-page d'un site (balises, indexation, données structurées, partage social). À utiliser pour toute revue de site, et dès que l'utilisateur parle de référencement, Google, visibilité, meta, sitemap ou Open Graph.
license: MIT
---

# Audit SEO

## Checklist
- **Balises** : `<title>` unique (50-60 car.), `meta description` unique (140-160 car.), `<link rel="canonical">`, un seul `<h1>` qui contient le sujet de la page.
- **Indexation** : `robots.txt` valide, `sitemap.xml` à jour et déclaré, pas de `noindex` accidentel, pages 404 qui renvoient un vrai code 404, redirections 301 sans chaînes.
- **Contenu** : URLs courtes et lisibles, maillage interne avec ancres descriptives, pas de contenu dupliqué.
- **Partage** : Open Graph (`og:title`, `og:description`, `og:image` 1200×630) et Twitter Card.
- **Données structurées** : JSON-LD adapté (Organization, Article, Product, FAQ...), à valider avec le Rich Results Test.
- **International** : `hreflang` cohérent si le site est multilingue.
- **Mobile et vitesse** : viewport correct, Core Web Vitals (voir `perf-audit`).

## Sortie
Tableau page par page des éléments manquants, avec le texte proposé pour chaque title et description.
