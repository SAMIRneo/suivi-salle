---
name: UX Writing
description: Revoir et écrire les textes de l'interface Suivi salle (libellés, hints, placeholders, messages d'erreur) en français, ton tutoiement motivant. À utiliser pour toute modification de texte visible.
---

# UX Writing Suivi salle

## Voix et ton

- **Tutoiement** systématique : « tes charges », « ta séance », « ton progrès ».
- **Motivant mais factuel** : pas de blabla fitness marketing, pas de jugement.
  Encourager sur l'effort et la régularité, jamais sur le corps ou le poids.
- **Concis** : les labels font 2-4 mots, les hints max 2 lignes mobile.
- **Zéro jargon non expliqué** : si un terme technique apparaît (lipides, PPG,
  1RM), il est soit standard, soit immédiatement expliqué dans un `.hint`.

## Conventions

- Unités FR avec espace insécable avant l'unité : `60 kg`, `12 rép.`.
- Titres de sections en snippets courts style carte d'entraînement
  (« Cardio », « Nutrition », « Suivi du cycle »), pas de phrases.
- Messages de confirmation : dire ce qui a été enregistré (« Séance enregistrée »)
  et où la retrouver si utile.
- Messages d'erreur : cause + action (« Champs incomplets — renseigne le poids
  et les répétitions »). Jamais de ton alarmiste.
- Empty states : expliquer pourquoi c'est vide et le premier pas
  (« Aucun programme — crée ton premier programme pour commencer »).

## Champs de données sensibles

- Poids, mensurations, cycle : formulation neutre et privée. Rappeler la
  promesse de confidentialité si le contexte s'y prête (données 100 % locales).

## Checklist

1. Aucune majuscule en milieu de phrase inutile, typographie FR correcte (apostrophes typographiques ok).
2. Boutons à l'infinitif ou à la 2e personne ("Enregistrer", "Ajouter une séance").
3. Cohérence des termes sur toute l'app (toujours « séance », jamais « workout » / « entraînement » mélangés).
4. Tous les nouveaux textes relisent en dark + light, mobile width.
