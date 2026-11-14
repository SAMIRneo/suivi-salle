# Personnalisation « douleurs & maladies » — Plan d'implémentation

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Chaque douleur (dos, genoux, épaules, poignets) et chaque maladie (asthme, diabète, hypertension, cardiaque, épilepsie, thyroïde, grossesse) déclarée au bilan change réellement la séance recommandée, les défis du jour et les conseils — via un moteur de compatibilité central (Option A).

**Architecture:** Une table de risque `RISK` (exercice → tags) + 3 fonctions (`exRisk`, `progRiskScore`, `defiRiskPenalty`) consommées par `recProg()`, `dailyCandidates()` et la vue Séance. Les exercices à risque restent visibles avec badge ⚠️ + alternative (PAINEALT). Aucun changement de format localStorage. Comportement strictement identique sans pain/maladie.

**Tech Stack:** HTML/JS vanilla dans un fichier unique `index.html`, tests par `browser.evaluate` sur serveur local, PWA `sw.js`.

**Spec:** `docs/superpowers/specs/2026-11-14-personnalisation-moteur-design.md` + `docs/audit-simulation/SIMULATION-LEA.md`

## Global Constraints

- Tout le code vit dans `index.html` (fichier unique). Aucune dépendance externe, CSP inchangée, zéro réseau.
- Libellés français, tutoiement (« tes genoux »). Dark + light mode, mobile 390px, cibles ≥ 46px.
- ⚠️ Pitfalls d'édition : ne jamais écrire `\'` dans les tool calls ; pour les chaînes JS contenant une apostrophe, utiliser des guillemets doubles `"..."` ou l'apostrophe typographique `’` (le fichier mélange déjà les deux — suivre le style local). Éditer avec l'outil edit sur du texte exact du fichier.
- Validation syntaxe après chaque tâche : `powershell -NoProfile -ExecutionPolicy Bypass -File C:\Users\samir\AppData\Local\Temp\opencode\extract-js.ps1` puis `node --check` sur les blocs extraits (offset block3 → HTML +561). Toute tâche avec une erreur de syntaxe est refusée.
- Serveur de test : `python -m http.server 8471` en tâche de fond depuis la racine du dépôt ; URL `http://localhost:8471/index.html`.
- Pattern de test (aucun framework) : dans la page, `evaluate` d'un script qui retourne `JSON.stringify({pass:[...],fail:[...]})` — chaque assertion pousse dans `pass`/`fail` avec un libellé. Les tests modifient `S.profile` en mémoire puis `save()` ; restaurer `S.profile` initial en fin de script.
- Commit à chaque tâche : `git add -A && git commit -m "..."` (pas de push avant la tâche 8).

## Review Focus

1. **Profil neutre (aucune douleur/maladie)** : `recProg()` doit retourner exactement les mêmes programmes qu'avant le patch (ex. salle+45min+fq2 → « Full body · Standard »). → Testé Tâche 2 + Tâche 8.
2. **Données legacy** : entrées cardio sans `km`/`j` ne doivent jamais produire de NaN à l'affichage. → Testé Tâche 7.
3. **Cycle vierge ou sans starts** : `cycDayInfo()`/`cycCalendar()` ne doivent ni crasher ni colorier un jour sans données. → Testé Tâche 6.
4. **Texte libre `malTxt`** : un `malTxt` long/avec caractères spéciaux ne doit pas casser le moteur (il n'entre jamais dans les calculs, seulement l'affichage esc()). → Testé Tâche 8.
5. **Badges ⚠️** : lisibles en dark ET light, non cliquables, sans casser le layout du programme (mobile 390px). → Testé Tâche 4 + Tâche 8.

---

### Task 1: Moteur de risque (RISK + exRisk + progRiskScore + defiRiskPenalty)

