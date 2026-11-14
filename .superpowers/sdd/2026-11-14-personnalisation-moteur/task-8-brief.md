### Task 8: Matrice multi-profils + vÃ©rif syntaxe + mise en ligne + review

**Files:**
- Create: `docs/audit-simulation/TESTS-PROFILS.md` (rÃ©sultats de la matrice)
- Modify: `sw.js` (CACHE â†’ `suivi-salle-v5`)

- [ ] **Step 1: Matrice automatisÃ©e.** Sur le serveur local, compte neuf `prof-test`, puis pour CHAQUE profil â€” neutre / genoux+asthme / dos / Ã©paules / poignets / diabÃ¨te / hypertension / cardiaque+Ã©paules / grossesse / malTxt long â€” Ã©valuer : recProg() (3 tirages avec D.shown reset), 20Ã— dailyCandidates(), prÃ©sence badges, textes santÃ©, cycle (si donnÃ©es), absence Â« NaN Â» sur ProgrÃ¨s/Tests/Corps/SÃ©ance. Ã‰crire le tableau des rÃ©sultats dans `TESTS-PROFILS.md`.
- [ ] **Step 2: Parcours visuel.** RecrÃ©er le profil Â« LÃ©a Â» (genoux+asthme) : captures SÃ©ance (badges + dÃ©fis), Cycle (cohÃ©rence), ProgrÃ¨s (machines triÃ©es), Cardio (stats masquÃ©es), Bilan (vide propre).
- [ ] **Step 3: Syntaxe complÃ¨te** : extract-js + `node --check` sur les 3 blocs.
- [ ] **Step 4: sw.js** CACHE â†’ `suivi-salle-v5`.
- [ ] **Step 5: Commit + push** `feat: personnalisation douleurs/maladies (moteur central) + cycle coherent + rattrapages` â€” puis vÃ©rifier le build GitHub Pages (webfetch du live, cache-buster) : badges visibles, pas de rÃ©gression onglets.
- [ ] **Step 6: Review** : dispatcher le subagent `reviewer` (relecture exigeante du diff, lecture seule) et corriger ce qu'il remonte de bloquant ; relancer la matrice aprÃ¨s correction.
- [ ] **Step 7: launch-checklist** : invoquer la skill `launch-checklist` et traiter les points bloquants.

## Self-Review

- Spec coverage : moteur (T1), recProg (T2), dÃ©fis (T3), badges (T4), conseils (T5), cycle (T6), rapport P4-P6 (T7), preuve multi-profils + mise en ligne (T8). âœ”
- Chaque Ã©tape = une action vÃ©rifiable ; les corps non dÃ©terminÃ©s (scoring constantes) sont encadrÃ©s par les tests a-e de T2. âœ”
- Types/noms cohÃ©rents : `exRisk`, `progRiskScore`, `defiRiskPenalty`, `PAINEALT`, `RISK` identiques partout. âœ”
- Review Focus 1-5 couverts par T2/T7/T6/T8/T4. âœ”
- Proportion : plan < spec. âœ”

