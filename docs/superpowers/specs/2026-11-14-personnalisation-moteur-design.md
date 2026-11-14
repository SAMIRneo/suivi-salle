# DESIGN — Moteur de personnalisation « douleurs & maladies » (Option A)

Date : 2026-11-14 · Statut : approuvé par l'utilisateur (« fait option A »)

## Intent & why
La simulation « Léa » (docs/audit-simulation/SIMULATION-LEA.md) a montré que le profil
déclaré au bilan (douleurs : dos/genoux/épaules/poignets ; maladies : asthme, diabète,
hypertension, cardiaque, épilepsie, thyroïde, grossesse) n'influence presque rien :
`recProg()` ignore tout sauf grossesse/fatigue, les défis ne filtrent que « dos »,
le cardio ignore les conditions. Objectif : que **chaque douleur/maladie change
réellement** la séance recommandée, les défis du jour et les conseils, avec une
seule source de vérité.

## Goals
1. Moteur central de compatibilité exercice ↔ profil (score de risque par tag).
2. Toutes les fonctions existantes (`recProg`, `dailyCandidates`, conseils, séance)
   consomment ce moteur.
3. Exercices à risque **affichés avec badge + alternative** (pas masqués) — EXINFO
   contient déjà des variantes « Plus facile ».
4. Matrice de tests multi-profils (≥7 profils) prouvant que les recommandations
   divergent et qu'aucun contenu à risque n'est poussé au mauvais profil.
5. Rattrapage du reste du rapport Léa : cycle cohérent + message de retard,
   onboarding en double, NaN cardio défensif + « 0 sauts » masqué, Bilan « 0 sur 0 »
   masqué, dropdown machines triées par usage.

## Non-goals / anti-goals
- Pas de nouveaux programmes séparés par douleur (option B refusée).
- Pas de masquage dur des exercices (option C refusée).
- Pas de changement du format de `localStorage` → aucune migration.
- Pas de promesse médicale : les messages restent des conseils, avec renvoi pro de santé.

## Approches (rappel)
- **A (retenue)** : moteur de scoring central consommé partout. Extensible, une
  source de vérité, comportement inchangé si aucun pain/maladie.
- B (refusée) : programmes dédiés par douleur — contenu à maintenir, croisements non couverts.
- C (refusée) : filtres durs — frustrant, casse les programmes existants.

## Design technique

### 1. Table de risque `RISK`
Par exercice (clé = nom exact utilisé dans PGW/EXINFO), un ensemble de tags :
`genoux`, `dos`, `epaules`, `poignets`, `impact` (sauts), `intensite-max` (HIIT/échec).
Définie comme objet JS littéral (pas de changement de données utilisateur).
Exemples : « Squats sautés » → {genoux, impact} ; « Pistol squats » → {genoux} ;
« Dips (barres parallèles) » → {epaules} ; « Pompes » → {poignets} ; « Burpees » →
{genoux, impact, intensite-max} ; « Jumping jacks » → {genoux, impact}.
- Maladies : `asthme` → pénalise programmes à dominante HIIT/impact (le cardio
  continu lui convient mieux), conseil spécifique ; `hypertension`/`cardiaque` →
  conseil intensité modérée, pénalise légèrement les défis « max/échec » ;
  `diabete` → bonus défis marche/fibres ; `thyroide`/`epilepsie` → conseil
  prudent (pas de pénalité d'exercices, message santé suffit).

### 2. Fonctions du moteur
- `exRisk(name)` → tableau de tags (ou {}).
- `progRiskScore(prog, P)` → somme pondérée des exercices du programme selon
  `P.pain` et maladies (pénalités fortes sur tags douleur déclarée + impact).
- `defiRiskPenalty(defi, P)` → pénalité/bonus par défi (généralise la ligne 883).

### 3. Points d'intégration (fichier unique index.html)
- `recProg()` (~723-739) : conserve les raccourcis grossesse/fatigue, sinon
  **score** tous les programmes (base selon goal/exp/time/fq/place) + pénalité
  risque → choix du meilleur. Comportement identique sans pain/maladie.
- `dailyCandidates()` (~865-886) : remplace la règle « dos » isolée par
  `defiRiskPenalty` ; nouveaux défis ciblés : mobilité genoux douce, mobilité
  épaules/nuque, échauffement progressif (asthme), marche modérée (hypertension).
- Vue Séance : sous le programme sélectionné, liste des exercices à risque pour le
  profil avec badge « ⚠️ [douleur] » + alternative tirée de EXINFO/PAINEALT.
- Conseils cardio (renderCardio / santé) : phrases par condition (asthme : air
  froid, échauffement long, cardio continu ; hypertension/cardiaque : talk test,
  pas d'apnée ; diabète : collation si séance longue).

### 4. Rattrapage rapport Léa
- Cycle : `cycDayInfo` recalé sur le dernier début (comme `cycInfo`), plus
  message bienveillant de retard (>35 j sans règles → note non alarmante).
- Onboarding : 1re carte renommée « 📊 Tes points de départ (facultatif) ».
- `renderCardio` : défauts `||0` défensifs + masquer « Distance »/« Sauts »
  quand non pertinents pour la semaine écoulée.
- Bilan : masquer la ligne « 0 sur 0 » quand aucun apport ; bouton grisé + hint.
- Progrès : dropdown machines trié (utilisées d'abord) ; garder toutes les autres
  en fin de liste.

## Tests (étape finale)
Matrice automatisée (browser + evaluate, serveur local) : profils genoux+asthme,
dos, épaules, poignets, diabète, hypertension, grossesse, profil neutre.
Assertions : recProg() diverge entre profils ; aucun défi tagué à risque n'est
retenu pour le profil concerné (10 tirages) ; badges visibles sur la vue Séance ;
cycle cohérent (texte = calendrier) ; pas de NaN. + parcours visuel avec captures.

## Contraintes respectées
Fichier unique, CSP inchangée, zéro backend, dark mode (pas de nouvelle couleur
forte ; badges réutilisent les tokens existants), cibles tactiles, français/tutoiement,
cache SW incrémenté à chaque mise en ligne.
