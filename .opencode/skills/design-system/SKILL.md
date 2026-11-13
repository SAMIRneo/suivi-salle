---
name: Design System
description: Appliquer et étendre le design system de Suivi salle — tokens de couleur, dark mode, typographie, composants (cards, boutons, inputs). À utiliser pour toute nouvelle interface, section ou retouche visuelle.
---

# Design System Suivi salle

## Palette (tokens existants — réutiliser, ne jamais coder en dur)

| Rôle | Light | Dark |
| --- | --- | --- |
| Fond | `--bg:#eef2f7` | `#0b1120` |
| Carte | `--card:#ffffff` | `#131c2e` |
| Texte | `--ink:#0f172a` | `#e6edf5` |
| Texte secondaire | `--mute:#64748b` | `#8b9ab0` |
| Bordure | `--line:#e2e8f0` | `#1f2a3d` |
| Accent principal | `--acc:#ff5a1f` | `#ff7340` |

## Couleurs fonctionnelles par section

- `--good` (vert) : nutrition · `--prot` (bleu) : progrès/programmes
- `--gluc` (jaune), `--lip` (rose) : macros · `--cardio` (cyan) : cardio
- `--corps` (violet) : poids/mensurations/cycle

Chaque couleur a un jumeau `-soft` (fond translucide ~14-20%). Nouvelle section :
lui assigner une `--sec` existante, pas de nouvelle teinte sans nécessité.

## Règles de composants

- **Carte** : `.card` = fond `--card`, bordure `--line`, rayon 16px, ombre
  `--shadow`, barre colorée 3px en haut via `::before` avec `--sec`.
- **Boutons** : `.btn` plein accent (rayon 12px, min-height 50px, ombre douce) ;
  `.btn.alt` variante neutre bordée. Feedback `:active{transform:scale(.97)}`.
- **Inputs** : fond `--soft`, bordure 1.5px `--line`, rayon 12px, focus → bordure
  `--sec`. Jamais de `outline:none` sans focus-visible visible.
- **Titres de section** : `h2` en 11px uppercase, letter-spacing .14em, couleur
  `--sec`, carré 10px devant.
- **Titres h1** : 24px, font-weight 800, letter-spacing -.4px.
- **Animations** : entrées de cartes en cascade 40ms avec `--ease-out`. Préférer
  transform/opacity. Respecter `prefers-reduced-motion` pour tout nouveau mouvement.
- **Focus** : `:focus-visible` outline 3px `--acc` offset 2px — obligatoire.

## Checklist avant de livrer une UI

1. Variables uniquement (aucune valeur hex en dur dans les nouveaux blocs).
2. Dark mode vérifié sur la zone modifiée.
3. Contrastes AA : texte `--mute` sur `--soft` ok ; tester toute nouvelle couleur.
4. Zone tactile ≥ 44px, marges ≥ 8px entre cibles.
5. Cohérence avec les sections existantes (même structure card > h2 > contenu).
