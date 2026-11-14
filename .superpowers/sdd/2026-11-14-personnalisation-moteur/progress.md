# SDD ledger — plan: docs/superpowers/plans/2026-11-14-personnalisation-moteur.md

## Rulings
- Ruling: travail direct sur `master` (pas de worktree) — le déploiement GitHub Pages part de `master`, pattern établi du projet (commits 85b7f39, b0276d8), et l'utilisateur a demandé l'exécution autonome complète. Coût si faux : déploiement d'une régression → mitigé par les tests multi-profils avant push.
- Ruling: les scripts bash du skill (sdd-workspace, task-brief, review-package) remplacés par des équivalents manuels (PowerShell/git) — environnement Windows sans bash garanti. Coût si faux : nul, mêmes artefacts.
- BASE de départ : 85b7f39526c50651559047abd7c278e24a38215c.

## Progress
