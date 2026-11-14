# RAPPORT D'AUDIT DE SIMULATION - Site Suivi bien-être

## Résumé exécutif

**Verdict** : Site accessible en apparence mais avec plusieurs risques majeurs pour l'inclusion de types d'utilisateurs variés.

**Atouts** :
- Interface française en tutoiement bienveillant
- Design mobile-first avec thèmes clair/sombre
- Stockage des données sur l'appareil (pas de collecte externe)
- Pas de doublons visibles dans l'interface
- Focus visible sur les inputs

**Plus gros risques** :
1. **Culpabilisation** à la moindre absence de saisie (personnes stress/burn-out)
2. **Risque troubles alimentaires** : suivi calorique/nutritionnel non encadré
3. **Accessibilité limitée** : screen readers, contraste some cases, cibles tactiles inégales
4. **Personnes peu numériques** : formulaires complexes, dépendance au clavier
5. **Données sensibles** stockées sans consentement explicite clair

**Actions prioritaires** (P0) :
1. Atténuer la culpabilisation à l'absence de saisie
2. Ajouter un consentement clair pour les données de santé
3. Améliorer l'accessibilité de base (contraste, focus)
4. Simplifier les formulaires pour les personnes peu habituées au numérique

---

## Méthode et limites

**Méthode** : Simulation IA avec 16 personas couvrant diversité maximale (âge, genre, capacité technique, état de santé, contexte vie). Pas de données utilisateurs réelles utilisées.

**Limites** :
- Pas de données utilisateurs réelles (simulation par IA)
- Site en code unique index.html (architecture limitée)
- Pas de backend ni de base de données externe
- Simulation sur 12 mois par jalons tous les 3 mois
- IA générant des scénarios plausibles mais non validés empiriquement

**Skills et agents utilisés** :
- `explore` : exploration codebase structure
- `general` : simulation personas et jalons
- `accessibility-audit` : vérifications WCAG de base
- `design-system` : compatibilité tokens CSS

---

## ✅ Points positifs à garder ou renforcer

- Interface française tutoiement bienveillant
- Design mobile-first qui fonctionne sur largeurs ≤ 360px
- Stockage des données sur l'appareil uniquement (pas de cloud)
- Thème clair/sombre fonctionnel
- Cibles tactiles ≥ 44px sur most éléments
- Pas de prompt/confirm natifs intrusifs (utilisation de toasts)
- Couleurs de tokens CSS cohérentes light/dark
- Structure d'onglets claire (6 onglets principaux)
- Pas de publicité, pas de tracking externe

---

## ❌ Points négatifs, classés

### Bloquant (empêche l'utilisation par certaines personas) :

1. **Culpabilisation systématique** - Quand l'utilisateur saute des jours, le site renvoie par défaut à "Aucune séance enregistrée" sans contexte bienveillant. Personnes concernées : burn-out, dépression, reprise après maladie. Preuve : code lignes 770-771 section Progrès, toast messages.

2. **Risque troubles alimentaires** - Section Nutrition avec suivi calorique et macronutriments sans avertissement, pour personne avec historique de TCA. Preuve : code Nutrition section, lignes 946-974.

3. **Accessibilité contrast** - Dans dark mode, certaines couleurs ont contraste < 4.5:1 (vérifié sur pgdone button, certains textes). Preuve : rapport designer initial, token --good-ink en dark.

4. **Formulaires complexes pour peu numérique** - L'onboarding nécessite de remplir 27 champs dont plusieurs optionnels, avec sélecteurs multiples. Preuve : code onboarding section, 27 champs demandés.

5. **Pas de consentement aux données de santé** - Cycle menstruel, suivi poids/humeur stockés sans écran de consentement explicite. Preuve : code section Cycle, stockage localStorage sans mention.

### Majeur (gêne importante mais utilisable) :

6. **Surcharge à l'onboarding** - 27 champs dont beaucoup restent vides ("Laisse vide ce que tu ne sais pas encore"), mais l'utilisateur doit tout de même défiler et sélectionner. Personnes : personnes âgées, peu numériques, ado. Preuve : code onboarding, 27 champs.

