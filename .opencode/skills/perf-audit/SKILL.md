---
name: perf-audit
description: Audit de performance web (Core Web Vitals : LCP, INP, CLS) et poids des pages. À utiliser pour toute revue de site, et dès que l'utilisateur parle de lenteur, Lighthouse, PageSpeed, images lourdes, bundle ou temps de chargement.
license: MIT
---

# Audit de performance

## Objectifs (mobile, 75e percentile)
LCP < 2,5 s · INP < 200 ms · CLS < 0,1 · poids de page initial < 1 Mo.

## Checklist
- **Images** : formats AVIF/WebP, `width` et `height` définis (évite le CLS), `srcset`/`sizes`, `loading="lazy"` sauf l'image LCP qui reçoit `fetchpriority="high"`.
- **JavaScript** : analyser le bundle (`npx vite-bundle-visualizer`, `next build`, etc.), code splitting, supprimer les dépendances lourdes inutiles, `defer` sur les scripts tiers.
- **CSS** : CSS critique inline, pas de CSS inutilisé volumineux.
- **Polices** : `font-display: swap`, sous-ensemble WOFF2, `preload` de la police principale seulement.
- **Réseau** : compression Brotli/gzip, cache long avec noms de fichiers hachés, HTTP/2 ou 3, `preconnect` vers les origines critiques.
- **Rendu** : éviter les longues tâches JS (> 50 ms), pas de layout thrashing, réserver l'espace des éléments chargés tard (pubs, embeds).

## Méthode
Mesurer avant et après : `npx lighthouse <url> --preset=perf --form-factor=mobile`. Chiffrer le gain de chaque correctif.
