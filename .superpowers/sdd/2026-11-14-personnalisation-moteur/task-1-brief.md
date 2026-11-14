### Task 1: Moteur de risque (RISK + exRisk + progRiskScore + defiRiskPenalty)

**Files:**
- Modify: `index.html` â€” insÃ©rer juste aprÃ¨s la dÃ©finition `EXINFO` (ligne ~698, aprÃ¨s le `});` qui ferme l'objet).

**Interfaces:**
- Produces (consommÃ© par TÃ¢ches 2-4) :
  - `const RISK={...}` â€” objet `{"Nom d'exercice": ["tag",...]}` ; tags autorisÃ©s : `"genoux","dos","epaules","poignets","impact","hiit"`.
  - `function exRisk(name)` â†’ tableau de tags ou `[]` (jamais `null`).
  - `function progRiskScore(p, P)` â†’ nombre (0 si aucun match). `p` = entrÃ©e PGW, `P` = `S.profile`.
  - `function defiRiskPenalty(d, P)` â†’ nombre (bonus positif ou pÃ©nalitÃ© nÃ©gative).

- [ ] **Step 1: Test (Ã©choue â€” fonctions absentes)** : evaluate : `typeof exRisk==="function" && typeof progRiskScore==="function" && typeof defiRiskPenalty==="function"` ; `JSON.stringify(exRisk("Squats sautÃ©s"))` contient `"genoux"` et `"impact"`.
- [ ] **Step 2: VÃ©rifier que le test Ã©choue** (ReferenceError / typeof undefined).
- [ ] **Step 3: ImplÃ©menter.** RISK couvre au minimum : Squats sautÃ©s, Pistol squats, Squats pistol (assistÃ©s), Fentes, Fentes alternÃ©es, Burpees, Jumping jacks, Corde Ã  sauter (option), Squats, Squats au poids du corps, Dips (barres parallÃ¨les), Dips sur une chaise, Pompes, Pompes dÃ©clinÃ©es, Pompes lestÃ©es, Tractions (ou australiennes), Dips lestÃ©s (barres parallÃ¨les), Mollets, Fentes basses (hanches), Mountain climbers, RelevÃ©s de jambes, Superman (dos), Rowing assis, DÃ©veloppÃ© Ã©paules (machine), DÃ©veloppÃ© couchÃ© (machine). DÃ©finir chaque entrÃ©e en suivant la logique : sauts â†’ `impact`; charges jambes profondes â†’ `genoux`; flexion lombaire/dos chargÃ© â†’ `dos`; appui au-dessus de la tÃªte â†’ `epaules`; appui paume/poignet flÃ©chi â†’ `poignets`; HIIT â†’ `hiit`.
  - `progRiskScore(p,P)`: somme par exercice de la pÃ©nalitÃ© : chaque tag âˆˆ P.pain â†’ âˆ’2, tag `impact` si P.pain inclut genoux â†’ âˆ’1 supplÃ©mentaire, tag `hiit` si maladies inclut asthme â†’ âˆ’2, tag `hiit` si maladies inclut hypertension ou cardiaque â†’ âˆ’1. Maladies utiles: `(P.maladies||[]).filter(x=>x&&x!=="aucune")`.
  - `defiRiskPenalty(d,P)`: map par `d.id` (voir TÃ¢che 3 pour la table complÃ¨te) retournant bonus/pÃ©nalitÃ© ; dÃ©faut 0.
- [ ] **Step 4: Test passe** (mÃªmes assertions + `progRiskScore` >0 pour Â« Maison Â· Cardio et HIIT Â» avec P.pain=["genoux"], =0 avec P.pain=[]).
- [ ] **Step 5: node --check + commit** `feat: moteur de risque exercice-profil (RISK, exRisk, progRiskScore, defiRiskPenalty)`