**Files:**
- Modify: `index.html` — insérer juste après la définition `EXINFO` (ligne ~698, après le `});` qui ferme l'objet).

**Interfaces:**
- Produces (consommé par Tâches 2-4) :
  - `const RISK={...}` — objet `{"Nom d'exercice": ["tag",...]}` ; tags autorisés : `"genoux","dos","epaules","poignets","impact","hiit"`.
  - `function exRisk(name)` → tableau de tags ou `[]` (jamais `null`).
  - `function progRiskScore(p, P)` → nombre (0 si aucun match). `p` = entrée PGW, `P` = `S.profile`.
  - `function defiRiskPenalty(d, P)` → nombre (bonus positif ou pénalité négative).

- [ ] **Step 1: Test (échoue — fonctions absentes)** : evaluate : `typeof exRisk==="function" && typeof progRiskScore==="function" && typeof defiRiskPenalty==="function"` ; `JSON.stringify(exRisk("Squats sautés"))` contient `"genoux"` et `"impact"`.
- [ ] **Step 2: Vérifier que le test échoue** (ReferenceError / typeof undefined).
- [ ] **Step 3: Implémenter.** RISK couvre au minimum : Squats sautés, Pistol squats, Squats pistol (assistés), Fentes, Fentes alternées, Burpees, Jumping jacks, Corde à sauter (option), Squats, Squats au poids du corps, Dips (barres parallèles), Dips sur une chaise, Pompes, Pompes déclinées, Pompes lestées, Tractions (ou australiennes), Dips lestés (barres parallèles), Mollets, Fentes basses (hanches), Mountain climbers, Relevés de jambes, Superman (dos), Rowing assis, Développé épaules (machine), Développé couché (machine). Définir chaque entrée en suivant la logique : sauts → `impact`; charges jambes profondes → `genoux`; flexion lombaire/dos chargé → `dos`; appui au-dessus de la tête → `epaules`; appui paume/poignet fléchi → `poignets`; HIIT → `hiit`.
  - `progRiskScore(p,P)`: somme par exercice de la pénalité : chaque tag ∈ P.pain → −2, tag `impact` si P.pain inclut genoux → −1 supplémentaire, tag `hiit` si maladies inclut asthme → −2, tag `hiit` si maladies inclut hypertension ou cardiaque → −1. Maladies utiles: `(P.maladies||[]).filter(x=>x&&x!=="aucune")`.
  - `defiRiskPenalty(d,P)`: map par `d.id` (voir Tâche 3 pour la table complète) retournant bonus/pénalité ; défaut 0.
- [ ] **Step 4: Test passe** (mêmes assertions + `progRiskScore` >0 pour « Maison · Cardio et HIIT » avec P.pain=["genoux"], =0 avec P.pain=[]).
- [ ] **Step 5: node --check + commit** `feat: moteur de risque exercice-profil (RISK, exRisk, progRiskScore, defiRiskPenalty)`

### Task 2: recProg() par score

**Files:**
- Modify: `index.html` lignes ~723-739 (fonction `recProg`), conservée, corps remplacé.

**Interfaces:**
- Consomme: `progRiskScore` (Tâche 1).
- Produces: `recProg()` → nom de programme (string) — même signature, même contrat grossesse/fatigue.

- [ ] **Step 1: Test** — scénarios (en manipulant S.profile puis restauration) :
  a) neutre (f, salle, 45min, fq2, d, sans pain/maladie, pas street) → « Full body · Standard » ;
  b) neutre sans salle → « Sans équipement · Full body » ;
  c) pains=["genoux"], sans salle, pas de street → JAMAIS « Sans équipement · Full body » ni « Maison · Cardio et HIIT » (assert !includes des deux) ;
  d) mêmes pains, avec salle → peut être « Full body · Standard » (les machines restent ok) ;
  e) maladies=["asthme"] → le résultat ne contient pas « HIIT ».
- [ ] **Step 2: Vérifier échec** (le recProg actuel retourne « Sans équipement · Full body » pour c).
- [ ] **Step 3: Implémenter.** Garder les raccourcis existants (grossesse → « Mobilité · Full body » ; fatigue → « Mobilité · Full body »). Sinon : score de base = 10 − 0,2×|PMETA[n].min − time| ; bonus fq : fq≥5 → +2 sur « Pousser… »,« Tirer… »,« Jambes… » ; fq=4 → +2 sur « Haut du corps »,« Bas du corps » ; fq≤3 → +2 sur les programmes dont le nom contient « Full body » ; street (gear traction ou likes street) → +2 sur les 3 programmes Street (+1 si exp correspond débutant/inter/avancé) ; sans salle (gear) → pénalité −100 sur cat muscu ; pains/maladies → −`progRiskScore` ; retourner le max (tie-break : ordre PGW). Vérifier que a) et b) restent vrais avec CE scoring (ajuster les constantes si nécessaire, mais documenter la valeur retenue dans le message de commit).
- [ ] **Step 4: Test passe** (a-e).
- [ ] **Step 5: node --check + commit** `feat: recProg() pondere par douleurs et maladies`

