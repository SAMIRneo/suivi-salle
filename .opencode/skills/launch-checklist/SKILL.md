---
name: Launch Checklist
description: Pipeline de validation avant chaque déploiement de Suivi salle — vérifications manuelles rapides puis audits complets avec les skills site-review, a11y-audit, perf-audit, seo-audit et security-audit. À utiliser avant tout commit/publie ou quand l'utilisateur demande de valider le site.
---

# Launch Checklist Suivi salle

Exécuter dans l'ordre. Stop à la première étape non passée et corriger.

## 1. Rapide (30 secondes)

- [ ] `index.html` : CSP, meta description, canonical, JSON-LD intacts.
- [ ] Version de cache dans `sw.js` incrémentée si le contenu a changé.
- [ ] Manifest et chemins d'icônes valides (pas de 404).

## 2. Fonctionnel

- [ ] Ouvrir la page en local (ex. `python -m http.server 8000`).
- [ ] Parcourir chaque section/tabs, enregistrer une donnée test, la retrouver.
- [ ] Dark mode + light mode + bascule manuelle du thème.
- [ ] Recharger : les données persistent (localStorage).

## 3. Offline / PWA

- [ ] Recharger une fois puis couper le serveur : rechargement OK.
- [ ] Console propre : aucune erreur, aucun fetch non intercepté.

## 4. Responsive & a11y express

- [ ] Viewport 390px : pas de scroll horizontal, cibles tactiles ≥ 44px.
- [ ] Navigation clavier possible, focus visible, contrastes AA.

## 5. Audits complets (lancer si étapes 1-4 OK)

Charger et suivre les skills du pack d'audit, dans cet ordre :

1. `a11y-audit` — accessibilité WCAG 2.2 AA
2. `perf-audit` — Core Web Vitals et poids de la page
3. `seo-audit` — balises, données structurées, Open Graph
4. `security-audit` — CSP, dépendances, exposition de données
5. `site-review` — revue globale de synthèse

## 6. Avant commit

- [ ] Un commit par intention claire (feature / fix / contenu).
- [ ] Message de commit en anglais ou français cohérent avec l'historique du dépôt.
- [ ] Après push : vérifier la version déployée sur GitHub Pages (hard-refresh).
