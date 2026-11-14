### Task 5: Conseils par condition (santÃ© + cardio)

**Files:**
- Modify: `index.html` â€” la fonction qui construit les messages SantÃ© (~lignes 1385-1396, celle qui fait `out.push("Tu as signalÃ© des douleurs...")`) : ajouter par maladie : asthme (Â« garde ton cardio en continu et modÃ©rÃ©, Ã©chauffe-toi 10 min, Ã©vite l'air froid sec Â»), hypertension + cardiaque (Â« pendant le cardio, tu dois pouvoir parler : pas d'apnÃ©e, pas de sÃ©rie Ã  l'Ã©chec Â»), diabÃ¨te (Â« sÃ©ance > 1 h â†’ collation Ã  proximitÃ© Â»), Ã©pilepsie (Â« Ã©vite l'hyperventilation en apnÃ©e et signale ta condition si tu t'entraÃ®nes en salle Â»), thyroÃ¯de (Â« fatigue inhabituelle â†’ allÃ¨ge, fais valider ton suivi par ton mÃ©decin Â»). Aussi dans le bloc santÃ© Cardio (rendu `renderCardio`/santÃ©) une ligne conditionnÃ©e.

**Interfaces:** consomme P.maladies ; produit aucun.

- [ ] **Step 1: Test** â€” S.profile.maladies=["asthme"] : la page SÃ©ance contient Â« air froid Â». maladies=["hypertension"] : contient Â« pouvoir parler Â». maladies=[] : aucun de ces textes.
- [ ] **Step 2: VÃ©rifier Ã©chec.**
- [ ] **Step 3: ImplÃ©menter** (une ligne out.push par condition, esc() si interpolation).
- [ ] **Step 4: Test passe.**
- [ ] **Step 5: node --check + commit** `feat: conseils cardio et sante par condition medicale`


