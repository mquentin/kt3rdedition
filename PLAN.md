# Kill Team One-Pager — Build Plan

Source pattern: `https://kt.wargamebuilder.com/fr/killteam/view/[ID]`

---

## Step 1 — Fetch the page

Use the Agent tool (general-purpose) to fetch the URL and extract **all visible text verbatim**:

- Kill team name, archetype (e.g. Recherche et destruction / Infiltration)
- Faction keywords (e.g. CHAOS · HERETICUS ASTARTES · NIGHT LORDS)
- Composition rules (how many agents, which ones are unique, which are unlimited)
- All faction rules (names + full descriptions)
- All operative profiles:
  - Name
  - Stats: LPA, M, SVG, PV
  - All weapon rows: name, A, T, D/DC, and the Règles column exactly as written
    - Note: only include range (e.g. `Portée 8"`) if it appears in the Règles column — do NOT invent ranges
    - Mark each weapon as TIR (ranged) or MEL (melee) based on whether it has a Portée rule
  - All abilities: name, cost (PA if any), PSYCHIQUE tag if present, full description verbatim
- All strategic ploys (Subterfuges Stratégiques): name, cost in PC, full description verbatim
- All firefight ploys (Subterfuges d'Affrontement): name, cost in PC, full description verbatim
- All faction equipment: name, full description verbatim

---

## Step 2 — Build the HTML file

Create a single self-contained `.html` file (no external dependencies).

### Layout structure (top to bottom)

```
Header bar
  └── Kill team name (h1) + archetype subtitle + faction keyword tags

Top row (2 columns)
  ├── Composition box
  └── Faction Rules (2-column grid, one cell per rule)

Ploys (2 columns side by side)
  ├── Subterfuges Stratégiques (left)
  └── Subterfuges d'Affrontement (right)

Operatives grid (auto-fill, ~340px min per card)
  └── One card per operative

Équipement de Faction (auto-fill grid)
  └── One cell per item

Footer
```

### Operative card structure (top to bottom)

```
Header row: Name | [Chef badge if leader]
Stat bar: LPA | M | SVG | PV
Weapon table: Arme | A | T | D/DC | Règles
Abilities (if any): Name [cost] [Psychique tag] + indented description
```

### Ploy row structure

```
[PC pip column] | [Name + indented description]
```

---

## Step 3 — Content rules (what to include / exclude)

**Include:**
- Full rule text verbatim (do not paraphrase)
- Weapon rules exactly as listed in the source Règles column
- `Portée X"` in the Règles column only if the source lists it
- TIR / MEL badge on each weapon name

**Exclude (redundant content):**
- `Night Lord` / faction suffix in each operative name — it's in the header
- `32 pts` per operative — all identical, not useful during play
- `×1` / `Illimité` limit badges — stated once in the Composition box
- Keywords rows at the bottom of each card — faction is obvious from context
- Outer section title "Subterfuges" — the column headers are sufficient
- Outer section title "Agents" — cards are self-evident

---

## Step 4 — Theme

Light theme:
- Background: `#f4f2ee` (page), `#ffffff` (cards), `#eeece8` (headers/bars)
- Borders: `#d4d0cb`
- Text: `#1c1a16` (main), `#6a6258` (dim/secondary)
- Accent (purple): `#2e1a5e` (dark), `#5c3a96` (mid)
- Strategy ploys: blue tint (`#1a3464` / `#e6ecf6`)
- Firefight ploys: red tint (`#6e1a1a` / `#f6e6e6`)
- Leader card: gold tint (`#6a4e08` / `#fdf4dc` / `#c8a020`)
- TIR badge: blue-on-blue-bg
- MEL badge: red-on-red-bg
- Ability names: mid purple
- Equipment names: gold

---

## Step 5 — Print (landscape)

Add at the end of the `<style>` block:

```css
@page { size: landscape; margin: 8mm 8mm; }

@media print {
  * { -webkit-print-color-adjust: exact; print-color-adjust: exact; }
  body { font-size: 9px; background: #fff; }
  .page { padding: 6px 0 0; max-width: 100%; }
  /* Tighter spacing on all text elements */
  /* Operatives grid: repeat(4, 1fr) */
  /* Equipment grid: repeat(4, 1fr) */
  /* break-inside: avoid on .op, .ploy-item, .eq, .rule-cell */
}
```

---

## CSS class reference

| Class | Role |
|---|---|
| `header` | Top bar (accent bg) |
| `top-row` | 2-col grid for composition + rules |
| `rules-grid` | 2-col grid inside faction rules |
| `ploys-row` | 2-col grid for strategy vs firefight |
| `ploy-item.strat` / `.fight` | Blue or red left pip |
| `ops-grid` | Auto-fill card grid |
| `op` / `op.leader` | Operative card (gold border for leader) |
| `stat-bar` | 4-col stat strip |
| `wt` | Weapon table |
| `.wn` | Weapon name cell |
| `.wr` | Weapon rules cell (italic, dim) |
| `.rt.rt-r` / `.rt.rt-m` | TIR / MEL inline badge |
| `.abilities` / `.ab` | Ability block |
| `.ab-head` | Ability name + cost + tag row |
| `.ab-name` | Ability title (purple) |
| `.ab-cost` | AP cost pill |
| `.ab-tag` | PSYCHIQUE green pill |
| `equip-grid` / `.eq` | Equipment 4-col grid |
| `.keywords` | Hidden (`display: none`) |
