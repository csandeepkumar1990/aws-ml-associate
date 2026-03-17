# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file static HTML study notes page for the **AWS ML Associate (MLA-C01)** exam.
Styled as a **digital handwritten notebook** — inspired by the user's own hand-drawn study notes.

## Running the App

No build step or dependencies. Open directly in a browser:

```
open index.html
```

## Design System

### Font & Background
- **Font**: `Patrick Hand` (Google Fonts) — clean, upright handwriting. NOT Caveat, NOT Kalam.
- **Background**: `#faf8f3` (cream notebook paper) with CSS `repeating-linear-gradient` ruled lines via `body::before`
- **Red margin line**: on every card via `.note::before` at `left: 40px`
- **Dark mode**: `html[data-theme="dark"]` selector; toggle button + `localStorage` persistence

### CSS Variables

```
:root (light default):
  --paper, --card           (backgrounds)
  --ink                     (#1a1a2e dark ink)
  --red                     (#c62828 accent/titles/headers)
  --blue                    (#1565c0 secondary)
  --green                   (#2e7d32 correct/output)
  --pencil                  (#666 muted text)
  --line                    (notebook ruled lines)
  --margin-line             (red margin line on cards)
  --border                  (#e8e3d8)
```

## Architecture

Everything lives in `index.html`:
- **CSS** — `:root` vars, dark theme, notebook aesthetic, all card components
- **HTML** — header, sticky search bar, filter chips, `.notes-grid` of `.note` cards
- **JS** — theme toggle, filter by tag, search with highlight/restore, auto tag badge injection

## Note Card Structure

```html
<div class="note" data-tags="sagemaker deployment" data-text="searchable plain text keywords">
  <div class="note-title"><span class="note-icon">EMOJI</span> Card Title</div>
  <!-- content: cmp-grid, flow, circle pills, colored tiles — NO bullet lists as primary content -->
</div>
```

**`data-tags`** — space-separated tags matching filter chips:
`sagemaker`, `mlconcepts`, `data`, `deployment`, `security`, `monitoring`, `aiservices`, `genai`, `pipeline`, `training`, `inference`

Author/source tags (auto-displayed as badges): `nikolaischuler`, `pt1`–`pt6`, `marlonAnesi`, `abhisheksingh`, `stephanemaarek`

**`data-text`** — plain text keywords for search (no HTML). Include synonyms and exam keywords.

## Visual Components (ALWAYS use these, NEVER plain bullet lists as primary content)

### Comparison grid (2×2 cross)
```html
<div class="cmp-grid">
  <div class="cmp-cell red">Left Header</div>    <!-- bold red, border-right + border-bottom -->
  <div class="cmp-cell blue">Right Header</div>  <!-- bold red (same color), border-bottom -->
  <div class="cmp-cell blue-val">Bottom Left</div>
  <div class="cmp-cell red-val">Bottom Right</div>
</div>
```
**Both `.cmp-cell.red` and `.cmp-cell.blue` render in `var(--red)`** — same color for all column headers.

### Flow diagram
```html
<div class="flow">
  <div class="flow-box red">input</div>
  <span class="arrow">→</span>
  <div class="flow-box blue">process</div>
  <span class="arrow">→</span>
  <div class="flow-box green">output</div>
</div>
```
Box variants: `.red` (input/trigger), `.blue` (process/service), `.green` (output/success), `.ink` (neutral)

### Circle pill (key term highlight)
```html
<span class="circle">SMOTE</span>
<span class="circle blue">hyperparam space</span>
```

### Colored tiles (grouped concepts)
```html
<div style="background:rgba(198,40,40,0.07);border-radius:8px;padding:8px">red tile</div>
<div style="background:rgba(21,101,192,0.07);border-radius:8px;padding:8px">blue tile</div>
<div style="background:rgba(46,125,50,0.07);border-radius:8px;padding:8px">green tile</div>
<div style="background:rgba(100,100,100,0.07);border-radius:8px;padding:8px">grey tile</div>
```

