### Task 2: recProg() par score

**Files:**
- Modify: `index.html` lignes ~723-739 (fonction `recProg`), conservÃ©e, corps remplacÃ©.

**Interfaces:**
- Consomme: `progRiskScore` (TÃ¢che 1).
- Produces: `recProg()` â†’ nom de programme (string) â€” mÃªme signature, mÃªme contrat grossesse/fatigue.

- [ ] **Step 1: Test** â€” scÃ©narios (en manipulant S.profile puis restauration) :
  a) neutre (f, salle, 45min, fq2, d, sans pain/maladie, pas street) â†’ Â« Full body Â· Standard Â» ;
  b) neutre sans salle â†’ Â« Sans Ã©quipement Â· Full body Â» ;
  c) pains=["genoux"], sans salle, pas de street â†’ JAMAIS Â« Sans Ã©quipement Â· Full body Â» ni Â« Maison Â· Cardio et HIIT Â» (assert !includes des deux) ;
  d) mÃªmes pains, avec salle â†’ peut Ãªtre Â« Full body Â· Standard Â» (les machines restent ok) ;
  e) maladies=["asthme"] â†’ le rÃ©sultat ne contient pas Â« HIIT Â».
- [ ] **Step 2: VÃ©rifier Ã©chec** (le recProg actuel retourne Â« Sans Ã©quipement Â· Full body Â» pour c).
- [ ] **Step 3: ImplÃ©menter.** Garder les raccourcis existants (grossesse â†’ Â« MobilitÃ© Â· Full body Â» ; fatigue â†’ Â« MobilitÃ© Â· Full body Â»). Sinon : score de base = 10 âˆ’ 0,2Ã—|PMETA[n].min âˆ’ time| ; bonus fq : fqâ‰¥5 â†’ +2 sur Â« Pousserâ€¦ Â»,Â« Tirerâ€¦ Â»,Â« Jambesâ€¦ Â» ; fq=4 â†’ +2 sur Â« Haut du corps Â»,Â« Bas du corps Â» ; fqâ‰¤3 â†’ +2 sur les programmes dont le nom contient Â« Full body Â» ; street (gear traction ou likes street) â†’ +2 sur les 3 programmes Street (+1 si exp correspond dÃ©butant/inter/avancÃ©) ; sans salle (gear) â†’ pÃ©nalitÃ© âˆ’100 sur cat muscu ; pains/maladies â†’ âˆ’`progRiskScore` ; retourner le max (tie-break : ordre PGW). VÃ©rifier que a) et b) restent vrais avec CE scoring (ajuster les constantes si nÃ©cessaire, mais documenter la valeur retenue dans le message de commit).
- [ ] **Step 4: Test passe** (a-e).
- [ ] **Step 5: node --check + commit** `feat: recProg() pondere par douleurs et maladies`


