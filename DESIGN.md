# Relay Design System

> **For AI agents:** Read this file in full before generating any Relay UI — landing page, dashboard, or worker config. Follow every rule exactly. Do not invent new colours, fonts, spacing values, or components.

---

## 1. Visual Theme & Atmosphere

**Mood:** Dark infrastructure platform — precise, data-forward, trustworthy. Relay sits at the intersection of publisher tools and AI infrastructure, so the UI must feel credible to both a newspaper CTO and a machine learning engineer. Think Cloudflare dashboard meets a premium SaaS — not a security ops centre.

**Key characteristics:**
- Near-black backgrounds with layered depth (dark → surface → card)
- Lime (`#a3e635`) as the single accent — CTAs, active states, positive signals, live indicators
- `JetBrains Mono` for all numbers, bytes, percentages, API keys, agent names, dates in tables
- `Inter` for all prose, labels, navigation, and headings
- Uppercase + tracked labels everywhere — section eyebrows, table headers, plan names, tier badges
- No gradients anywhere — depth comes from background layering only
- Glassmorphism only on the fixed nav (`backdrop-filter: blur(16px)`)
- Animate sparingly: spinner, the live-dot pulse on the hero badge, Chart.js transitions

**Relay vs SwarmHawk distinction:**
- SwarmHawk is a SOC tool — aggressive, dense, red/amber severity signals dominant
- Relay is a publisher platform — same dark foundation, but calmer; severity palette is used for agent tiers, not threat levels; marketing sections have larger type and more breathing room

**Do not:**
- Use white backgrounds, light cards, or any light-mode elements
- Add `box-shadow` to cards — use background layering for depth
- Use emojis anywhere — use inline SVG icons only
- Add gradients — none, ever, including on `featured` pricing cards
- Use border-radius above `16px` (modal maximum), above `12px` on cards, above `8px` on buttons/inputs
- Use more than 5 font sizes in a single component
- Use lime as body text colour — lime is for labels, badges, accents only
- Nest more than 2 card depth layers (`--surface` → `--card`)

---

## 2. Colour Palette

### CSS Custom Properties — always use these, never raw hex

```css
/* Base */
--dark:        #080810   /* page background                          */
--surface:     #0f0f1a   /* nav, inputs, modals, secondary panels    */
--card:        #13131f   /* card backgrounds, stat boxes             */
--border:      rgba(255,255,255,.07)  /* default border everywhere   */
--text:        #e8e8f0   /* primary body text                        */
--muted:       #8888a0   /* labels, meta, secondary descriptive text */

/* Accent */
--lime:        #a3e635   /* primary CTA, active states, positive     */
--lime-pale:   rgba(163,230,53,.12)  /* lime-tinted bg               */
--lime-border: rgba(163,230,53,.25)  /* lime borders                 */

/* Semantic */
--red:         #f87171   /* errors, blocked state, danger            */
--amber:       #fbbf24   /* warnings, caution                        */
--blue:        #60a5fa   /* info, AI inference agents, links         */
--purple:      #a78bfa   /* AI training agents, enterprise tier      */
--orange:      #fb923c   /* high-volume signals                      */

/* Layout */
--r:           12px      /* primary border radius (cards)            */
--nav-h:       60px      /* fixed nav height                         */
```

### Agent Tier Colour Map

Relay classifies AI agents by tier. Use these colours consistently across badges, charts, and table rows.

| Tier       | Colour   | Hex       | Background tint              | Border                         | Meaning                        |
|------------|----------|-----------|------------------------------|--------------------------------|--------------------------------|
| training   | purple   | `#a78bfa` | `rgba(167,139,250,.15)`      | `rgba(167,139,250,.3)`         | LLM training crawlers          |
| inference  | blue     | `#60a5fa` | `rgba(96,165,250,.12)`       | `rgba(96,165,250,.25)`         | Real-time agent/chat requests  |
| search     | amber    | `#fbbf24` | `rgba(251,191,36,.12)`       | `rgba(251,191,36,.25)`         | AI-powered search bots         |
| unknown    | muted    | `#8888a0` | `rgba(255,255,255,.07)`      | `rgba(255,255,255,.07)`        | Unclassified agent traffic     |
| blocked    | red      | `#f87171` | `rgba(248,113,113,.12)`      | `rgba(248,113,113,.25)`        | Requests blocked by policy     |

