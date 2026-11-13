---
name: security-audit
description: Audit de sécurité défensif d'un site web (en-têtes HTTP, dépendances, secrets, XSS, formulaires). À utiliser pour toute revue de site avant mise en ligne, et dès que l'utilisateur parle de sécurité, HTTPS, CSP, clés API ou vulnérabilités de son propre projet.
license: MIT
---

# Audit de sécurité (défensif, sur le projet de l'utilisateur)

## Checklist
- **Secrets** : chercher clés API, tokens et mots de passe dans le code et l'historique git ; `.env` dans `.gitignore` ; aucune clé secrète côté client.
- **Dépendances** : `npm audit` (ou équivalent), mises à jour des versions vulnérables, lockfile commité.
- **En-têtes HTTP** : `Strict-Transport-Security`, `Content-Security-Policy` (sans `unsafe-inline` si possible), `X-Content-Type-Options: nosniff`, `Referrer-Policy`, `Permissions-Policy`, protection contre le clickjacking (`frame-ancestors`).
- **XSS** : repérer `innerHTML`, `dangerouslySetInnerHTML`, `eval`, `document.write` alimentés par des données utilisateur ; échapper ou assainir.
- **Formulaires et API** : validation côté serveur, protection CSRF, limitation de débit, messages d'erreur sans détails techniques.
- **Cookies** : `Secure`, `HttpOnly`, `SameSite`.
- **Liens externes** : `rel="noopener noreferrer"` avec `target="_blank"`.
- **HTTPS** : redirection HTTP vers HTTPS, pas de contenu mixte.

## Règle
Ne tester que le projet de l'utilisateur. Ne jamais afficher un secret trouvé en entier : le masquer et recommander de le révoquer.
