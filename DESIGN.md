# Relay Design System

> **For AI agents:** Read this file in full before generating any Relay UI — landing page, dashboard, or worker config. Follow every rule exactly. Do not invent new colours, fonts, spacing values, or components.

---

## 1. Visual Theme & Atmosphere

**Mood:** Clean publisher platform — confident, modern, approachable. Relay is software for publishers and content teams, not infrastructure engineers. The UI should feel closer to Notion, Linear, or Stripe Checkout than a Cloudflare dashboard. Publishers trust it immediately; developers respect it.

**Key characteristics:**
- White page background with layered off-white surfaces
- Vivid violet (`#7c3aed`) as the primary accent — CTAs, active states, links
- Soft secondary accents: teal (`#0d9488`) for positive/saved signals, amber (`#d97706`) for warnings
- `Inter` for all prose, labels, navigation, and headings
- `JetBrains Mono` for numbers, bytes, API keys, agent names, code, dates in tables
- Depth from subtle shadows and off-white tiers — no dark layering
- Gradients allowed only on the hero section background (soft, not vivid)
- Glassmorphism only on the fixed nav (`backdrop-filter: blur(12px)`)
- Animate sparingly: spinner, live-dot pulse, Chart.js entry transitions, button hover

**Relay personality:**
- Not a security tool — never red-dominant or threat-themed
- Not a dev tool — avoid terminal/CLI aesthetics outside code blocks
- Publisher-first: clean reading rhythm, generous whitespace, trust signals prominent
- Vivid but not loud: the accent pops against white, but doesn't shout

**Do not:**
- Use dark backgrounds, dark cards, or any dark-mode elements
- Use emojis anywhere — use inline SVG icons only
- Use more than one gradient per page (hero only)
- Use border-radius above `20px` (modal maximum), above `14px` on cards, above `8px` on buttons/inputs
- Use more than 5 font sizes in a single component
- Use violet as body text colour — violet is for labels, badges, accents, CTAs only
- Set `btn-primary` text to anything other than `#fff`
- Nest more than 2 surface layers (`--surface` → `--card`)

---

## 2. Colour Palette

### CSS Custom Properties — always use these, never raw hex

```css
/* Base */
--page:        #ffffff   /* page background                              */
--surface:     #f8f7ff   /* nav, sidebar, secondary panels               */
--card:        #ffffff   /* card backgrounds, stat boxes                 */
--border:      #e5e3f0   /* default border everywhere                    */
--border-dark: #d1cee8   /* stronger border for emphasis                 */
--text:        #1a1730   /* primary body text                            */
--muted:       #6b6885   /* labels, meta, secondary descriptive text     */
--subtle:      #9896b0   /* placeholder text, tertiary labels            */

/* Accent — Violet */
--violet:        #7c3aed   /* primary CTA, active states, links           */
--violet-light:  #ede9fe   /* violet-tinted backgrounds                   */
--violet-border: #c4b5fd   /* violet borders                              */
--violet-hover:  #6d28d9   /* button hover                                */

/* Positive — Teal */
--teal:        #0d9488   /* success, saved, positive signals, live dot   */
--teal-light:  #ccfbf1   /* teal-tinted bg                               */
--teal-border: #5eead4   /* teal borders                                 */

/* Semantic */
--red:         #dc2626   /* errors, blocked state, danger                */
--red-light:   #fee2e2   /* red-tinted bg                                */
--red-border:  #fca5a5   /* red borders                                  */
--amber:       #d97706   /* warnings, caution, search-tier agents        */
--amber-light: #fef3c7   /* amber-tinted bg                              */
--amber-border:#fcd34d   /* amber borders                                */
--blue:        #2563eb   /* info, AI inference agents                    */
--blue-light:  #dbeafe   /* blue-tinted bg                               */
--blue-border: #93c5fd   /* blue borders                                 */
--purple:      #7c3aed   /* AI training agents (same as --violet)        */

/* Layout */
--r:           14px      /* primary border radius (cards)                */
--nav-h:       60px      /* fixed nav height                             */
```

### Agent Tier Colour Map

