### Task 6: Cycle cohÃ©rent + message de retard

**Files:**
- Modify: `index.html` â€” `cycDayInfo()` (lignes ~1247-1252), `cycCalendar()` (~1254-1267), `renderCyc()` (~1274-1276).

**Interfaces:**
- Consomme: S.cyc {pl, cl, starts[], ent{}, sym{}}.
- Produces: `cycDayInfo(d)` â†’ `{n, i, prev, late}` oÃ¹ `n` = jours Ã©coulÃ©s depuis le DERNIER dÃ©but â‰¤ d (+1), `i` = phase 0-3 bornÃ©e (n â‰¤ pl â†’ 0 ; n < clâˆ’14 â†’ 1 ; n â‰¤ clâˆ’12 â†’ 2 ; sinon 3), `late` = n > cl. `cycInfo()` inchangÃ© (dÃ©jÃ  dernier-dÃ©but) mais DOIT retourner le mÃªme `n` que `cycDayInfo(d)` pour tout d.

- [ ] **Step 1: Test** â€” S.cyc={pl:5,cl:28,starts:[...,"2026-09-23"]}, d="2026-11-14" : `cycDayInfo(d).n === cycInfo(d).n` ; `.late === true`. starts=[] â†’ les deux renvoient null, cycCalendar() retourne "". Dernier dÃ©but = aujourd'hui â†’ late=false, n=1, i=0.
- [ ] **Step 2: VÃ©rifier Ã©chec** (n diverge : 52 % 28 +1 â‰  diff+1).
- [ ] **Step 3: ImplÃ©menter** : `cycDayInfo` basÃ© sur le dernier dÃ©but â‰¤ d (mÃªme calcul que `cycInfo`), flag `late`, `prev` = n â‰¤ pl. Calendrier : jours â‰¤ dernier dÃ©but + pl â†’ rouge ; de dernier dÃ©but+pl+1 jusqu'Ã  dernier dÃ©but+cl â†’ phases ; au-delÃ  de dernier dÃ©but+cl â†’ couleur Â« prÃ©vision approximative Â» (rÃ©utiliser `var(--corps-soft)`), aujourd'hui reste toujours cerclÃ© mais jamais colorÃ© Â« rÃ¨gles Â» si late. Dans `renderCyc()` : si `c.late` â†’ hint bienveillant Â« ðŸ©¸ Tes rÃ¨gles ont du retard par rapport Ã  d'habitude (X jours depuis le dernier dÃ©but). C'est frÃ©quent â€” stress, voyage, cycle qui dÃ©rape. Note le jour oÃ¹ elles reviennent ; si Ã§a se rÃ©pÃ¨te plusieurs mois, parle-en Ã  un pro de santÃ©. Â» (compter X = nâˆ’1). Ajouter aussi le retard au Â« Prochain estimÃ© Â» : Â« estimÃ© vers â€¦ (ou plus tard si ton cycle dÃ©rape) Â».
- [ ] **Step 4: Test passe + capture Cycle.**
- [ ] **Step 5: node --check + commit** `fix: cycle cohÃ©rent (calendrier = texte) + message de retard de rÃ¨gles`


