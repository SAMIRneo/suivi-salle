---
name: PWA Hardening
description: Fiabiliser la PWA Suivi salle — manifest, service worker, cache, offline, installabilité, icônes. À utiliser dès qu'on touche à sw.js, manifest.webmanifest, ou qu'on parle d'offline, d'installation ou de mise à jour.
---

# PWA Hardening

## Architecture actuelle

- `manifest.webmanifest` : name/short_name FR, display standalone, portrait,
  theme `#ff5a1f`, 3 icônes (SVG any, PNG 192, PNG 512 maskable).
- `sw.js` : cache par version. **Toute modification de `index.html` ou des icônes
  impose d'incrémenter le nom du cache**, sinon les utilisateurs restent bloqués
  sur l'ancienne version.
- Hébergement : GitHub Pages (`https://samirneo.github.io/suivi-salle/`). Chemins
  relatifs (`./`) obligatoires — pas de leading slash.

## Checklist manifest

1. `background_color` cohérent avec le fond réel de démarrage (éviter le flash).
2. Icône 512 maskable présente (fait) — vérifier zone de sûreté si on la régénère.
3. `start_url` et `scope` en relatif (fait).

## Checklist service worker

1. Version de cache incrémentée à chaque déploiement de contenu modifié.
2. `index.html` en network-first (ou cache avec update) pour que les mises à jour
   arrivent ; assets statiques en cache-first.
3. Purge des anciens caches dans l'événement `activate`.
4. Gestion du fallback offline : si l'app ouvre hors-ligne sans cache → message
   clair, jamais une erreur blanche.
5. Pas de requête cross-origin non gérée dans le fetch handler.

## Vérifications d'installabilité

- Manifest servi en 200, icônes accessibles (tester les URLs directes).
- `apple-touch-icon` présent pour iOS (fait).
- Après install sur Chrome desktop et Android : lancement standalone correct,
  pas de barre d'URL.

## Procédure de test offline

1. Servir en local (ex. `python -m http.server`).
2. Charger la page une fois, puis couper le serveur / couper le réseau.
3. Recharger : l'app doit fonctionner intégralement (tout est localStorage).
4. Vérifier la console : aucune erreur de fetch non interceptée.
