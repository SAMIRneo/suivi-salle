### Task 7: Rattrapage divers (rapport LÃ©a P4-P6)

**Files:**
- Modify: `index.html` â€” carte onboarding ~179-187 ; `renderCardio` ~907-910 ; Bilan journalier (rendu Â« Jours avec apportâ€¦ Â» + bouton) ; dropdown machines ProgrÃ¨s (`mlist()`/`fill` ~596, select `cmach`).

**Interfaces:** aucun nouveau.

- [ ] **Step 1: Tests** : a) cardio avec entrÃ©e sans `km` â†’ la carte semaine n'affiche pas Â« NaN Â» ; semaine sans corde â†’ Â« Sauts Â» masquÃ©, avec corde â†’ affichÃ© (idem Â« Distance Â» si semaine 100 % corde) ; b) Bilan sans apport aujourd'hui â†’ la ligne Â« 0 sur 0 Â» absente du DOM (hidden), bouton prÃ©sent avec hint Â« Note d'abord ce que tu as mangÃ© dans Nutrition Â» ; c) machines : avec 6 machines utilisÃ©es, le select liste d'abord les 6 utilisÃ©es puis les autres.
- [ ] **Step 2: VÃ©rifier Ã©checs.**
- [ ] **Step 3: ImplÃ©menter** : reduce `(s,x)=>s+(x.km||0)` / `(x.j||0)` ; masquer `$("ckw")`/`$("cvr")` si la semaine n'a aucune entrÃ©e avec km > 0 (idem cjw/j) ; titre carte 1 â†’ Â« ðŸ“Š Tes points de dÃ©part (facultatif) Â» ; bilan : `$("jstat")`-equivalent hidden si 0 jour ; select : trier `mlist()` â€” d'abord machines prÃ©sentes dans S.sessions (ordre dÃ©croissant d'usage), puis le reste.
- [ ] **Step 4: Tests passent.**
- [ ] **Step 5: node --check + commit** `fix: NaN cardio, stats inutiles masquees, bilan vide, machines triees, titre onboarding`


