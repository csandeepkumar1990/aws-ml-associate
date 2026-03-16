# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file static HTML study notes page for the **AWS ML Associate (MLA-C01)** exam. Covers PT-1 through PT-6 (390 questions by Marlon Anesi / Abhishek Singh / Stephane Maarek). Styled as a **digital handwritten notebook** — inspired by the user's own hand-drawn study notes.

## Running the App

No build step or dependencies. Open directly in a browser:

```
open index.html
```

## Design System

The visual style is a **handwritten notebook aesthetic**:

- **Font**: `Caveat` (Google Fonts) — handwriting style throughout
- **Background**: `#faf8f3` (cream notebook paper) with CSS `repeating-linear-gradient` ruled lines via `body::before`
- **Red margin line**: on every card via `.note::before` pseudo-element at `left: 40px`
- **Dark mode**: `html[data-theme="dark"]` attribute selector; light is default; toggle button + `localStorage` persistence

### CSS Variables

```
:root (light default):
  --paper, --card           (backgrounds)
  --ink                     (#1a1a2e dark ink)
  --red                     (#c62828 accent/titles)
  --blue                    (#1565c0 secondary)
  --green                   (#2e7d32 correct/output)
  --pencil                  (#666 muted text)
  --line                    (notebook ruled lines)
  --margin-line             (red margin line on cards)
  --border                  (#e8e3d8)

html[data-theme="dark"]:   overrides all vars for dark mode
```

## Architecture

Everything lives in `index.html`:
- **CSS** — `:root` vars, dark theme, notebook aesthetic, card components
- **HTML** — header, sticky search bar, filter chips, `.notes-grid` of `.note` cards
- **JS** — theme toggle, filter by tag, search with highlight/restore

### Note Card Structure

Each concept is a `.note` div:

```html
<div class="note" data-tags="sagemaker deployment" data-text="searchable plain text keywords">
  <div class="note-title"><span class="note-icon">EMOJI</span> Card Title</div>
  <!-- content: cmp-grid, flow, note-list, or combination -->
</div>
```

**`data-tags`** — space-separated, matches filter chips: `sagemaker`, `mlconcepts`, `data`, `deployment`, `security`, `monitoring`, `aiservices`, `genai`

**`data-text`** — plain text keywords for search (no HTML). Always include synonyms and exam keywords.

### Visual Components

**Comparison grid** (the `+` cross from handwritten notes):
```html
<div class="cmp-grid">
  <div class="cmp-cell red">Top Left Header</div>   <!-- red, border-right + border-bottom -->
  <div class="cmp-cell blue">Top Right Header</div>  <!-- blue, border-bottom -->
  <div class="cmp-cell blue-val">Bottom Left</div>   <!-- blue, border-right -->
  <div class="cmp-cell red-val">Bottom Right</div>   <!-- ink color -->
</div>
```

**Flow diagram** (boxes with arrows):
```html
<div class="flow">
  <div class="flow-box red">input</div>
  <span class="arrow">→</span>
  <div class="flow-box blue">process</div>
  <span class="arrow">→</span>
  <div class="flow-box green">output</div>
</div>
```
Box colors: `.red` (input), `.blue` (process), `.green` (correct output), `.ink` (neutral)

**Circle term** (for circled key concepts from notes):
```html
<span class="circle">SMOTE</span>
<span class="circle blue">hyperparam space</span>
```

**Bullet list** (→ bullets, with ✓/✗ variants):
```html
<ul class="note-list">
  <li>normal item with → bullet</li>
  <li class="yes">correct answer with ✓ bullet (green)</li>
  <li class="no">wrong/exception with ✗ bullet (red)</li>
</ul>
```

**Text helpers**: `.r` (red bold), `.b` (blue bold), `.g` (green bold), `.gy` (pencil/gray), `.u` (underline)

**Divider**: `<div class="rule"></div>`

**Sub-label**: `<div class="sub">Section Label</div>` (monospace uppercase)

### JS Patterns

- `toggleTheme()` — switches `data-theme` attr, persists to `localStorage`
- `setFilter(tag, btn)` — sets `activeTag`, re-runs `applyFilters()`
- `applyFilters()` — restores `data-orig` HTML, then hides/shows cards, then highlights matches with `<mark class="hl">`
- `clearSearch()` — clears input, re-runs `applyFilters()`
- `DOMContentLoaded` — stores initial `innerHTML` as `data-orig` on every `.note` for safe highlight/restore

## Adding Content

**Add a note card**: Add a new `.note` div inside `.notes-grid` before the closing `</div>`:

```html
<div class="note" data-tags="TAGS" data-text="keywords for search">
  <div class="note-title"><span class="note-icon">📌</span> Topic Name</div>
  <!-- use cmp-grid, flow, note-list as needed -->
</div>
```

**Do NOT add**:
- Analytics dashboards, charts, or data arrays
- The old `.kcard`, `.ctable`, `tableData`, `categories` structures
- Tab panels or navigation
- Any content that isn't a `.note` card

## Content Coverage

48 note cards covering:
- **SageMaker** (15 cards): Pipeline, Clarify, Model Monitor, Feature Importance Drift, Data Wrangler + Feature Store, JumpStart, Experiments + Trackers, Debugger, Profiler, Script Mode + BYOC, Inference Types, Warm Pools + Spot, HPO Methods, Studio vs Notebooks, Processing Jobs
- **ML Concepts** (13 cards): Hyperparams vs Model Params, Supervised Learning, Unsupervised vs RL, Overfitting vs Underfitting, Class Imbalance, L1 vs L2, Bias-Variance, Loss Curves, Model vs Data Parallelism, Boosting vs Bagging, Evaluation Metrics, Online Learning, Transfer Learning
- **Data Engineering** (6 cards): CloudWatch vs AWS Config, Kinesis Firehose, Kinesis Streams vs Analytics, AWS Glue, Parquet + Athena, S3 SSE Encryption
- **Deployment** (3 cards): Deployment Strategies, Multi-Model Endpoints, CloudFormation IaC
- **Security** (2 cards): IAM Roles + Trust Policy, VPC NACL vs Security Groups
- **Monitoring** (2 cards): Model Monitor Setup, Drift Types
- **AI Services** (4 cards): AI Service Selector, Transcribe + Translate + Polly, Textract + Comprehend + Kendra, Lookout Services
- **GenAI** (3 cards): Amazon Bedrock, RAG vs Fine-tuning, Grid Search HPO