### Q-labeled exam cards (for PT question cards)
```html
<div class="note" data-tags="nikolaischuler pt1 sagemaker" data-text="keywords">
  <div class="note-title"><span class="r" style="font-size:14px;margin-right:6px">Q1.</span> Topic Name</div>
  <!-- diagrammatic content -->
  <p style="margin:8px 0 0;color:var(--green);font-size:14px">✓ Answer: ...</p>
</div>
```

### Text helpers
`.r` (red bold), `.b` (blue bold), `.g` (green bold), `.gy` (pencil/gray), `.u` (underline)

### Sub-label
`<div class="sub">Section Label</div>` (monospace uppercase)

## Auto Tag Badges

Every card automatically gets a greyed-out badge strip at the bottom (injected by JS on `DOMContentLoaded`) showing all its `data-tags` as readable pills. No manual markup needed. `tagLabels` maps tag keys to display names.

## Filter Chips

Current chips: `All`, `SageMaker`, `ML Concepts`, `Data`, `Deployment`, `Security`, `Monitoring`, `AI Services`, `GenAI`, `PT-1`, `Nikolai Schuler`

## JS Patterns

- `toggleTheme()` — switches `data-theme`, persists to `localStorage`
- `setFilter(tag, btn)` — sets `activeTag`, re-runs `applyFilters()`
- `applyFilters()` — restores `data-orig`, hides/shows cards, highlights matches with `<mark class="hl">`
- `clearSearch()` — clears input, re-runs `applyFilters()`
- `DOMContentLoaded` — injects `.card-meta` badges from `data-tags`, then stores `innerHTML` as `data-orig`

## Content Coverage

**Original cards (~58):**
- SageMaker (15): Pipeline, Clarify, Model Monitor, Feature Importance Drift, Data Wrangler + Feature Store, JumpStart, Experiments + Trackers, Debugger, Profiler, Script Mode + BYOC, Inference Types, Warm Pools + Spot, HPO Methods, Studio vs Notebooks, Processing Jobs
- ML Concepts (13): Hyperparams vs Model Params, Supervised Learning, Unsupervised vs RL, Overfitting vs Underfitting, Class Imbalance, L1 vs L2, Bias-Variance, Loss Curves, Model vs Data Parallelism, Boosting vs Bagging, Evaluation Metrics, Online Learning, Transfer Learning
- Data Engineering (6): CloudWatch vs AWS Config, Kinesis Firehose, Kinesis Streams vs Analytics, AWS Glue, Parquet + Athena, S3 SSE Encryption
- Deployment (3): Deployment Strategies, Multi-Model Endpoints, CloudFormation IaC
- Security (2): IAM Roles + Trust Policy, VPC NACL vs Security Groups
- Monitoring (2): Model Monitor Setup, Drift Types
- AI Services (4): AI Service Selector, Transcribe + Translate + Polly, Textract + Comprehend + Kendra, Lookout Services
- GenAI (3): Amazon Bedrock, RAG vs Fine-tuning, Grid Search HPO
- Additional (5): SNS+SQS routing, Endpoint Auto-Scaling, Data Wrangler Viz, Built-in Algorithms, Comprehend

**Nikolai Schuler PT-1 (65 cards, Q1–Q65):**
Tags: `nikolaischuler pt1` + topic tag. Covers all 65 exam questions with diagrammatic cards.

## Rules

- **NEVER** use `<ul class="note-list">` as the primary card content — use flows/grids/circles/tiles
- **NEVER** add analytics dashboards, charts, data arrays, tab panels, or the old `.kcard`/`.ctable` structures
- **ALWAYS** add `data-tags` and `data-text` to every new card
- **ALWAYS** use diagrammatic components — the goal is visual, notebook-style learning aids
- When adding exam question cards, use the Q-label format and include `✓ Answer:` indicator
