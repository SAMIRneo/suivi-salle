# SIMULATION D'UTILISATION — 1 AN (persona « Léa »)

> Différent du `RAPPORT.md` (audit multi-personas théorique) : ici, une vraie
> simulation d'utilisation avec un an de données injectées et un parcours UI complet.

## Persona

**Léa, 25 ans** — femme, 165 cm, 68 kg, objectif « Être en forme », 1-2 séances/semaine,
débutante, ≈45 min par séance, s'entraîne **en salle ET à la maison**.
- **Asthme** (+ précision « Genoux : douleurs à l'effort, suivi kiné »)
- Douleurs : **genoux** (pain = genoux)
- Suivi du cycle activé

Compte de test : `lea` / `simu2026` (localStorage localhost:8471, clé `suivi-salle-v1:lea`).

## Données injectées (52 semaines)

| Donnée | Volume | Notes |
|---|---|---|
| Séances | 350 (91 jours) | salle + maison, progression 40 → 90 kg hip thrust |
| Cardio | 49 | tapis, corde à sauter, marche d'été |
| Pesées | 13 | 68 → 66 kg |
| Tests | 14 | hip thrust max, squat au poids du corps |
| Cycle | 11 départs | cycles 28-30 j, derniers 28/07, 25/08, 23/09 |
| Nutrition | jours notés | ~1930 kcal/j cohérent |
| Journal | 10 notes | pause genoux, asthme, records, vacances |

Événements réalistes : **pause genoux semaines 13-15** (février), **vacances semaines
38-44** (été), reprise à -20 % de charges après la pause.

## Ce qui marche bien ✅

1. **Page Séance personnalisée** : messages asthme (échauffement progressif, traitement à
   proximité) et genoux (alternatives plus sûres) bien affichés.
2. **Progrès** : stats justes (« 90 kg record », « +50 kg depuis le début »), courbes
   propres, filtres par catégorie pertinents.
3. **Corps → Poids** : courbe lisible, la pause d'été est visible, −2,1 kg cohérent.
4. **Nutrition** : 1930 kcal/j, macros bien réparties, cohérent avec son profil.
5. **Navigation** : bug « l'onglet Tests copie l'onglet d'avant » corrigé (commit `85b7f39`,
   en ligne vérifié le 14/11/2026 : plus aucune réf `"perf"` dans `go()`).
6. **Bilan (journal)** : onglet câblé sur les apports Nutrition, aria-current correct,
   couleurs de la barre du bas correctes (les icônes emoji sont colorées par nature —
   pas un bug).
7. **Barre du bas** : 6 onglets (Séance, Tests, Progrès, Nutrition, Corps, Bilan) OK.

## Les trucs à changer 🔧 (par priorité)

### P1 — Cycle : deux calculs contradictoires + pas de message de retard
`cycInfo()` (ligne ~1238) part du **dernier** début de règles ; `cycDayInfo()`
(ligne ~1247) part du **premier** début + modulo 28. Résultat chez Léa (52 jours sans
règles depuis le 23/09) : le texte affiche « JOUR 23 DU CYCLE · Phase lutéale »
pendant que le calendrier colorie **aujourd'hui en rouge « règles »** — et annonce des
règles prévues « le 20 nov. » qui n'ont jamais été respectées.
**Aucun message « tes règles ont du retard »** alors que c'est l'info n°1 pour l'utilisatrice.
- Unifier les deux fonctions sur la même base (dernier début, pas de modulo sur 1 an).
- Dès ~35 jours sans règles, afficher une note bienveillante (« tes règles ont du
  retard par rapport à d'habitude — c'est fréquent, tu peux le noter »).

### P2 — Les douleurs genoux et l'asthme ne changent presque rien
- `recProg()` (lignes ~723-739) ne regarde que grossesse/fatigue : il recommande
  « Full body · Standard ≈50 min » à Léa alors qu'elle a choisi **45 min**, sans
  tenir compte de ses genoux.
- Les défis ne sont filtrés que pour le pain « dos » (ligne ~883) : Léa reçoit des
  défis **fentes 20/jambe, squats sautés** (« Maison · Cardio et HIIT »), pistol…
  exactement ce que son kiné lui interdit.
- L'asthme n'influence ni le cardio recommandé ni les défis.
- Faire de `recProg()` un trieur : score négatif aux programmes riches en impact pour
  les genoux (sauts, fentes profondes, pistol), filtrer les défis par pain **et** maladie.

### P3 — Onboarding : deux cartes avec le même titre
Lignes ~179-187 et ~188-197 : deux cartes « 🎯 Ta pratique (matériel, envies,
contraintes) » l'une au-dessus de l'autre. La première (tests de départ pompes/gainage/
squat/couché) est même **dépliée par défaut** alors que la seconde est la vraie
section pratique. Renommer la première « 📊 Tes points de départ (facultatif) » ou la
fusionner.

### P4 — Cardio : code non défensif + stat « 0 sauts » inutile
`renderCardio()` (ligne ~910) : `x.km`/`x.j` manquants → **NaN** (« NaN km », « NaN
sauts ») — reproduit avec des données sans `km` (cas réel : import, données anciennes).
Et une coureuse voit « 0 sauts » en permanence. Masquer les stats non pertinentes
(selon l'activité dominante de la semaine) et défauts `||0` dans le reduce.

### P5 — Bilan : affichage vide + bouton actif pour rien
« Jours avec apport enregistré : **0 sur 0** » et bouton « Enregistrer apport du jour »
actif alors qu'aucun apport n'est noté. Masquer la ligne quand il n'y a rien (comme
`renderPlan` le fait déjà avec `hidden`), et griser/expliquer le bouton si
`S.flog[today()]` est vide.

### P6 — Dropdown machines : 16 choix, 6 utilisés
Progrès → machine : la liste complète du genre `f` s'affiche même quand Léa n'a
enregistré que 6 machines → « Aucune donnée » partout. Trier/masquer : machines
utilisées d'abord (ou seule la liste utilisée + « autre »).

## Verdict

L'application est **solide pour le suivi pur** (courbes, stats, nutrition) et les
messages santé sont bien présents. Mais pour Léa — le cas d'usage que tu décris —
**l'app affiche ses limites avec méthode : elle ne s'adapte pas**. Les deux priorités
qui changeraient vraiment son quotidien sont le **P1 (cycle cohérent)** et le
**P2 (genoux + asthme qui influencent vraiment les recommandations et les défis)**.

## Méthode

- Serveur local : `python -m http.server 8471`, parcours piloté par le navigateur
  intégré (snapshots + captures), injection de données via `evaluate` sur les
  structures internes (`S.sessions`, `S.cardio`, `S.tests`, `S.body`, `S.mens`,
  `S.cyc`, `S.flog`, `S.journal`) puis `save()`.
- Chronologie corrigée après un premier essai (les 3 lots se chevauchaient sur 6 mois).
- Vérification live du fix navigation le 14/11/2026 : `go()` sans `"perf"`, TS
  identique au local.