7. **Navigation cachée** - Certains segments (Corps → cycle, nutrition avancée) sont cachés derrière des sous-segments, difficiles à découvrir. Preuve : structure onglets, 5 onglets principaux avec sous-sections cachées.

### Mineur (amelioration) :

8. **Variabilité des cibles tactiles** - Certains boutons 40px, d'autres 44px. Inconstance. Preuve : code CSS, certaines règles 40px, d'autres 44px.

9. **Focus clavier** - `outline:none` sur `input:focus` écrase `focus-visible` dans certains cas, rendant la navigation au clavier juste suffisante en AA. Preuve : code CSS focus rules.

---

## 🔧 À améliorer

1. **Texte de contexte après saut** - Au lieu de "Aucune séance enregistrée", proposer "Tu n'as pas encore enregistré de séance. C'est normal pour démarrer !" avec bouton "Commencer une séance".

2. **Seuils de rappel doux** - Option pour ne pas recevoir de notification de "rappel" après 3 jours d'absence, ou avoir un message "Pas de souci, on reparte quand tu veux".

3. **Mode dégradé connexion lente** - Vérifier que les SVG de courbes et les images optionnelles se dégradent gracieusement sans afficher d'erreurs ou de contenu inutile.

4. **Séparation des données sensibles** - Cycle menstruel et données de poids pourraient être dans un sous-ensemble verrouillé ou avec mot de passe secondaire.

5. **Aide contextuelle progressive** - Au lieu de tout montrer à l'onboarding, afficher l'aide au moment de l'utilisation (juste-à-temps).

---

## ➕ Ce qui manque

1. **Mode "débutant absolu"** - Qui simplifie l'interface au premier lancement et montre seulement l'essentiel.
2. **Mode "accessibilité"** - Dans les réglages, option pour augmenter tout, désactiver les animations, mode daltonisme basique.
3. **Export/import de préférences** - Sauvegarder ses préférences (thème, cibles 44px, réduire les notifications) pour les restaurer sur un nouveau téléphone.
4. **Mode "partenaire/santé"** - Option de partager certaines données (only avec consentement) avec un professionnel de santé, mais pas de diagnostic.
5. **Notifications opt-in** - Demander la permission pour les rappels doux plutôt que par défaut.

---

## Risques sensibles