### Plan Colour Map

| Plan       | Colour   | Background tint              | Border                         |
|------------|----------|------------------------------|--------------------------------|
| free       | lime     | `var(--lime-pale)`           | `var(--lime-border)`           |
| growth     | blue     | `rgba(96,165,250,.12)`       | `rgba(96,165,250,.25)`         |
| scale      | purple   | `rgba(167,139,250,.12)`      | `rgba(167,139,250,.25)`        |
| enterprise | amber    | `rgba(251,191,36,.12)`       | `rgba(251,191,36,.25)`         |

### Semantic Colour Uses

| Colour   | Use cases                                                                 |
|----------|---------------------------------------------------------------------------|
| Lime     | Primary CTA buttons, active nav tab, positive/saved states, live dot      |
| Red      | Blocked policy state, errors, danger actions, `format_served: blocked`    |
| Amber    | Search-tier agents, warnings, Growth plan badge                           |
| Blue     | Inference-tier agents, info states, Growth plan, links                    |
| Purple   | Training-tier agents, Scale/Enterprise plan badges                        |
| Muted    | All labels, eyebrows, secondary text, unknown tier badges                 |

---

## 3. Typography

### Font Stack

```css
font-family: 'Inter', system-ui, sans-serif;      /* all UI text, headings, nav */
font-family: 'JetBrains Mono', monospace;         /* numbers, API keys, agent names, bytes, paths */
```

Google Fonts import (use this exact string):
```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet" />
```

### Size & Weight Hierarchy

#### Marketing / Landing Page

| Role                       | Size            | Weight | Family    | Transform  | Tracking   |
|----------------------------|-----------------|--------|-----------|------------|------------|
| Hero headline              | `clamp(36px,6vw,68px)` | 800 | Inter  | —          | `-2px`     |
| Section title              | `clamp(28px,4vw,44px)` | 800 | Inter  | —          | `-1px`     |
| Card / feature title       | `18–22px`       | 700    | Inter     | —          | —          |
| Body prose                 | `15–17px`       | 400    | Inter     | —          | —          |
| Hero eyebrow / badge label | `10px`          | 700    | JetBrains | uppercase  | `3px`      |
| Section eyebrow            | `10px`          | 700    | JetBrains | uppercase  | `3px`      |
| Stats bar number           | `34–36px`       | 800    | JetBrains | —          | —          |
| Pricing amount             | `40px`          | 800    | JetBrains | —          | —          |
| Plan name label            | `12–13px`       | 700    | JetBrains | uppercase  | `0.06em`   |
| Feature list item          | `13–14px`       | 400–500| Inter     | —          | —          |
| Footer / legal             | `12–13px`       | 400    | Inter     | —          | —          |

#### Dashboard / App

| Role                       | Size    | Weight | Family    | Transform  | Tracking   |
|----------------------------|---------|--------|-----------|------------|------------|
| Stat number (KPI cards)    | `28px`  | 700    | JetBrains | —          | tabular    |
| Page / modal title         | `18px`  | 600–700| Inter     | —          | —          |
| Card title                 | `13px`  | 600    | Inter     | uppercase  | `0.06em`   |
| Table row data             | `13px`  | 400    | Inter     | —          | —          |
| Monospace data (paths, keys) | `12–13px` | 400 | JetBrains | —        | —          |
| Labels / table headers     | `11px`  | 600    | Inter     | uppercase  | `0.08em`   |
| Nav brand                  | `16px`  | 700    | Inter     | —          | —          |
| Nav tab                    | `13px`  | 500    | Inter     | —          | —          |
| Badge / tier label         | `11px`  | 600    | JetBrains | uppercase  | `0.05em`   |
| Sub-label / stat note      | `12px`  | 400    | JetBrains | —          | —          |

### Rules
- All byte values, percentages, request counts, latency numbers → `JetBrains Mono`
- All agent names, page paths, API keys → `JetBrains Mono`
- All dates in tables → `JetBrains Mono`
- Section eyebrows/labels → `JetBrains Mono`, 10px, 700, uppercase, `letter-spacing: 3px`, `color: var(--lime)`
- `font-variant-numeric: tabular-nums` on all KPI and stat numbers
- Line heights: `1` for large Mono numbers, `1.2` for headings, `1.5–1.6` for body, `1.65–1.7` for marketing prose

---

## 4. Component Library

### 4.1 Buttons

