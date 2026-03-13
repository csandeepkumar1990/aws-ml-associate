# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file static HTML reference dashboard for the **AWS ML Associate (MLA-C01)** certification exam. It analyzes keywords and concepts from practice tests (PT-1 & PT-2, 130 questions by Marlon Anesi / Abhishek Singh / Stephane Maarek).

## Running the App

No build step or dependencies. Open directly in a browser:

```
open index.html
```

## Architecture

Everything lives in `index.html` — styles, data, and logic are all inline:

- **CSS** (`:root` CSS vars, dark theme) — lines ~9–156
- **HTML structure** — three tab panels: Analytics Dashboard, Keyword Cards, Quick Compare Tables
- **JS data + logic** — all inline `<script>` at the bottom of the file

### Tab Panels

| Panel ID | Content |
|---|---|
| `panel-analytics` | Stats, category grid, donut chart, frequency bars, bubble cloud, coverage table |
| `panel-keywords` | Searchable/filterable keyword cards (`.kcard` with `data-tags`) |
| `panel-compare` | Side-by-side comparison tables for similar AWS services |

### Key JS Patterns

- All keyword/category data is defined as JS arrays/objects inline in the `<script>` block
- `doFilter()` — filters `.kcard` elements by search text and `data-tags` attribute
- `setTag(tag, btn)` — category filter buttons
- `showTab(name, btn)` — tab switching
- Animated counters, SVG donut chart, and progress bars are all rendered from the data arrays on `DOMContentLoaded`

### CSS Variables (theme)

```
--bg, --surface, --card, --border       (backgrounds/borders)
--accent1 through --accent6             (colors per category)
--text, --muted, --glow                 (typography/effects)
```

## Modifying Content

- **Add a keyword card**: Add a new `.kcard` div inside `#kgrid` with appropriate `data-tags` matching one of the filter buttons (`pipeline`, `inference`, `training`, `data`, `mlconcepts`, `monitoring`, `security`, `genai`)
- **Add a category or data point**: Update the JS data arrays in the `<script>` block — the analytics panel renders dynamically from those arrays
- **Add a compare table**: Add a `.ctable` div inside the `.ctable-grid` in `panel-compare`