| Risque | Niveau | Description |
|--------|--------|-------------|
| Santé mentale | ÉlevÉ | Culpabilisation utilisateur qui saute des jours, streaks qui s'arrêtent |
| Vie privée données santé | Moyen | Données cycle, poids, humeur stockées en localStorage, aucune politique de confidentialité visible |
| Accessibilité screen reader | Moyen | Code complexe, titres d'en-tête pas toujours sémantiques, éléments dynamiques mal annoncés |
| Sécurité données | Élevé | Si l'appareil est partagé, les données d'un utilisateur sont visibles par le suivant (pas de verrouillage par mot de passe sur l'app lui-même) |
| Troubles alimentaires | ÉlevÉ | Section nutrition sans avertissement, suivi calorique pour personne TCA |

---

## Rétention : où et pourquoi chaque persona décroche

| Persona | M1 (1 mois) | M3 (3 mois) | M6 (6 mois) | M12 (12 mois) |
|---------|-------------|-------------|-------------|----------------|
| Débutant complet | Continue (motivation initiale) | Ralentit (formulaire complexe) | Risque abandon (25%) | Risque abandon (40%) |
| Sportif confirmé | Continue | Continue | Continue | Continue (80%) |
| Senior | Continue (première semaine) | Ralentit (techniques) | Abandon (60%) | Abandon (80%) |
| Surpoids/TCA | Ralentit (semaine 2) | Risque retour TCA (30%) | Risque rechute (50%) | Risque rechute chronique (70%) |
| Burn-out/stress | Abandon semaine 1 (40%) | N/A | N/A | N/A |
| Enceinte/post-partum | Continue (premier trimestre) | Changement besoins (50%) | N/A | N/A |
| Très pressée | Abandon semaine 1 (50%) | N/A | N/A | N/A |
| Voyage/changement fuseau | Continu avec adaptation | Ralentit (connexion lente) | S'adapte ou abandonne | S'abandonne |
| Handicap (mobilité) | Continue (si adaptatif) | Ralentit | Abandon (70%) | Abandon (85%) |
| Peu numérique | Abandon semaine 1 (60%) | N/A | N/A | N/A |
| Non francophone | Continue (if French UI) | Ralentit (vocabulaire) | Abandon (40%) | Abandon (60%) |
| Vieux téléphone/connexion lente | Continue (lent) | Ralentit fortement | Abandon (50%) | Abandon (70%) |
| Saisie aléatoire | Ralentit semaine 2 | Abandon semaine 3 | N/A | N/A |
| Coach gérant salle | Continue | Continue | Continue | Continue |

---

## Plan priorisé

| ID | Problème | Action | Personas | Impact | Effort | Priorité | Skill utile |
|----|----------|--------|----------|--------|--------|----------|-------------|
| P01 | Culpabilisation à l'absence | Message de contexte bienveillant + option "reprendre quand prêt" | Burn-out, senior, débutant | Élevé | 1 journée | P0 | ask-matt, humanizer |
| P02 | Risque troubles alimentaires | Ajouter avertissement + option "masquer apport calorique" | TCA, surpoids | Élevé | 1 journée | P0 | copywriting, humanizer |
| P03 | Accessibilité contrast | Corriger ratios de contraste dark mode | Toutes personas | Moyen | 2 journées | P1 | design-system |
| P04 | Surcharge onboarding | Réduire à 8 champs essentiels + aide progressive | Peu numérique, senior, ado | Élevé | 1 journée | P0 | ask-matt, mobile-native |
| P05 | Pas de consentement données santé | Écran de consentement explicite pour cycle/poids | Toutes personas | Moyen | 1 journée | P1 | copywriting, seo-audit |
| P05 | Focus clavier incohérent | Harmoniser `input:focus` et `input:focus-visible` | Toutes personas | Faible | 1 journée | P2 | code-review |
| P06 | Séparation données sensibles | Option mot de passe secondaire pour cycle/poids | Toutes personas | Moyen | 2 journées | P1 | code-review |
| P07 | Mode dégradé connexion | Vérifier que tout fonctionne sans SVG/images lourdes | Vieux téléphone, connexion lente | Faible | 1 journée | P3 | research |

**Quick wins (≤ 1 journée)** :
- P01, P04, P05 (partiel), P02 (avertissement seulement)
- Enlever le `outline:none` qui écrase `focus-visible` en priorité

**Feuille de route** :
- **30 jours** : P01, P04, P02 avertissement, P05 consentement basique
- **90 jours** : P03 contraste, P04 onboarding simplifié, P03 focus-visible
- **365 jours** : P06 séparation données, mode accessibilité, mode débutant, mode dégradé

---

## À valider avec de vrais utilisateurs

Les 5 hypothèses les plus risquées à tester :

1. **H1** : Le message de contexte bienveillant après saut de séance réduit l'abandon de 30% chez les utilisateurs en burn-out ( tester avec 5 utilisateurs en congé maladie/reprise )
2. **H2** : L'avertissement "troubles alimentaires" avant section nutrition empêche 80% des utilisateurs TCA de se sentir déclenchés ( tester avec groupe de pairs TCA )
3. **H3** : L'onboarding réduit à 8 champs essentiels n'augmente pas le taux d'abandon de 40% actuel et augmente la complétion de 25% ( tester A/B avec 10 utilisateurs )
4. **H4** : L'option "masquer apport calorique" est utilisée par 60% des utilisateurs qui l'active, sans effet négatif sur l'usage global de l'app ( tester sur 20 utilisateurs )
5. **H5** : Le mode accèsibilité (agrandir tout, réduire animations) augmente la satisfaction de 40% des utilisateurs seniors ou en situation de handicap ( tester sur 8 utilisateurs )

**Métriques à instrumenter (si backend ajouté un jour)** :
- Taux d'abandon par étape (onboarding → première saisie → séance première fois → M1 → M3 → M6 → M12)
- Fréquence de saut de journée et message suivant consulté
- Utilisation de l'option "masquer calorique"
- Taux de consentement aux données sensibles
- Sessions par semaine moyennes par persona
- Temps passé sur écran par séance

---

## ANNEXE-SIMULATIONS.md

*Voir le fichier joint pour les détails des 16 personas et leur simulation sur 12 mois.*