```css
.btn {
  display: inline-flex; align-items: center; gap: 6px;
  border-radius: 8px; padding: 9px 18px;
  font-weight: 600; font-size: 13px;
  border: 1px solid transparent;
  transition: all .15s; cursor: pointer;
  font-family: 'Inter', sans-serif;
}
.btn-lime    { background: var(--lime); color: #000; }
.btn-outline { border: 1px solid var(--border); color: var(--text); background: transparent; }
.btn-ghost   { background: rgba(255,255,255,.05); border: 1px solid var(--border); color: var(--text); }
.btn-danger  { background: rgba(248,113,113,.12); border: 1px solid rgba(248,113,113,.3); color: var(--red); }

/* Sizes */
.btn-sm  { padding: 6px 12px; font-size: 12px; }
.btn-xs  { padding: 4px 10px; font-size: 11px; border-radius: 6px; }
.btn-lg  { padding: 13px 28px; font-size: 15px; } /* marketing CTAs — still border-radius: 8px */

/* States */
.btn:disabled { opacity: .4; cursor: not-allowed; }
.btn-lime:hover { background: #b5f23d; color: #000; }
.btn-outline:hover { border-color: var(--lime-border); color: var(--lime); }
```

**Rules:**
- `btn-lime` text is always `#000` — never `#080810` or `var(--dark)`
- Never exceed `border-radius: 8px` on buttons (even `.btn-lg`)
- One `btn-lime` per screen section maximum — all others use `btn-outline` or `btn-ghost`

### 4.2 Cards

```css
.card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--r);  /* 12px */
  padding: 24px;
}
.card-title {
  font-size: 13px; font-weight: 600;
  letter-spacing: 0.06em; text-transform: uppercase;
  color: var(--muted); margin-bottom: 16px;
}
```

Alert variants (border only — background stays `var(--card)`):
```css
.card-alert-blocked  { border-color: rgba(248,113,113,.25); }
.card-alert-featured { border-color: var(--lime-border); }
```

**Rules:**
- No `background: white` inside cards
- No `box-shadow` on cards — depth is from background layering
- No gradients on cards, including featured/highlighted variants
- Sub-surfaces inside cards: `background: rgba(255,255,255,.03)` maximum

### 4.3 Inputs

```css
.input, .form-input {
  width: 100%; padding: 10px 14px;
  background: var(--surface); color: var(--text);
  border: 1px solid var(--border); border-radius: 8px;
  font-size: 14px; font-family: inherit;
  transition: border-color .15s; outline: none;
}
.input:focus, .form-input:focus { border-color: var(--lime-border); }
.input::placeholder { color: var(--muted); }
```

### 4.4 Agent Tier Badges

Used in analytics tables, agent breakdown, event logs.

```css
.tier-badge {
  display: inline-flex; align-items: center;
  padding: 2px 8px; border-radius: 4px;
  font-size: 11px; font-weight: 600;
  letter-spacing: 0.05em; text-transform: uppercase;
  font-family: 'JetBrains Mono', monospace;
}
.tier-training  { background: rgba(167,139,250,.15); color: #a78bfa; border: 1px solid rgba(167,139,250,.3); }
.tier-inference { background: rgba(96,165,250,.12);  color: #60a5fa; border: 1px solid rgba(96,165,250,.25); }
.tier-search    { background: rgba(251,191,36,.12);  color: #fbbf24; border: 1px solid rgba(251,191,36,.25); }
.tier-unknown   { background: rgba(255,255,255,.07); color: #8888a0; border: 1px solid rgba(255,255,255,.07); }
.tier-blocked   { background: rgba(248,113,113,.12); color: #f87171; border: 1px solid rgba(248,113,113,.25); }
```

### 4.5 Plan Badges

Used in nav, settings, billing cards.

```css
.plan-badge {
  padding: 3px 10px; border-radius: 20px;
  font-size: 11px; font-weight: 600;
  letter-spacing: 0.06em; text-transform: uppercase;
  font-family: 'JetBrains Mono', monospace;
}
.plan-free       { background: var(--lime-pale);          color: var(--lime);   border: 1px solid var(--lime-border); }
.plan-growth     { background: rgba(96,165,250,.12);       color: #60a5fa;       border: 1px solid rgba(96,165,250,.25); }
.plan-scale      { background: rgba(167,139,250,.12);      color: #a78bfa;       border: 1px solid rgba(167,139,250,.25); }
.plan-enterprise { background: rgba(251,191,36,.12);       color: #fbbf24;       border: 1px solid rgba(251,191,36,.25); }
```