### Task 3: Défis adaptés (nouvelles entrées + defiRiskPenalty)

**Files:**
- Modify: `index.html` — tableau `DEFIS` (ligne ~840-864) : ajouter 4 entrées à la fin ; `dailyCandidates()` (ligne ~865-886) : remplacer la ligne 883 (`pains.includes("dos")&&d.id==="etire-dos"`) par `s+=defiRiskPenalty(d,P)`.

**Interfaces:**
- Consomme: `defiRiskPenalty` (Tâche 1).
- Produces: nouveaux ids de défis : `mobilite-genoux`, `mobilite-epaules`, `marche-douce`, `echauffement`.

- [ ] **Step 1: Test** — pour P.pain=["genoux"], 20 appels de `dailyCandidates()` (D.shown=[] avant chaque) : jamais `fentes` ; au moins 2× l'un des nouveaux ids genoux/marche/echauffement. Pour pains=["poignets"] : jamais `pompes-max`. Pour pains=["epaules"] : jamais `pompes-max` ni `street`. Profil neutre : `fentes` peut sortir.
- [ ] **Step 2: Vérifier échec** (`fentes` sort pour genoux).
- [ ] **Step 3: Implémenter.** 4 nouveaux défis (textes français tutoiement, cf. spec §3 ; ex. mobilite-genoux : « 5 minutes de mobilité douce pour tes genoux, sans saut »). Table defiRiskPenalty : genoux → fentes −4, squats-rep −1, mobilite-genoux +3, marche-douce +1, echauffement +1 ; dos → etire-dos +3, dorsaux +2, gainage-soir +1, fentes −0.5, squats-rep −0.5 ; epaules → pompes-max −3, street −2, mobilite-epaules +3 ; poignets → pompes-max −3, street −2 ; asthme → respiration +1.5, echauffement +2, cardio-leger +0.5 ; hypertension/cardiaque → pompes-max −2, record −1, marche-douce +1.5, respiration +1 ; diabete → fibre +1.5, marche +1, sucre +1, marche-douce +1 ; grossesse → squats-rep −2, fentes −2, marche-douce +2.
- [ ] **Step 4: Test passe.**
- [ ] **Step 5: node --check + commit** `feat: defis filtres et bonus par douleur/maladie`

### Task 4: Vue Séance — badges ⚠️ + alternatives (PAINEALT)