| Tier       | Colour | Hex       | Background    | Border          | Text       | Meaning                        |
|------------|--------|-----------|---------------|-----------------|------------|--------------------------------|
| training   | violet | `#7c3aed` | `#ede9fe`     | `#c4b5fd`       | `#5b21b6`  | LLM training crawlers          |
| inference  | blue   | `#2563eb` | `#dbeafe`     | `#93c5fd`       | `#1d4ed8`  | Real-time agent/chat requests  |
| search     | amber  | `#d97706` | `#fef3c7`     | `#fcd34d`       | `#b45309`  | AI-powered search bots         |
| unknown    | muted  | `#6b6885` | `#f3f2f8`     | `#e5e3f0`       | `#6b6885`  | Unclassified agent traffic     |
| blocked    | red    | `#dc2626` | `#fee2e2`     | `#fca5a5`       | `#b91c1c`  | Requests blocked by policy     |

### Plan Colour Map

| Plan       | Background    | Border          | Text       |
|------------|---------------|-----------------|------------|
| free       | `#f3f2f8`     | `var(--border)` | `#6b6885`  |
| growth     | `#dbeafe`     | `#93c5fd`       | `#1d4ed8`  |
| scale      | `#ede9fe`     | `#c4b5fd`       | `#5b21b6`  |
| enterprise | `#fef3c7`     | `#fcd34d`       | `#b45309`  |

### Semantic Colour Uses

| Colour | Use cases                                                                 |
|--------|---------------------------------------------------------------------------|
| Violet | Primary CTA buttons, active nav tab, links, accent text, live dot         |
| Teal   | Success states, saved confirmations, positive KPI delta, plan: free badge |
| Red    | Blocked policy, errors, danger actions                                    |
| Amber  | Search-tier agents, warnings, caution states                              |
| Blue   | Inference-tier agents, info states, growth plan badge                     |
| Muted  | All labels, eyebrows, secondary text, unknown tier badges                 |

---

## 3. Typography

### Font Stack

```css
font-family: 'Inter', system-ui, sans-serif;        /* all UI text, headings, nav */
font-family: 'JetBrains Mono', monospace;           /* numbers, API keys, agent names, bytes, paths */
```

Google Fonts import (use this exact string):
```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet" />
```

### Size & Weight Hierarchy

#### Marketing / Landing Page

| Role                       | Size                   | Weight | Family    | Transform | Tracking |
|----------------------------|------------------------|--------|-----------|-----------|----------|
| Hero headline              | `clamp(38px,6vw,72px)` | 800    | Inter     | —         | `-2px`   |
| Section title              | `clamp(28px,4vw,46px)` | 800    | Inter     | —         | `-1px`   |
| Card / feature title       | `18–22px`              | 700    | Inter     | —         | —        |
| Body prose                 | `16–17px`              | 400    | Inter     | —         | —        |
| Hero eyebrow / badge label | `11px`                 | 700    | JetBrains | uppercase | `3px`    |
| Section eyebrow            | `11px`                 | 700    | JetBrains | uppercase | `3px`    |
| Stats bar number           | `36–40px`              | 800    | JetBrains | —         | —        |
| Pricing amount             | `44px`                 | 800    | JetBrains | —         | —        |
| Plan name label            | `12px`                 | 700    | JetBrains | uppercase | `0.06em` |
| Feature list item          | `14px`                 | 400–500| Inter     | —         | —        |
| Footer / legal             | `13px`                 | 400    | Inter     | —         | —        |

#### Dashboard / App

| Role                          | Size      | Weight  | Family    | Transform | Tracking  |
|-------------------------------|-----------|---------|-----------|-----------|-----------|
| Stat number (KPI cards)       | `30px`    | 700     | JetBrains | —         | tabular   |
| Page / modal title            | `18px`    | 700     | Inter     | —         | —         |
| Card title                    | `13px`    | 600     | Inter     | uppercase | `0.06em`  |
| Table row data                | `13px`    | 400     | Inter     | —         | —         |
| Monospace data (paths, keys)  | `12–13px` | 400     | JetBrains | —         | —         |
| Labels / table headers        | `11px`    | 600     | Inter     | uppercase | `0.08em`  |
| Nav brand                     | `16px`    | 700     | Inter     | —         | —         |
| Nav tab                       | `13px`    | 500     | Inter     | —         | —         |
| Badge / tier label            | `11px`    | 600     | JetBrains | uppercase | `0.05em`  |
| Sub-label / stat note         | `12px`    | 400     | Inter     | —         | —         |