### 4.6 Stats / KPI Cards (Dashboard)

```css
.stat-card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--r);
  padding: 20px;
}
.stat-label {
  font-size: 11px; font-weight: 600;
  letter-spacing: 0.08em; text-transform: uppercase;
  color: var(--muted); margin-bottom: 8px;
}
.stat-value {
  font-size: 28px; font-weight: 700;
  font-family: 'JetBrains Mono', monospace;
  line-height: 1; font-variant-numeric: tabular-nums;
}
.stat-sub {
  font-size: 12px; color: var(--muted); margin-top: 6px;
  font-family: 'JetBrains Mono', monospace;
}
/* Colour modifiers — only for the number itself */
.stat-value.lime   { color: var(--lime); }
.stat-value.blue   { color: var(--blue); }
.stat-value.amber  { color: var(--amber); }
.stat-value.purple { color: var(--purple); }
```

### 4.7 Data Tables (Dashboard)

```css
.data-table { width: 100%; border-collapse: collapse; font-size: 13px; }
.data-table th {
  padding: 8px 12px; text-align: left;
  font-size: 11px; font-weight: 600;
  letter-spacing: 0.08em; text-transform: uppercase;
  color: var(--muted); border-bottom: 1px solid var(--border);
}
.data-table td { padding: 12px; border-bottom: 1px solid var(--border); color: var(--text); }
.data-table tr:last-child td { border-bottom: none; }
.data-table tbody tr:hover { background: rgba(255,255,255,.02); }
.mono { font-family: 'JetBrains Mono', monospace; font-size: 12px; }
```

### 4.8 Toggles

```css
.toggle { position: relative; width: 40px; height: 22px; flex-shrink: 0; }
.toggle input { opacity: 0; width: 0; height: 0; }
.toggle-slider {
  position: absolute; inset: 0;
  background: var(--surface); border: 1px solid var(--border);
  border-radius: 22px; cursor: pointer; transition: all .2s;
}
.toggle-slider::before {
  content: '';
  position: absolute; width: 16px; height: 16px;
  left: 2px; top: 2px;
  background: var(--muted); border-radius: 50%; transition: all .2s;
}
.toggle input:checked + .toggle-slider { background: var(--lime-pale); border-color: var(--lime-border); }
.toggle input:checked + .toggle-slider::before { transform: translateX(18px); background: var(--lime); }
```

### 4.9 Code Blocks

Used for API keys, CLI commands, config snippets.

```css
.code-block {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: 8px; padding: 12px 14px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 12px; color: var(--lime);
  white-space: pre-wrap; word-break: break-all;
  position: relative;
}
```

### 4.10 Navigation

```css
/* Fixed top nav */
nav {
  position: fixed; top: 0; left: 0; right: 0;
  height: var(--nav-h);  /* 60px */
  background: rgba(8,8,16,.85);
  border-bottom: 1px solid var(--border);
  backdrop-filter: blur(16px);
  z-index: 300;
}

/* Nav tabs (dashboard) */
.nav-tab {
  padding: 6px 14px; border-radius: 6px;
  font-size: 13px; font-weight: 500;
  color: var(--muted); border: none; background: none;
  cursor: pointer; transition: all .15s;
}
.nav-tab:hover  { color: var(--text); background: rgba(255,255,255,.05); }
.nav-tab.active { color: var(--lime); background: var(--lime-pale); }
```

### 4.11 Modals / Overlays

```css
.modal-overlay {
  position: fixed; inset: 0;
  background: rgba(8,8,16,.9);
  backdrop-filter: blur(8px);
  z-index: 500;
  display: flex; align-items: center; justify-content: center;
}
.modal-box {
  background: var(--card); border: 1px solid var(--border);
  border-radius: 16px; padding: 40px;
  max-width: 520px; width: 90%;
}
```

`border-radius: 16px` is the maximum — only for modals/overlays.

### 4.12 Spinners

```css
.spinner {
  display: inline-block; width: 20px; height: 20px;
  border: 2px solid var(--border);
  border-top-color: var(--lime);
  border-radius: 50%;
  animation: spin .8s linear infinite;
}
@keyframes spin { to { transform: rotate(360deg); } }
```