**Files:**
- Modify: `index.html` — constante `PAINEALT` insérée après `RISK` ; la fonction qui rend le programme (`pgEx`/rendu de `$("pgl")`, ligne ~721) : pour chaque exercice dont `exRisk(ex)` ∩ P.pain ≠ ∅, ajouter un badge `<small style="...">⚠️ Genoux</small>` (un par pain matché, style token existant `var(--warn...)`/couleur d'avertissement déjà utilisée) ; après la liste, bloc `<p class="hint">` listant les alternatives `PAINEALT[pain][exercice]`.

**Interfaces:**
- Consomme: `exRisk` (Tâche 1), P.pain.
- Produces: aucun (affichage).

- [ ] **Step 1: Test** — S.profile.pain=["genoux"], sélectionner « Sans équipement · Full body » dans le programme : le DOM de `#pgl` contient « ⚠️ » et un texte d'alternative contenant « saut » est absent du programme mais présent dans le hint. P.pain=[] → aucun « ⚠️ » dans `#pgl`.
- [ ] **Step 2: Vérifier échec.**
- [ ] **Step 3: Implémenter** PAINEALT (~12 entrées clés `pain` → {exercice: alternative}), rendu esc() partout, badge non interactif (span, pas de bouton).
- [ ] **Step 4: Test passe + capture dark et light** (screenshot mobile 390px).
- [ ] **Step 5: node --check + commit** `feat: badges douleur et alternatives dans la vue Seance`

### Task 5: Conseils par condition (santé + cardio)

**Files:**
- Modify: `index.html` — la fonction qui construit les messages Santé (~lignes 1385-1396, celle qui fait `out.push("Tu as signalé des douleurs...")`) : ajouter par maladie : asthme (« garde ton cardio en continu et modéré, échauffe-toi 10 min, évite l'air froid sec »), hypertension + cardiaque (« pendant le cardio, tu dois pouvoir parler : pas d'apnée, pas de série à l'échec »), diabète (« séance > 1 h → collation à proximité »), épilepsie (« évite l'hyperventilation en apnée et signale ta condition si tu t'entraînes en salle »), thyroïde (« fatigue inhabituelle → allège, fais valider ton suivi par ton médecin »). Aussi dans le bloc santé Cardio (rendu `renderCardio`/santé) une ligne conditionnée.

**Interfaces:** consomme P.maladies ; produit aucun.

- [ ] **Step 1: Test** — S.profile.maladies=["asthme"] : la page Séance contient « air froid ». maladies=["hypertension"] : contient « pouvoir parler ». maladies=[] : aucun de ces textes.
- [ ] **Step 2: Vérifier échec.**
- [ ] **Step 3: Implémenter** (une ligne out.push par condition, esc() si interpolation).
- [ ] **Step 4: Test passe.**
- [ ] **Step 5: node --check + commit** `feat: conseils cardio et sante par condition medicale`

### Task 6: Cycle cohérent + message de retard

**Files:**
- Modify: `index.html` — `cycDayInfo()` (lignes ~1247-1252), `cycCalendar()` (~1254-1267), `renderCyc()` (~1274-1276).

**Interfaces:**
- Consomme: S.cyc {pl, cl, starts[], ent{}, sym{}}.
- Produces: `cycDayInfo(d)` → `{n, i, prev, late}` où `n` = jours écoulés depuis le DERNIER début ≤ d (+1), `i` = phase 0-3 bornée (n ≤ pl → 0 ; n < cl−14 → 1 ; n ≤ cl−12 → 2 ; sinon 3), `late` = n > cl. `cycInfo()` inchangé (déjà dernier-début) mais DOIT retourner le même `n` que `cycDayInfo(d)` pour tout d.

- [ ] **Step 1: Test** — S.cyc={pl:5,cl:28,starts:[...,"2026-09-23"]}, d="2026-11-14" : `cycDayInfo(d).n === cycInfo(d).n` ; `.late === true`. starts=[] → les deux renvoient null, cycCalendar() retourne "". Dernier début = aujourd'hui → late=false, n=1, i=0.
- [ ] **Step 2: Vérifier échec** (n diverge : 52 % 28 +1 ≠ diff+1).
- [ ] **Step 3: Implémenter** : `cycDayInfo` basé sur le dernier début ≤ d (même calcul que `cycInfo`), flag `late`, `prev` = n ≤ pl. Calendrier : jours ≤ dernier début + pl → rouge ; de dernier début+pl+1 jusqu'à dernier début+cl → phases ; au-delà de dernier début+cl → couleur « prévision approximative » (réutiliser `var(--corps-soft)`), aujourd'hui reste toujours cerclé mais jamais coloré « règles » si late. Dans `renderCyc()` : si `c.late` → hint bienveillant « 🩸 Tes règles ont du retard par rapport à d'habitude (X jours depuis le dernier début). C'est fréquent — stress, voyage, cycle qui dérape. Note le jour où elles reviennent ; si ça se répète plusieurs mois, parle-en à un pro de santé. » (compter X = n−1). Ajouter aussi le retard au « Prochain estimé » : « estimé vers … (ou plus tard si ton cycle dérape) ».
- [ ] **Step 4: Test passe + capture Cycle.**
- [ ] **Step 5: node --check + commit** `fix: cycle cohérent (calendrier = texte) + message de retard de règles`

### Task 7: Rattrapage divers (rapport Léa P4-P6)

**Files:**
- Modify: `index.html` — carte onboarding ~179-187 ; `renderCardio` ~907-910 ; Bilan journalier (rendu « Jours avec apport… » + bouton) ; dropdown machines Progrès (`mlist()`/`fill` ~596, select `cmach`).

**Interfaces:** aucun nouveau.

- [ ] **Step 1: Tests** : a) cardio avec entrée sans `km` → la carte semaine n'affiche pas « NaN » ; semaine sans corde → « Sauts » masqué, avec corde → affiché (idem « Distance » si semaine 100 % corde) ; b) Bilan sans apport aujourd'hui → la ligne « 0 sur 0 » absente du DOM (hidden), bouton présent avec hint « Note d'abord ce que tu as mangé dans Nutrition » ; c) machines : avec 6 machines utilisées, le select liste d'abord les 6 utilisées puis les autres.
- [ ] **Step 2: Vérifier échecs.**
- [ ] **Step 3: Implémenter** : reduce `(s,x)=>s+(x.km||0)` / `(x.j||0)` ; masquer `$("ckw")`/`$("cvr")` si la semaine n'a aucune entrée avec km > 0 (idem cjw/j) ; titre carte 1 → « 📊 Tes points de départ (facultatif) » ; bilan : `$("jstat")`-equivalent hidden si 0 jour ; select : trier `mlist()` — d'abord machines présentes dans S.sessions (ordre décroissant d'usage), puis le reste.
- [ ] **Step 4: Tests passent.**
- [ ] **Step 5: node --check + commit** `fix: NaN cardio, stats inutiles masquees, bilan vide, machines triees, titre onboarding`

### Task 8: Matrice multi-profils + vérif syntaxe + mise en ligne + review

**Files:**
- Create: `docs/audit-simulation/TESTS-PROFILS.md` (résultats de la matrice)
- Modify: `sw.js` (CACHE → `suivi-salle-v5`)

- [ ] **Step 1: Matrice automatisée.** Sur le serveur local, compte neuf `prof-test`, puis pour CHAQUE profil — neutre / genoux+asthme / dos / épaules / poignets / diabète / hypertension / cardiaque+épaules / grossesse / malTxt long — évaluer : recProg() (3 tirages avec D.shown reset), 20× dailyCandidates(), présence badges, textes santé, cycle (si données), absence « NaN » sur Progrès/Tests/Corps/Séance. Écrire le tableau des résultats dans `TESTS-PROFILS.md`.
- [ ] **Step 2: Parcours visuel.** Recréer le profil « Léa » (genoux+asthme) : captures Séance (badges + défis), Cycle (cohérence), Progrès (machines triées), Cardio (stats masquées), Bilan (vide propre).
- [ ] **Step 3: Syntaxe complète** : extract-js + `node --check` sur les 3 blocs.
- [ ] **Step 4: sw.js** CACHE → `suivi-salle-v5`.
- [ ] **Step 5: Commit + push** `feat: personnalisation douleurs/maladies (moteur central) + cycle coherent + rattrapages` — puis vérifier le build GitHub Pages (webfetch du live, cache-buster) : badges visibles, pas de régression onglets.
- [ ] **Step 6: Review** : dispatcher le subagent `reviewer` (relecture exigeante du diff, lecture seule) et corriger ce qu'il remonte de bloquant ; relancer la matrice après correction.
- [ ] **Step 7: launch-checklist** : invoquer la skill `launch-checklist` et traiter les points bloquants.

## Self-Review

- Spec coverage : moteur (T1), recProg (T2), défis (T3), badges (T4), conseils (T5), cycle (T6), rapport P4-P6 (T7), preuve multi-profils + mise en ligne (T8). ✔
- Chaque étape = une action vérifiable ; les corps non déterminés (scoring constantes) sont encadrés par les tests a-e de T2. ✔
- Types/noms cohérents : `exRisk`, `progRiskScore`, `defiRiskPenalty`, `PAINEALT`, `RISK` identiques partout. ✔
- Review Focus 1-5 couverts par T2/T7/T6/T8/T4. ✔
- Proportion : plan < spec. ✔
