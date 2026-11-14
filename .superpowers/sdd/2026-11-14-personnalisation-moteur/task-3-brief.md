### Task 3: DÃ©fis adaptÃ©s (nouvelles entrÃ©es + defiRiskPenalty)

**Files:**
- Modify: `index.html` â€” tableau `DEFIS` (ligne ~840-864) : ajouter 4 entrÃ©es Ã  la fin ; `dailyCandidates()` (ligne ~865-886) : remplacer la ligne 883 (`pains.includes("dos")&&d.id==="etire-dos"`) par `s+=defiRiskPenalty(d,P)`.

**Interfaces:**
- Consomme: `defiRiskPenalty` (TÃ¢che 1).
- Produces: nouveaux ids de dÃ©fis : `mobilite-genoux`, `mobilite-epaules`, `marche-douce`, `echauffement`.

- [ ] **Step 1: Test** â€” pour P.pain=["genoux"], 20 appels de `dailyCandidates()` (D.shown=[] avant chaque) : jamais `fentes` ; au moins 2Ã— l'un des nouveaux ids genoux/marche/echauffement. Pour pains=["poignets"] : jamais `pompes-max`. Pour pains=["epaules"] : jamais `pompes-max` ni `street`. Profil neutre : `fentes` peut sortir.
- [ ] **Step 2: VÃ©rifier Ã©chec** (`fentes` sort pour genoux).
- [ ] **Step 3: ImplÃ©menter.** 4 nouveaux dÃ©fis (textes franÃ§ais tutoiement, cf. spec Â§3 ; ex. mobilite-genoux : Â« 5 minutes de mobilitÃ© douce pour tes genoux, sans saut Â»). Table defiRiskPenalty : genoux â†’ fentes âˆ’4, squats-rep âˆ’1, mobilite-genoux +3, marche-douce +1, echauffement +1 ; dos â†’ etire-dos +3, dorsaux +2, gainage-soir +1, fentes âˆ’0.5, squats-rep âˆ’0.5 ; epaules â†’ pompes-max âˆ’3, street âˆ’2, mobilite-epaules +3 ; poignets â†’ pompes-max âˆ’3, street âˆ’2 ; asthme â†’ respiration +1.5, echauffement +2, cardio-leger +0.5 ; hypertension/cardiaque â†’ pompes-max âˆ’2, record âˆ’1, marche-douce +1.5, respiration +1 ; diabete â†’ fibre +1.5, marche +1, sucre +1, marche-douce +1 ; grossesse â†’ squats-rep âˆ’2, fentes âˆ’2, marche-douce +2.
- [ ] **Step 4: Test passe.**
- [ ] **Step 5: node --check + commit** `feat: defis filtres et bonus par douleur/maladie`