### 4.13 Live Indicator Dot (Marketing / Status)

```css
.live-dot {
  width: 6px; height: 6px;
  border-radius: 50%; background: var(--lime);
  animation: pulse 2s ease-in-out infinite;
}
@keyframes pulse {
  0%, 100% { opacity: 1; }
  50%       { opacity: .3; }
}
```

### 4.14 Toasts

```css
.toast {
  padding: 10px 18px; border-radius: 8px;
  font-size: 13px; font-weight: 600; max-width: 320px;
}
.toast-success { background: rgba(163,230,53,.12);  color: var(--lime); border: 1px solid var(--lime-border); }
.toast-error   { background: rgba(248,113,113,.12); color: var(--red);  border: 1px solid rgba(248,113,113,.3); }
.toast-info    { background: rgba(96,165,250,.12);  color: var(--blue); border: 1px solid rgba(96,165,250,.25); }
```

### 4.15 FAQ Accordion

```css
.faq-item { background: var(--card); border: 1px solid var(--border); border-radius: 8px; overflow: hidden; }
.faq-q {
  padding: 20px 24px; font-weight: 600; color: var(--text);
  cursor: pointer; display: flex; align-items: center; justify-content: space-between;
  user-select: none;
}
.faq-q:hover { color: var(--lime); }
.faq-icon { color: var(--lime); font-size: 20px; transition: transform .2s; }
.faq-item.open .faq-icon { transform: rotate(45deg); }
.faq-a {
  max-height: 0; overflow: hidden; padding: 0 24px;
  font-size: 14px; color: var(--muted); line-height: 1.7;
  transition: max-height .3s ease, padding .3s ease;
}
.faq-item.open .faq-a { max-height: 400px; padding: 0 24px 20px; }
```

---

## 5. Layout

### Page Shell

```
Max-width:      1280px (centred, padding: 0 40px desktop, 0 20px mobile)
Nav height:     60px (fixed, z-index: 300)
Dashboard top:  calc(60px + 32px)
Section padding (marketing): 96px 40px desktop, 64px 20px mobile
```

### Grid Patterns

| Pattern        | CSS                                        | Use case                              |
|----------------|--------------------------------------------|---------------------------------------|
| 4-col stats    | `grid-template-columns: repeat(4, 1fr)`    | KPI cards row (dashboard)             |
| 4-col pricing  | `grid-template-columns: repeat(4, 1fr)`    | Pricing tier cards (marketing)        |
| 3-col segments | `grid-template-columns: repeat(3, 1fr)`    | Customer segment cards (marketing)    |
| 2-col equal    | `grid-template-columns: 1fr 1fr`           | Side-by-side (problem/solution, SOM)  |
| 3-col steps    | `grid-template-columns: repeat(3, 1fr)`    | How-it-works steps                    |
| Full width     | `width: 100%`                              | Tables, inputs, single-col sections   |

Gap standard: `16px` (dense / stat rows), `20px` (cards), `24px` (marketing sections).

Responsive collapses:
- `≤ 1100px`: 4-col pricing → 2-col
- `≤ 900px`: 3-col → 1-col, 2-col → 1-col, stats 4-col → 2-col
- `≤ 640px`: 2-col pricing → 1-col
- `≤ 480px`: nav links hidden, hero text scales down

### Spacing Scale

Use only these values:

```
4px   micro (badge padding, icon gaps)
6px   tight (button internals)
8px   element gap (icon + label, nav items)
10px  component padding small
12px  component padding medium, .btn-sm padding
14px  input padding, nav spacing
16px  card padding compact, stat card padding
20px  element gap standard, card header margin
24px  card padding, section gap
28px  page top clearance, modal padding
32px  dashboard top clearance, hero padding
40px  section horizontal padding (desktop)
48px  section vertical padding (mobile)
64px  section vertical padding (mobile large)
96px  section vertical padding (desktop)
```

### Elevation / z-index

```
9999  tooltips, toasts
500   modal overlay
300   fixed nav
200   mobile menu
```

---

## 6. Depth System

Depth via background layering — never shadows on cards.

| Layer   | Background                      | Used for                              |
|---------|---------------------------------|---------------------------------------|
| Base    | `#080810` (`--dark`)            | Page background                       |
| Surface | `#0f0f1a` (`--surface`)         | Nav, inputs, code blocks, modals      |
| Card    | `#13131f` (`--card`)            | All cards, stat boxes, panels         |
| Raised  | `rgba(255,255,255,.03–.06)`     | Hover rows, sub-panels inside cards   |