### Rules
- All byte values, percentages, request counts, latency numbers → `JetBrains Mono`
- All agent names, page paths, API keys → `JetBrains Mono`
- All dates in tables → `JetBrains Mono`
- Section eyebrows/labels → `JetBrains Mono`, 11px, 700, uppercase, `letter-spacing: 3px`, `color: var(--violet)`
- `font-variant-numeric: tabular-nums` on all KPI and stat numbers
- Line heights: `1` for large Mono numbers, `1.2` for headings, `1.55` for body, `1.7` for marketing prose
- Body text is always `var(--text)` (`#1a1730`) — never pure `#000`

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
/* Primary */
.btn-primary  { background: var(--violet);  color: #fff; border-color: var(--violet); }
.btn-primary:hover { background: var(--violet-hover); border-color: var(--violet-hover); }

/* Secondary / outline */
.btn-outline  { background: #fff; color: var(--text); border: 1px solid var(--border-dark); }
.btn-outline:hover { border-color: var(--violet-border); color: var(--violet); }

/* Ghost */
.btn-ghost    { background: var(--violet-light); color: var(--violet); border: 1px solid var(--violet-border); }
.btn-ghost:hover { background: #ddd6fe; }

/* Danger */
.btn-danger   { background: var(--red-light); color: var(--red); border: 1px solid var(--red-border); }
.btn-danger:hover { background: #fecaca; }

/* Sizes */
.btn-sm  { padding: 6px 12px; font-size: 12px; }
.btn-xs  { padding: 4px 10px; font-size: 11px; border-radius: 6px; }
.btn-lg  { padding: 14px 30px; font-size: 15px; } /* marketing CTAs — still border-radius: 8px */

/* Disabled */
.btn:disabled { opacity: .4; cursor: not-allowed; }
```

**Rules:**
- `btn-primary` text is always `#fff`
- Never exceed `border-radius: 8px` on buttons (including `.btn-lg`)
- One `btn-primary` per screen section maximum — all others use `btn-outline` or `btn-ghost`

### 4.2 Cards

```css
.card {
  background: var(--card);          /* #ffffff */
  border: 1px solid var(--border);  /* #e5e3f0 */
  border-radius: var(--r);          /* 14px */
  padding: 24px;
  box-shadow: 0 1px 4px rgba(28,24,60,.06);
}
.card-title {
  font-size: 13px; font-weight: 600;
  letter-spacing: 0.06em; text-transform: uppercase;
  color: var(--muted); margin-bottom: 16px;
}
```

Alert variants (border + tinted background):
```css
.card-alert-blocked  { background: var(--red-light);    border-color: var(--red-border); }
.card-alert-featured { background: var(--violet-light); border-color: var(--violet-border); }
.card-alert-success  { background: var(--teal-light);   border-color: var(--teal-border); }
```

**Rules:**
- Cards sit on `--page` (`#fff`) — surface cards on `--surface` (`#f8f7ff`)
- `box-shadow: 0 1px 4px rgba(28,24,60,.06)` — the only allowed shadow on cards (very subtle lift)
- No gradients inside cards, including featured/highlighted variants
- Sub-surfaces inside cards: `background: var(--surface)` or `var(--violet-light)` — never darker than `--surface`

### 4.3 Inputs

```css
.input, .form-input {
  width: 100%; padding: 10px 14px;
  background: #fff; color: var(--text);
  border: 1.5px solid var(--border); border-radius: 8px;
  font-size: 14px; font-family: inherit;
  transition: border-color .15s, box-shadow .15s; outline: none;
}
.input:focus, .form-input:focus {
  border-color: var(--violet);
  box-shadow: 0 0 0 3px var(--violet-light);
}
.input::placeholder { color: var(--subtle); }
```

### 4.4 Agent Tier Badges

```css
.tier-badge {
  display: inline-flex; align-items: center;
  padding: 2px 8px; border-radius: 4px;
  font-size: 11px; font-weight: 600;
  letter-spacing: 0.05em; text-transform: uppercase;
  font-family: 'JetBrains Mono', monospace;
}
.tier-training  { background: #ede9fe; color: #5b21b6; border: 1px solid #c4b5fd; }
.tier-inference { background: #dbeafe; color: #1d4ed8; border: 1px solid #93c5fd; }
.tier-search    { background: #fef3c7; color: #b45309; border: 1px solid #fcd34d; }
.tier-unknown   { background: #f3f2f8; color: #6b6885; border: 1px solid #e5e3f0; }
.tier-blocked   { background: #fee2e2; color: #b91c1c; border: 1px solid #fca5a5; }
```

### 4.5 Plan Badges

```css
.plan-badge {
  padding: 3px 10px; border-radius: 20px;
  font-size: 11px; font-weight: 600;
  letter-spacing: 0.06em; text-transform: uppercase;
  font-family: 'JetBrains Mono', monospace;
}
.plan-free       { background: #f3f2f8; color: #6b6885;  border: 1px solid #e5e3f0; }
.plan-growth     { background: #dbeafe; color: #1d4ed8;  border: 1px solid #93c5fd; }
.plan-scale      { background: #ede9fe; color: #5b21b6;  border: 1px solid #c4b5fd; }
.plan-enterprise { background: #fef3c7; color: #b45309;  border: 1px solid #fcd34d; }
```

### 4.6 Stats / KPI Cards (Dashboard)

```css
.stat-card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--r);
  padding: 20px;
  box-shadow: 0 1px 4px rgba(28,24,60,.06);
}
.stat-label {
  font-size: 11px; font-weight: 600;
  letter-spacing: 0.08em; text-transform: uppercase;
  color: var(--muted); margin-bottom: 8px;
}
.stat-value {
  font-size: 30px; font-weight: 700;
  font-family: 'JetBrains Mono', monospace;
  color: var(--text);
  line-height: 1; font-variant-numeric: tabular-nums;
}
.stat-sub {
  font-size: 12px; color: var(--muted); margin-top: 6px;
}
/* Colour modifiers — only for the number itself */
.stat-value.violet { color: var(--violet); }
.stat-value.teal   { color: var(--teal); }
.stat-value.amber  { color: var(--amber); }
.stat-value.red    { color: var(--red); }
```

### 4.7 Data Tables (Dashboard)

```css
.data-table { width: 100%; border-collapse: collapse; font-size: 13px; }
.data-table th {
  padding: 8px 12px; text-align: left;
  font-size: 11px; font-weight: 600;
  letter-spacing: 0.08em; text-transform: uppercase;
  color: var(--muted); border-bottom: 1px solid var(--border);
  background: var(--surface);
}
.data-table td { padding: 12px; border-bottom: 1px solid var(--border); color: var(--text); }
.data-table tr:last-child td { border-bottom: none; }
.data-table tbody tr:hover { background: var(--surface); }
.mono { font-family: 'JetBrains Mono', monospace; font-size: 12px; color: var(--muted); }
```

### 4.8 Toggles

```css
.toggle { position: relative; width: 40px; height: 22px; flex-shrink: 0; }
.toggle input { opacity: 0; width: 0; height: 0; }
.toggle-slider {
  position: absolute; inset: 0;
  background: #e5e3f0; border: 1.5px solid var(--border-dark);
  border-radius: 22px; cursor: pointer; transition: all .2s;
}
.toggle-slider::before {
  content: '';
  position: absolute; width: 16px; height: 16px;
  left: 2px; top: 1px;
  background: #fff; border-radius: 50%;
  box-shadow: 0 1px 3px rgba(0,0,0,.15);
  transition: all .2s;
}
.toggle input:checked + .toggle-slider { background: var(--violet); border-color: var(--violet-hover); }
.toggle input:checked + .toggle-slider::before { transform: translateX(18px); }
```

### 4.9 Code Blocks

Used for API keys, CLI commands, config snippets.

```css
.code-block {
  background: #1e1b2e; border: 1px solid #352f4a;
  border-radius: 8px; padding: 12px 14px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 12px; color: #a78bfa;
  white-space: pre-wrap; word-break: break-all;
  position: relative;
}
```

Code blocks are the only dark-background element allowed. They stand out as intentional contrast.

### 4.10 Navigation

```css
/* Fixed top nav */
nav {
  position: fixed; top: 0; left: 0; right: 0;
  height: var(--nav-h);  /* 60px */
  background: rgba(255,255,255,.88);
  border-bottom: 1px solid var(--border);
  backdrop-filter: blur(12px);
  z-index: 300;
}

/* Nav tabs (dashboard) */
.nav-tab {
  padding: 6px 14px; border-radius: 6px;
  font-size: 13px; font-weight: 500;
  color: var(--muted); border: none; background: none;
  cursor: pointer; transition: all .15s;
}
.nav-tab:hover  { color: var(--text); background: var(--surface); }
.nav-tab.active { color: var(--violet); background: var(--violet-light); font-weight: 600; }
```

### 4.11 Modals / Overlays

```css
.modal-overlay {
  position: fixed; inset: 0;
  background: rgba(26,23,48,.5);
  backdrop-filter: blur(6px);
  z-index: 500;
  display: flex; align-items: center; justify-content: center;
}
.modal-box {
  background: #fff; border: 1px solid var(--border);
  border-radius: 20px; padding: 40px;
  max-width: 520px; width: 90%;
  box-shadow: 0 20px 60px rgba(26,23,48,.15);
}
```

`border-radius: 20px` is the maximum — only for modals/overlays.

### 4.12 Spinners

```css
.spinner {
  display: inline-block; width: 20px; height: 20px;
  border: 2px solid var(--border);
  border-top-color: var(--violet);
  border-radius: 50%;
  animation: spin .8s linear infinite;
}
@keyframes spin { to { transform: rotate(360deg); } }
```

### 4.13 Live Indicator Dot (Marketing / Status)

```css
.live-dot {
  width: 7px; height: 7px;
  border-radius: 50%; background: var(--teal);
  animation: pulse 2s ease-in-out infinite;
}
@keyframes pulse {
  0%, 100% { opacity: 1; transform: scale(1); }
  50%       { opacity: .5; transform: scale(.8); }
}
```

### 4.14 Toasts

```css
.toast {
  padding: 10px 18px; border-radius: 10px;
  font-size: 13px; font-weight: 600; max-width: 320px;
  box-shadow: 0 4px 16px rgba(28,24,60,.12);
}
.toast-success { background: var(--teal-light);  color: #0f766e; border: 1px solid var(--teal-border); }
.toast-error   { background: var(--red-light);   color: var(--red); border: 1px solid var(--red-border); }
.toast-info    { background: var(--violet-light); color: var(--violet); border: 1px solid var(--violet-border); }
```

### 4.15 FAQ Accordion

```css
.faq-item {
  background: #fff;
  border: 1px solid var(--border);
  border-radius: 10px; overflow: hidden;
  box-shadow: 0 1px 3px rgba(28,24,60,.05);
}
.faq-q {
  padding: 20px 24px; font-weight: 600; font-size: 15px; color: var(--text);
  cursor: pointer; display: flex; align-items: center; justify-content: space-between;
  user-select: none; transition: background .15s;
}
.faq-q:hover { background: var(--surface); }
.faq-icon { color: var(--violet); font-size: 22px; transition: transform .2s; }
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
Section padding (marketing): 100px 40px desktop, 64px 20px mobile
Page background: #ffffff
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
12px  component padding medium
14px  input padding, nav spacing
16px  card padding compact
20px  element gap standard, card header margin
24px  card padding, section gap
28px  page top clearance
32px  dashboard top clearance, hero padding inner
40px  section horizontal padding (desktop)
48px  section vertical padding (mobile)
64px  section vertical padding (mobile large)
100px section vertical padding (desktop marketing)
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

Depth via subtle shadows and off-white tiers — no dark layering.

| Layer   | Background | Used for                              |
|---------|------------|---------------------------------------|
| Page    | `#ffffff`  | Page background                       |
| Surface | `#f8f7ff`  | Nav bg (blurred), section alternates, table headers |
| Card    | `#ffffff`  | Cards — lifted above page with 1px border + subtle shadow |
| Tinted  | `var(--violet-light)` / `var(--teal-light)` | Highlighted cards, alert rows |

**Allowed shadows:**
- Cards: `0 1px 4px rgba(28,24,60,.06)` — very subtle lift
- Modals: `0 20px 60px rgba(26,23,48,.15)` — visible lift
- Toasts: `0 4px 16px rgba(28,24,60,.12)`
- Focus rings (inputs): `0 0 0 3px var(--violet-light)`

**Allowed blur effects:**
- Nav only: `backdrop-filter: blur(12px)`
- Modal overlay: `backdrop-filter: blur(6px)`

---

## 7. Marketing Page — Specific Rules

The landing page (`index.html`) has larger type and more vertical breathing room than the dashboard.

- **Hero headline**: `clamp(38px, 6vw, 72px)`, weight 800, `letter-spacing: -2px`
- **Hero background**: single soft radial gradient allowed — `radial-gradient(ellipse 80% 50% at 50% -10%, rgba(124,58,237,.08) 0%, transparent 60%)` on `--surface` (`#f8f7ff`) section background. This is the only gradient in the entire system.
- **Hero accent**: wrap key phrase in `<span class="accent">` → `color: var(--violet)`
- **Section eyebrow** (`.section-label`): `11px`, `700`, `JetBrains Mono`, `uppercase`, `letter-spacing: 3px`, `color: var(--violet)`, `margin-bottom: 14px`
- **Section title**: `clamp(28px, 4vw, 46px)`, weight 800, `letter-spacing: -1px`, `color: var(--text)`
- **Stats bar**: full-width strip, `background: var(--surface)`, `border-top: 1px solid var(--border)`, `border-bottom: 1px solid var(--border)`, numbers in `JetBrains Mono 40px 800 var(--violet)`
- **Alternating sections**: odd sections white (`#fff`), even sections `var(--surface)` (`#f8f7ff`)
- **How-it-works steps**: numbered cards on `var(--surface)` background, step number in `JetBrains Mono 24px 800 var(--violet)`
- **SOM code demo**: two panels side-by-side. Both use `.code-block` (dark bg `#1e1b2e`) — consistent dark code aesthetic
- **Segment icons**: inline SVG only, `22×22px` inside `52×52px` icon container with appropriate tier colour light background (`--violet-light`, `--teal-light`, etc.) and `border-radius: 14px`
- **Pricing cards**: white background for all tiers. Featured uses `border: 2px solid var(--violet)` and `box-shadow: 0 8px 32px rgba(124,58,237,.12)` — slightly elevated, no different background
- **Featured badge** (`.featured-badge`): `background: var(--violet)`, `color: #fff`, positioned absolute `-12px` top, centered
- **FAQ**: section on `var(--surface)` background, `.faq-item` accordion with `+` icon in violet

---

## 8. Icons

Use inline SVG only. No emoji, no icon fonts, no external icon libraries.

Standard icon size in containers: `22×22px`.
Standard icon stroke: `stroke-width: 1.8`, `stroke="currentColor"` or hardcoded accent colour.
Icon container: `52×52px`, `border-radius: 14px`, appropriate light background.

Stroke colours by usage:
- Publisher icons → `stroke="#7c3aed"` (violet)
- AI agent icons → `stroke="#2563eb"` (blue)
- Positive/saved → `stroke="#0d9488"` (teal)
- Warning/caution → `stroke="#d97706"` (amber)
- Danger/blocked → `stroke="#dc2626"` (red)
- Generic UI (nav, steps) → `stroke="#7c3aed"` (violet)

---

## 9. Relay Logo

The Relay logo mark is a horizontal relay diagram:

```svg
<svg viewBox="0 0 20 20" fill="none">
  <circle cx="4"  cy="10" r="2.5" fill="#7c3aed"/>                    <!-- left node (publisher) -->
  <circle cx="16" cy="10" r="2.5" fill="#7c3aed" opacity="0.4"/>      <!-- right node (AI) -->
  <line x1="6.5" y1="10" x2="13.5" y2="10" stroke="#7c3aed" stroke-width="1.5"/>
  <circle cx="10" cy="10" r="1.5" fill="#7c3aed" opacity="0.75"/>     <!-- relay node (centre) -->
</svg>
```

- Full opacity left node = publisher (content source, active)
- 40% opacity right node = AI agent (consumer, passive)
- Central node = Relay (the intelligence layer)
- Nav size: `28×28px` container, `16×16px` SVG
- Login/marketing size: `40×40px` container, `22×22px` SVG

---

## 10. Do's and Don'ts

### Do
- Read this file before writing any Relay HTML or CSS
- Use `var(--violet)` for exactly one primary CTA per section
- Use `JetBrains Mono` for all numbers, bytes, paths, agent names, API keys
- Use `text-transform: uppercase` + `letter-spacing` for all eyebrow / label text
- Add `transition: all .15s` to all interactive elements
- Use SVG icons — never emoji
- Use `var(--surface)` (`#f8f7ff`) for alternating sections — not grey, not dark
- Close-tag all SVGs properly; always specify `fill="none"` when using stroke

### Don't
- Don't use dark backgrounds (except `.code-block`)
- Don't add gradients — only the hero section radial gradient is allowed
- Don't use emoji as decoration
- Don't exceed `border-radius: 8px` on buttons, `14px` on cards, `20px` on modals
- Don't set `btn-primary` text to anything other than `#fff`
- Don't use violet as body text colour (only labels, badges, accents, CTAs)
- Don't nest more than 2 surface layers (`--surface` → `--card`)
- Don't use font-size below `10px`
- Don't invent new CSS custom properties — use the defined palette
- Don't use pure `#000` black — use `var(--text)` (`#1a1730`)

---

## 11. Agent Prompt Guide

### Quick colour reference

```
Page:            #ffffff
Surface:         #f8f7ff  (nav, section alternates, table headers)
Card:            #ffffff  (+ 1px border + subtle shadow)
Border:          #e5e3f0
Border dark:     #d1cee8
Text:            #1a1730
Muted:           #6b6885
Subtle:          #9896b0
Violet (accent): #7c3aed
Violet light bg: #ede9fe
Violet border:   #c4b5fd
Teal (positive): #0d9488
Teal light bg:   #ccfbf1
Red:             #dc2626
Amber:           #d97706
Blue:            #2563eb
```

### Starter prompt — new dashboard tab

> "Add a new tab to the Relay dashboard following DESIGN.md (light theme). Nav tab uses `.nav-tab` / `.nav-tab.active` (active = violet bg `#ede9fe`, violet text). Content uses `.card` (white, 14px radius, subtle shadow) with `.card-title` (uppercase, muted, 13px, 0.06em tracking). KPI row uses 4-col stat grid with `.stat-card` + `.stat-value` (JetBrains Mono 30px 700) + `.stat-label` (uppercase muted 11px). Tables use `.data-table` with surface-bg headers, hover rows, `.mono` on path/agent cells. Agent tiers use `.tier-badge .tier-{training|inference|search|unknown|blocked}` (all light pastel). Loading uses `.spinner` (violet). Page background is white."

### Starter prompt — new marketing section

> "Add a section to the Relay landing page following DESIGN.md. Section background alternates: white or `#f8f7ff`. Section eyebrow: `.section-label` (JetBrains Mono, 11px, 700, uppercase, letter-spacing 3px, violet). Section title: `clamp(28px,4vw,46px)`, 800, letter-spacing -1px. Cards: white bg, `1px solid #e5e3f0`, border-radius 14px, subtle shadow. Icons: inline SVG 22×22 inside 52×52 container with pastel light bg. No gradients (hero radial only), no emoji. One `.btn-primary` CTA maximum per section, text `#fff`."

### Component class reference

| Need                      | Class / pattern                                                          |
|---------------------------|--------------------------------------------------------------------------|
| Primary CTA button        | `.btn .btn-primary` — `color: #fff`                                     |
| Secondary button          | `.btn .btn-outline`                                                      |
| Ghost button              | `.btn .btn-ghost`                                                        |
| Danger button             | `.btn .btn-danger`                                                       |
| Large marketing CTA       | `.btn .btn-primary .btn-lg` — still `border-radius: 8px`                |
| Card container            | `.card`                                                                  |
| Card title                | `.card-title` (uppercase, muted, 13px, 0.06em)                          |
| KPI stat card             | `.stat-card` + `.stat-value` + `.stat-label` + `.stat-sub`              |
| Agent tier badge          | `.tier-badge .tier-{training\|inference\|search\|unknown\|blocked}`     |
| Plan badge                | `.plan-badge .plan-{free\|growth\|scale\|enterprise}`                   |
| Dashboard data table      | `.data-table` + `.mono` on numeric/path cells                           |
| Section eyebrow           | `.section-label` (JetBrains Mono, 11px, 700, uppercase, 3px, violet)   |
| Nav tab                   | `.nav-tab` / `.nav-tab.active`                                          |
| Toggle switch             | `.toggle` + `.toggle-slider`                                             |
| Code/CLI block            | `.code-block` (dark bg — intentional contrast)                          |
| API key display           | `.api-key-box` (JetBrains Mono, dark bg `#1e1b2e`, violet text)        |
| Spinner                   | `.spinner` (violet)                                                     |
| Live dot                  | `.live-dot` (teal, pulse animation)                                     |
| Toast success             | `.toast .toast-success` (teal)                                          |
| Toast error               | `.toast .toast-error`                                                   |
| FAQ accordion             | `.faq-item` + `.faq-q` + `.faq-a` + `.faq-icon` (violet)              |
| Onboarding modal          | `.modal-overlay` + `.modal-box`                                         |
| Empty state               | `text-align: center; padding: 48px; color: var(--muted)`               |
