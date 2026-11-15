# Matrice multi-profils — personnalisation douleurs & maladies

> Test automatisé (navigateur, localhost) exécuté le 2026-11-15, build `master` (après commits T2-T7).
> Par profil : 3 tirages `recProg()`, 20 tirages de défi du jour (`dailyCandidates`), badges ⚠️ vue Séance, carte santé, ligne cardio conditionnée, cycle (si profil `f`), recherche de « NaN » sur les vues Séance/Progrès/Corps/Tests/Cardio.

## Résultats

| Profil | recProg (3 tirages) | Défis (20 tirages) — exclusions | Badges ⚠️ | Carte santé | Ligne cardio | Cycle | NaN |
|---|---|---|---|---|---|---|---|
| **neutre** (salle, g=n) | Full body · Standard ×3 | mélange libre (fentes 3, pompes-max 3, street 3…) — aucun défi bloqué | 0 | cachée | aucune | — | 0 |
| **genoux + asthme** (sans salle, f) | Sans équipement · Haut du corps ×3 | **jamais fentes, jamais squats-rep** ; mobilité-genoux ×5, échauffement ×5 ; jamais HIIT | 0 (programme haut du corps, sans exercice genoux) | visible (asthme) | « échauffe-toi… air froid sec » | jour 54, late=true | 0 |
| **dos** (salle, h) | Full body · Standard ×3 | **dorsaux 20/20** (biais fort : base train +2,8 et bonus dos +2 > randomness ±0,7) ; fentes/squats-rep plafonnés | 0 (Standard : machines saines) | cachée | aucune | — | 0 |
| **épaules** (salle, h) | **Bas du corps ×3** (jamais un programme pousser) | **jamais pompes-max, jamais street** ; mobilité-épaules ×11 | 0 | cachée | aucune | — | 0 |
| **poignets** (sans salle, h) | Sans équipement · Full body ×3 | **jamais pompes-max, jamais street** | 1 (Pompes, avec alternative affichée) | cachée | aucune | — | 0 |
| **diabète** (salle, f) | Full body · Standard ×3 | libres | 0 | visible | « collation à proximité » | jour 54, late=true | 0 |
| **hypertension** (salle, f) | Full body · Standard ×3 | libres (record/pompes-max plafonnés par pénalité −1/−2) | 0 | visible | « tu dois pouvoir parler » | jour 54, late=true | 0 |
| **cardiaque + épaules** (salle, n) | **Bas du corps ×3** | **jamais pompes-max, jamais street** ; mobilité-épaules ×10 ; jamais HIIT (pénalité hiit) | 0 | visible | « tu dois pouvoir parler » | — | 0 |
| **grossesse** (salle, f) | **Mobilité · Full body ×3** (verrou contractuel) | **jamais fentes, jamais squats-rep** | 0 | visible | aucune (pas de ctip grossesse) | jour 54, late=true | 0 |
| **malTxt long + caractères spéciaux** (f) | Full body · Standard ×3 | identiques au profil neutre — le texte libre n'entre jamais dans les calculs | 0 | cachée | aucune | jour 54, late=true | 0 |

## Vérifications transverses

- **Comportement historique préservé** : profil neutre → « Full body · Standard » (salle) / « Sans équipement · Full body » (sans salle), défis non filtrés.
- **Jamais de « NaN »** sur aucune vue pour aucun profil, y compris avec des entrées legacy sans `km`/`j` (fix T7).
- **Cycle** : `cycInfo`/`cycDayInfo` cohérents (jour 54/54, `late=true`), calendrier = texte, message de retard affiché ; la lutéale « retard » n'influence plus `recProg`/défis/conseils (fix T6, y compris le bug `cycCalendar()` qui **mutait `S.cyc.starts`** via `.sort().pop()` — corrigé par `.slice()`).
- **Badges** : tokens `--danger`/`--danger-soft`, lisibles dark + light (captures T4), non cliquables, layout 390px intact.
- **Machines** : le select Progrès liste d'abord les machines utilisées (usage décroissant), puis le reste (fix T7).
- **Bilan journalier** : sans apport, la ligne « 0 sur 0 » est masquée, bouton + hint « Note tes aliments dans l'onglet Nutrition » présents (fix T7).
- **Onboarding** : carte maxes renommée « 📊 Tes points de départ (facultatif) » (fix T7).

## Parcours visuel « Léa » (genoux + asthme, f)

Captures réalisées sur localhost (mobile 390px, dark) : Séance (badges + alternatives), Cycle (jour 54 + retard), Progrès (machines triées), Cardio (stats masquées si non pertinentes), Bilan (vide propre). Voir le canal de revue.