**Allowed blur effects:**
- Nav only: `backdrop-filter: blur(16px)`
- Modal overlay: `backdrop-filter: blur(8px)`
- Nothing else

**Allowed shadow:**
- Tooltips only: `0 4px 24px rgba(0,0,0,.6)`
- No `box-shadow` on any card, button, or input

---

## 7. Marketing Page — Specific Rules

The landing page (`index.html`) has larger type and more vertical breathing room than the dashboard. Additional rules:

- **Hero headline**: `clamp(36px, 6vw, 68px)`, weight 800, `letter-spacing: -2px`
- **Hero accent**: wrap key phrase in `<span class="accent">` → `color: var(--lime)`
- **Section eyebrow** (`.section-label`): `10px`, `700`, `JetBrains Mono`, `uppercase`, `letter-spacing: 3px`, `color: var(--lime)`, `margin-bottom: 14px`
- **Section title**: `clamp(28px, 4vw, 44px)`, weight 800, `letter-spacing: -1px`
- **Stats bar**: full-width strip, `background: var(--surface)`, numbers in `JetBrains Mono 36px 800`
- **How-it-works steps**: rendered as a connected grid (`background: var(--border)` gap between cells, each cell `background: var(--card)`)
- **SOM code demo**: two panels side-by-side — bad (large, red badge) / good (small, lime badge). Use `var(--surface)` background, never `var(--card)`
- **Segment icons**: inline SVG only, sized `22×22px` inside a `48×48px` icon container with the appropriate tier/segment colour background
- **Pricing cards**: flat `var(--card)` background for all tiers including featured. Featured uses `border-color: var(--lime-border)` only — no gradient, no different background
- **Featured badge** (`.featured-badge`): `background: var(--lime)`, `color: #000`, positioned absolute `-12px` top, centered
- **FAQ**: `.faq-item` accordion with `+` / rotated `+` icon in lime

---

## 8. Icons

Use inline SVG only. No emoji, no icon fonts, no external icon libraries.

Standard icon size in containers: `22×22px`.
Standard icon stroke: `stroke-width: 1.8`, `stroke="currentColor"` or hardcoded accent colour.
Icon container: `48×48px`, `border-radius: 12px`, appropriate tier/segment background.

Stroke colours by usage:
- Publisher icons → `stroke="#a3e635"` (lime)
- AI company icons → `stroke="#60a5fa"` (blue)
- CDN/infra icons → `stroke="#a78bfa"` (purple)
- Warning/caution → `stroke="#fbbf24"` (amber)
- Danger/blocked → `stroke="#f87171"` (red)
- Generic UI (nav logo, step icons) → `stroke="#a3e635"` (lime)

---

## 9. Relay Logo

The Relay logo mark is a horizontal relay diagram:

```svg
<svg viewBox="0 0 20 20" fill="none">
  <circle cx="4"  cy="10" r="2.5" fill="#a3e635"/>           <!-- left node (publisher) -->
  <circle cx="16" cy="10" r="2.5" fill="#a3e635" opacity="0.5"/> <!-- right node (AI) -->
  <line x1="6.5" y1="10" x2="13.5" y2="10" stroke="#a3e635" stroke-width="1.5"/> <!-- wire -->
  <circle cx="10" cy="10" r="1.5" fill="#a3e635" opacity="0.8"/> <!-- relay node (centre) -->
</svg>
```

- Full opacity left node = publisher (content source, active)
- 50% opacity right node = AI agent (consumer, passive)
- Central node = Relay (the intelligence layer)
- Nav size: `28×28px` container, `16×16px` SVG
- Login/marketing size: `36×36px` container, `20×20px` SVG

---

## 10. Do's and Don'ts

### Do
- Read this file before writing any Relay HTML or CSS
- Use `var(--lime)` for exactly one primary CTA per section
- Use `JetBrains Mono` for all numbers, bytes, paths, agent names, API keys
- Use `text-transform: uppercase` + `letter-spacing` for all eyebrow / label text
- Add `transition: all .15s` to all interactive elements
- Use SVG icons — never emoji
- Use background layering for depth — never `box-shadow` on cards
- Close-tag all SVGs properly; always specify `fill="none"` when using stroke

