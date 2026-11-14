### Task 4: Vue SÃ©ance â€” badges âš ï¸ + alternatives (PAINEALT)

**Files:**
- Modify: `index.html` â€” constante `PAINEALT` insÃ©rÃ©e aprÃ¨s `RISK` ; la fonction qui rend le programme (`pgEx`/rendu de `$("pgl")`, ligne ~721) : pour chaque exercice dont `exRisk(ex)` âˆ© P.pain â‰  âˆ…, ajouter un badge `<small style="...">âš ï¸ Genoux</small>` (un par pain matchÃ©, style token existant `var(--warn...)`/couleur d'avertissement dÃ©jÃ  utilisÃ©e) ; aprÃ¨s la liste, bloc `<p class="hint">` listant les alternatives `PAINEALT[pain][exercice]`.

**Interfaces:**
- Consomme: `exRisk` (TÃ¢che 1), P.pain.
- Produces: aucun (affichage).

- [ ] **Step 1: Test** â€” S.profile.pain=["genoux"], sÃ©lectionner Â« Sans Ã©quipement Â· Full body Â» dans le programme : le DOM de `#pgl` contient Â« âš ï¸ Â» et un texte d'alternative contenant Â« saut Â» est absent du programme mais prÃ©sent dans le hint. P.pain=[] â†’ aucun Â« âš ï¸ Â» dans `#pgl`.
- [ ] **Step 2: VÃ©rifier Ã©chec.**
- [ ] **Step 3: ImplÃ©menter** PAINEALT (~12 entrÃ©es clÃ©s `pain` â†’ {exercice: alternative}), rendu esc() partout, badge non interactif (span, pas de bouton).
- [ ] **Step 4: Test passe + capture dark et light** (screenshot mobile 390px).
- [ ] **Step 5: node --check + commit** `feat: badges douleur et alternatives dans la vue Seance`