### Don't
- Don't use `#fff` or `background: white` anywhere
- Don't add gradients — not even on featured/highlighted cards
- Don't use emoji as decoration
- Don't use `box-shadow` on cards or inputs
- Don't exceed `border-radius: 8px` on buttons, `12px` on cards, `16px` on modals
- Don't set `btn-lime` text to anything other than `#000`
- Don't use lime as body text colour (only labels, badges, accents)
- Don't nest more than 2 depth layers (`--surface` → `--card`)
- Don't use font-size below `9px`
- Don't invent new CSS custom properties — use the defined palette

---

## 11. Agent Prompt Guide

### Quick colour reference

```
Page bg:       #080810
Surface:       #0f0f1a  (nav, inputs, code blocks)
Card:          #13131f  (cards, stat boxes)
Border:        rgba(255,255,255,.07)
Text:          #e8e8f0
Muted:         #8888a0
Lime (accent): #a3e635
Lime pale bg:  rgba(163,230,53,.12)
Lime border:   rgba(163,230,53,.25)
Red:           #f87171
Amber:         #fbbf24
Blue:          #60a5fa
Purple:        #a78bfa
```

### Starter prompt — new dashboard tab

> "Add a new tab to the Relay dashboard following DESIGN.md. Nav tab uses `.nav-tab` / `.nav-tab.active`. Content uses `.card` with `.card-title` (uppercase, muted, 13px, 0.06em tracking). KPI row uses 4-col stat grid with `.stat-card` + `.stat-value` (JetBrains Mono 28px 700) + `.stat-label` (uppercase muted 11px). Tables use `.data-table` with `.mono` on path/agent name cells. Agent tiers use `.tier-badge .tier-{training|inference|search|unknown|blocked}`. Empty states use `color: var(--muted)` centred. Loading uses `.spinner`."

### Starter prompt — new marketing section

> "Add a section to the Relay landing page following DESIGN.md. Section eyebrow: `.section-label` (JetBrains Mono, 10px, 700, uppercase, letter-spacing 3px, lime). Section title: `clamp(28px,4vw,44px)`, 800, letter-spacing -1px. Cards: `var(--card)` bg, `1px solid var(--border)`, border-radius 12px. Icons: inline SVG 22×22 inside 48×48 container with appropriate tier colour bg. No gradients, no emoji, no box-shadows. One `.btn-lime` CTA maximum per section, `color: #000`."

### Component class reference

| Need                      | Class / pattern                                              |
|---------------------------|--------------------------------------------------------------|
| Primary CTA button        | `.btn .btn-lime` — `color: #000`                            |
| Secondary button          | `.btn .btn-outline`                                          |
| Ghost button              | `.btn .btn-ghost`                                            |
| Danger button             | `.btn .btn-danger`                                           |
| Large marketing CTA       | `.btn .btn-lime .btn-lg` — still `border-radius: 8px`       |
| Card container            | `.card`                                                      |
| Card title                | `.card-title` (uppercase, muted, 13px, 0.06em)              |
| KPI stat card             | `.stat-card` + `.stat-value` + `.stat-label` + `.stat-sub`  |
| Agent tier badge          | `.tier-badge .tier-{training\|inference\|search\|unknown\|blocked}` |
| Plan badge                | `.plan-badge .plan-{free\|growth\|scale\|enterprise}`        |
| Dashboard data table      | `.data-table` + `.mono` on numeric/path cells               |
| Section eyebrow           | `.section-label` (JetBrains Mono, 10px, 700, uppercase, 3px tracking, lime) |
| Nav tab                   | `.nav-tab` / `.nav-tab.active`                              |
| Toggle switch             | `.toggle` + `.toggle-slider`                                 |
| Code/CLI block            | `.code-block`                                               |
| API key display           | `.api-key-box` (JetBrains Mono, lime, surface bg)           |
| Spinner                   | `.spinner`                                                  |
| Live dot                  | `.live-dot` (pulse animation)                               |
| Toast success             | `.toast .toast-success`                                     |
| Toast error               | `.toast .toast-error`                                       |
| FAQ accordion             | `.faq-item` + `.faq-q` + `.faq-a` + `.faq-icon`            |
| Onboarding modal          | `.modal-overlay` + `.modal-box`                             |
| Empty state               | `text-align: center; padding: 48px; color: var(--muted)`    |
