# PATHWAY — Employee Skills Assessment & Skill Management
## Complete Design Specification (Implementation Blueprint, Phase 5)

**Status: LOCKED DIRECTION.** This document specifies Option C "Pathway" exactly as explored in `design-exploration.md` and `design-boards.html`. It is a specification, not a redesign: philosophy, palette, navigation model, dashboard structure, wizard pattern, and gap visualization are unchanged from the approved direction. Everything below is build-ready detail for Phase 6 (HTML5 + CSS3 + vanilla JavaScript only, no build step).

Convention: every color pairing claimed for text or meaningful boundaries has been computed against WCAG 2.2 AA; measured ratios appear in §3.1.

---

## 1. PRODUCT SCOPE & PAGE ARCHITECTURE

### 1.1 Roles (prototype demo switcher in top bar, labeled "View as")
- **Evaluator / Manager** — runs assessments, issues reports. Default demo role.
- **HR / L&D admin** — everything above + Settings (proficiency model, skill library, role expectations).
- **Employee** — read-only own profile, own report, own development plan. Nav item 2 relabels to "My Growth" and deep-links to own profile.

### 1.2 Routes (hash router, vanilla JS)
| Route | Page | Purpose |
|---|---|---|
| `#/home` | Growth Map (dashboard) | Selected employee's competency picture, momentum, strengths/focus, activity |
| `#/people` | Employees | Searchable, filterable team wall / list |
| `#/people/:id` | Employee Profile | Story layout: identity, skill chapters, development, history |
| `#/assessments` | Assessments | Queue of assessment cycles: draft / submitted / not started, with progress |
| `#/assess/:id` | Assessment Wizard | Setup sheet → category stations → reflection → submit |
| `#/report/:id` | Assessment Report | Compiled, print/PDF-ready document |
| `#/settings` | Settings (admin) | Proficiency model, skill library, role expectations |

No login page in prototype (session assumed); no org-analytics page (out of scope per direction: employee-first product).

### 1.3 Primary vs secondary actions (hierarchy rule: exactly one primary per view)
- Primary per view: Home → "Start / Continue assessment"; People → none (search is the tool); Profile → "Start assessment" or "Continue assessment"; Wizard → "Save & continue" (per station) / "Submit assessment" (reflection); Report → "Print / Save as PDF"; Settings → "Save changes".
- Secondary: Save draft, View report, Add observation, filters, edit expectations.
- Destructive (Discard draft) lives inside a confirm modal, styled destructive-outline, never primary.

---

## 2. PAGE STRUCTURE & COMPONENT HIERARCHY

Trees use indentation = DOM nesting. `[C]` = component defined in §4.

### 2.1 Global chrome (all pages)
```
<body>
  skip-link ("Skip to main content")
  header.topbar [C-Topbar]
    brand (mark + "Pathway" wordmark)
    nav.tabs (Home · Employees|My Growth · Assessments · Reports→latest list anchor) [C-Tabs]
    actions: search toggle [C-Search], "View as" select [C-Select sm], avatar [C-Avatar]
  div.chipbar (contextual sub-navigation, present on profile/report pages) [C-ChipNav]
  main#main (container 1120px)
    <page content>
  div.toast-region (aria-live polite, bottom-center) [C-Toast]
  div.sheet-root / div.modal-root (portals) [C-Sheet, C-Modal]
```
Reports tab opens `#/assessments` filtered to submitted (no separate reports list page; report pages are reached from queues, profile history, and toasts). This keeps IA at 5 top-level items.

### 2.2 `#/home` — Growth Map
```
section.pagehead
  h1 "Growth map" + employee switcher [C-Select lg, searchable] + status chip [C-StatusChip]
div.hero (grid: 5/7 columns desktop)
  article.card.wheel-card
    h2 (employee name) + role/department line [C-Avatar lg inline]
    svg.wheel [C-Wheel] (6 spokes = 6 categories, labeled, value ticks 1–5)
    legend row: category chips with avg value text [C-CatChip]
  article.card.now-next
    h2 "Now"
    dl.meta (Evaluator · Cycle · Last assessment · Overall level word)
    progress bar "Assessment coverage" [C-Progress] (x of y skills rated, % text)
    h2 "Next"
    ul.next-list (next review date, 2 suggested focus items with gap chips)
    button.primary "Continue assessment" | "Start assessment" | "View report" (state-dependent)
section.journey
  h2 "Journey"
  ol.rail [C-JourneyRail] (milestones: last assessment · today marker · next review; horizontal)
div.shelves (grid 6/6)
  article.card.shelf.strengths
    h2 + count chip; ul: skill name + level word + tick icon + category chip (max 5, "+n more" link)
  article.card.shelf.focus
    h2 + count chip; ul: skill name + gap bracket mini [C-GapBracket compact] + first suggested action (max 5)
section.feed
  h2 "Recent activity"
  ul.feed (event icon, sentence with linked names, date mono) — max 6, "View all" → #/assessments
```

### 2.3 `#/people` — Employees
```
section.pagehead: h1 "Employees" + count text
div.filterbar [C-FilterBar]: search input, department select, status select, "Gaps only" toggle [C-Switch], sort select
div.wall (grid auto-fill minmax(260px,1fr))
  article.card.person [C-PersonCard] × n   (or ul.list-persons on tablet-mobile, see §5)
div.listfoot: "Showing x of y" + button "Load more" (page size 24)
empty state [C-Empty] when filters return none
```

### 2.4 `#/people/:id` — Employee Profile
```
header.profilehead.card
  C-Avatar xl | h1 name + role line + dept chip | dl.meta (ID mono, email, joined, manager)
  div.actions: button.primary start/continue, button.secondary "Latest report", button.ghost "Edit profile" (admin)
nav.chipbar [C-ChipNav]: Skills · Development · History (anchor scroll)
section#skills
  h2 "Skill chapters"
  article.card.chapter [C-ChapterCard] × 6 categories
    header: C-CatChip + category name + avg level word + coverage text
    ul.skillrows [C-SkillRow read-only]
      li: name | dots [C-Dots read] + level word | trend [C-Spark] | gap callout [C-GapBracket] when below expectation
section#development
  h2 "Development"
  div.two-col: ul.gaps (gap brackets full) | ul.recs [C-RecItem] (action, type chip, target date, owner)
section#history
  h2 "Assessment history"
  ol.timeline [C-Timeline] (event: cycle, evaluator, status chip, score delta, link to report)
empty states: no assessments yet → C-Empty with CTA inside #skills chapters (dots unfilled, "Not yet assessed")
```

### 2.5 `#/assessments` — Assessments queue
```
section.pagehead: h1 "Assessments" + cycle label
div.filterbar: status select (All/Draft/In progress/Submitted/Not started), department select, search
ul.assess-list
  li.card.assess [C-AssessCard]: employee (avatar+name), category coverage progress [C-Progress],
     status chip, updated date mono, due date, actions: Continue/Review/View report (single primary per row)
empty state per filter
```

### 2.6 `#/assess/:id` — Assessment Wizard
```
setup (first entry only, or via "Edit scope"): C-Sheet "Set up assessment"
  category checkbox chips [C-CatChip checkbox] with skill counts; confirm button primary
wizard shell
  header.momentum (sticky): employee avatar+name · "Category k of n" · coverage % · autosave text (aria-live) · buttons: Save draft (secondary), Exit (ghost)
  aside.rail [C-ProgressRail] (vertical stations: done tick / current filled / upcoming hollow; reflection station last)
  section.station (one category per screen)
    h2 category name + C-CatChip + plain-language category description line
    article.card.skill [C-SkillCard] × skills in category
      h3 skill name
      p.level-meaning [C-MeaningPanel] (definition of currently selected level; default: "Select a level to see what it means")
      fieldset.dots [C-Dots editable]: 5 radio dots + level word label beside; ghost expected dot [C-DotsGhost] with "Role expects: X"
      label+textarea "What did you observe?" [C-Textarea] (optional, 0–600 chars, counter)
      gap preview line [C-GapBracket compact] appears when rating ≠ expectation
  footer.stationbar (sticky bottom): button.ghost "Previous" · button.primary "Save & continue"
reflection station
  h2 "Review & reflect"
  table.review (skill, category chip, current word, expected word, gap chip) with "edit" jump links
  ul.rec-suggest [C-RecSuggest]: auto-suggested from gap rules; each = checkbox + action text + editable select (alternative actions) + target date input
  textarea "Evaluator summary" [C-Textarea]
  attestation line + button.primary "Submit assessment" (disabled until 100% coverage; reason tooltip + inline note)
submit → toast + route to #/report/:id
```

### 2.7 `#/report/:id` — Report
```
header.reporthead: org block ("Northwind Labs — People Development"), h1 employee name, dl metadata grid
   (ID, department, designation, role, evaluator, assessment date, cycle)
section: svg.wheel [C-Wheel print-safe] + visually-hidden equivalent table (revealed in print)
section: category tables (skill · current word · expected word · gap words) grouped by C-CatChip headers
section: "Overall summary" paragraph block (computed coverage + evaluator summary text)
section: "Skill gaps" ul [C-GapBracket full] sorted by severity
section: "Recommendations" ol [C-RecItem] with owners/dates
section: "Evaluator comments" per-category comment quotes
footer.reportfoot: generated-on mono + "Pathway" mark
toolbar (screen only, sticky top-right): button.primary "Print / Save as PDF", button.secondary "Back"
@media print: toolbar/nav hidden, single column, black ink, page-break-inside avoid per section
```

### 2.8 `#/settings` (admin)
```
h1 + three accordion sections [C-Accordion]:
  1 Proficiency model: ordered list of 5 levels; each: label input + definition textarea; note "Used across all views"
  2 Skill library: per category: list of skills (text + delete ghost icon) + "Add skill" inline form (validation: non-empty, unique within category)
  3 Role expectations: table role × skill: select of 5 levels (grouped by category, filter by role select)
footer bar: button.primary "Save changes" + dirty indicator dot
```

---

## 3. DESIGN TOKENS

All tokens are CSS custom properties on `:root`. No magic numbers in component CSS.

### 3.1 Color
**Surfaces & text**
| Token | Hex | Use | Measured contrast |
|---|---|---|---|
| `--bg-canvas` | `#F1F6FA` | page background | — |
| `--bg-surface` | `#FFFFFF` | cards, topbar, inputs | — |
| `--bg-sunken` | `#E9F1F6` | wheel well, rail track, table head tint | — |
| `--text-primary` | `#12283A` | headings, body emphasis | 15.1:1 white / 13.9:1 canvas |
| `--text-secondary` | `#46586A` | secondary lines, descriptions | 7.33:1 white / 6.74:1 canvas |
| `--text-muted` | `#5B6F82` | meta, captions, placeholders | 5.2:1 white / 4.78:1 canvas |
| `--text-faint` | `#64788C` | non-essential meta on white surfaces only | 4.56:1 white |
| `--border` | `#D8E4EC` | decorative card/input borders | non-text |
| `--border-strong` | `#7E97AB` | meaningful boundaries: input borders, unfilled dots, focus fallback | 3.04:1 vs white (inputs always sit on `--bg-surface`) |

**Interactive / accent (deep teal)**
| Token | Hex | Use | Contrast |
|---|---|---|---|
| `--accent` | `#0E7490` | links, primary buttons, current dots, active tab underline | 5.36:1 on white; white-on-it 5.36:1 |
| `--accent-hover` | `#155E75` | hover | 7.27:1 both ways |
| `--accent-active` | `#0B4A5C` | pressed | 8.9:1 |
| `--accent-soft` | `#E0F0F4` | selected chip bg, station current bg, tint panels | with `--accent-ink` 8.35:1 |
| `--accent-ink` | `#0B4A5C` | text on accent-soft | 8.35:1 |

**Status (always icon + text, never hue alone)**
| Token | Hex | Tint | Ink-on-tint |
|---|---|---|---|
| success `--success` | `#15803D` | `--success-tint #E7F4EB` | `--success-ink #146C36` (5.74:1) |
| warning/gap `--warning` | `#B45309` | `--warning-tint #FCF0E0` | `--warning-ink #9A4607` (5.74:1) |
| error `--error` | `#B42318` | `--error-tint #FCEBEA` | `--error-ink #B42318` (5.7:1) |
| info `--info` | `#1665A6` | `--info-tint #E8F1F9` | 5.33:1 |

Solid status chips use white text on `--success`/`--warning` (5.02:1 both).

**Category tints (6 fixed categories; chip always carries the name)**
| Category | tint | ink/dot |
|---|---|---|
| Technical | `#E4EEF8` | `#215E90` (5.83:1) |
| Behavioral | `#E3F2F4` | `#0F6473` (5.91:1) |
| Management | `#EFEAF6` | `#5D448B` (6.67:1) |
| Leadership | `#FBF1DF` | `#8F5B0A` (5.11:1) |
| Compliance | `#ECEFF3` | `#44546A` (6.69:1) |
| Social Impact | `#EDF3E4` | `#4F6B21` (5.36:1) |

Rules: (a) status never communicated by hue alone — icon + word always; (b) category never by hue alone — name inside chip; (c) inputs and meaning-bearing borders only placed on `--bg-surface`; (d) no gradients except print-safe hatching in gap meters (pattern + words accompany).

### 3.2 Typography
- `--font-sans`: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif` (no webfont download; friendly neo-grotesque per direction).
- `--font-mono`: `ui-monospace, SFMono-Regular, Menlo, Consolas, monospace` — employee IDs, dates, cycle codes only.
| Token | size/line | weight | tracking | Use |
|---|---|---|---|---|
| `t-display` | 28/34 | 700 | −0.015em | page h1 (desktop; 24/30 ≤640) |
| `t-title` | 24/30 | 700 | −0.01em | report h1, profile h1 |
| `t-h2` | 20/26 | 600 | −0.005em | card/section headings |
| `t-h3` | 17/24 | 600 | 0 | skill names, subsection heads |
| `t-card` | 15/22 | 600 | 0 | card titles, button labels |
| `t-body-lg` | 16/26 | 400 | 0 | meaning panel, report prose |
| `t-body` | 15/24 | 400 | 0 | default UI text |
| `t-sm` | 13/20 | 400 | 0 | descriptions, table cells |
| `t-caption` | 12/16 | 500 | 0 | meta lines, counters |
| `t-eyebrow` | 12/16 | 600 | +0.04em, uppercase | shelf labels, report org block (rare) |
| `t-mono` | 12.5/18 | 500 | 0 | IDs, dates |
Heading hierarchy: one `h1` per page; card headings `h2`; items `h3`; never skip levels. Sentence case everywhere except `t-eyebrow`.

### 3.3 Spacing (4px base)
`--sp-1 4 · --sp-2 8 · --sp-3 12 · --sp-4 16 · --sp-5 20 · --sp-6 24 · --sp-8 32 · --sp-10 40 · --sp-12 48 · --sp-16 64`
Semantic usage: inline gap 8; stack gap in cards 12; card padding 24 (16 ≤640); between cards 16; between page sections 32; page top/bottom 24 (16 ≤640); topbar height 60 (56 ≤640); filterbar gap 12.

### 3.4 Radius · Shadow · Motion · Focus · Elevation
- Radius: `--r-sm 6` (buttons, inputs, textarea), `--r-md 10` (sheets, popovers, meaning panel), `--r-lg 12` (cards), `--r-pill 999` (chips, dots container, switches).
- Shadow: `--shadow-1: 0 1px 2px rgba(16,42,67,.08)` (cards at rest); `--shadow-2: 0 4px 12px rgba(16,42,67,.10)` (hover lift, popovers); `--shadow-3: 0 12px 32px rgba(16,42,67,.16)` (modal, sheet). Nothing else casts shadows.
- Motion: `--dur-fast 120ms` (color/background), `--dur 180ms` (lift, chip, accordion), `--dur-slow 240ms` (station change, sheet slide, wheel draw-once); `--ease: cubic-bezier(.2,0,0,1)`. `@media (prefers-reduced-motion: reduce)`: all durations → 1ms, transforms disabled, wheel pre-drawn.
- Focus: `outline: 2px solid var(--accent); outline-offset: 2px` on `:focus-visible` for every interactive element; on accent-soft backgrounds switch outline to `--accent-ink`. Focus never removed.
- z-index: content 0 · sticky bars 40 · sheet 60 · modal 70 · toast 80.

### 3.5 Layout grid & breakpoints
- Container: max-width 1120px, centered; padding-inline 24 (16 ≤640).
- Breakpoints: **mobile ≤640**, **tablet 641–999**, **desktop ≥1000**, wide ≥1400 (container caps; no layout change).
- Desktop grids: Home hero `grid-template-columns: 5fr 7fr` (gap 16); shelves `1fr 1fr`; wizard `240px 1fr`; profile development `1fr 1fr`; report metadata `repeat(3, 1fr)`.
- Touch targets: ≥44×44px for dots, chips-in-use, icon buttons (padding-enlarged), switch.

---

## 4. COMPONENT SYSTEM (structure + states)

State key: **D** default · **H** hover · **F** focus-visible · **A** active/pressed · **X** disabled · **L** loading · **S** success · **E** error · **∅** empty.

**C-Topbar** — white surface, 1px `--border` bottom, sticky top. D: tabs `--text-secondary`; H: `--text-primary` + `--bg-sunken` pill; F: ring; A: pressed tint; current tab: `--text-primary` + 3px `--accent` underline (offset −1px inside bar). ≤999: tabs move into a slide-in drawer opened by a menu icon button (44px); drawer lists same items + current indicated by chip.

**C-Tabs / C-ChipNav** — chip nav: pills, D white + `--border`; current: `--accent-soft` bg + `--accent-ink` text + `--accent` 1px border; H lift shadow-2 1px; anchors smooth-scroll (instant under reduced motion) with `scroll-margin-top: 76px`.

**C-Button** — sizes: sm 32px, md 40px, lg 44px; padding-inline 16 (sm 12); radius `--r-sm`; label `t-card`.
- primary: bg `--accent`, white text. H `--accent-hover`; F ring offset 2; A `--accent-active` + translateY(1px); X 40% opacity + `cursor:not-allowed` + reason in `title`/inline note; L: spinner 14px (border-anim) + label swap ("Saving…") + aria-busy.
- secondary: white bg, 1px `--border-strong`, `--accent-ink` text. H `--bg-sunken`; A `--accent-soft`.
- ghost: transparent, `--text-secondary`. H `--bg-sunken` + `--text-primary`.
- destructive-outline: white bg, 1px `--error`, `--error-ink` text; H `--error-tint`.
**C-IconButton** — 40px square (44 on touch), radius `--r-sm`, same state colors as ghost; icon 18px stroke 1.75.

**C-Search** — input with leading magnifier icon, trailing clear (×) icon button when non-empty; D white + `--border-strong`; F ring + border `--accent`; E border `--error` + message below (aria-describedby); placeholder `--text-muted`. Debounce 250ms on type. "/" key focuses when no field focused.

**C-Select** — native `<select>` styled: white, `--border-strong`, radius `--r-sm`, height 40 (36 sm), custom chevron via background SVG (inline data URI). F ring. Searchable variant (employee switcher): button opening a popover listbox (role=listbox, type-ahead filter, arrow keys, Enter selects, Esc closes).

**C-Textarea / C-Input** — white, 1px `--border-strong`, radius `--r-sm`, padding 10×12, `t-body`; F ring + border accent; E border `--error` + `--error-ink` message 12px below + aria-invalid + aria-describedby; X bg `--bg-sunken`, text `--text-muted`; counter caption bottom-right when maxlength ("214 / 600", turns `--warning-ink` at >90%).

**C-Dots (proficiency selector)** — fieldset+legend (or role=radiogroup); 5 radios visually = dots 18px (hit area 44px via padding), gap 8; unfilled: white + 2px `--border-strong`; filled up to current: `--accent`; current dot additionally 3px ring `--accent-soft` halo; level word label right of group (`t-card`, updates live); native arrow-key behavior retained (real radios). **C-DotsGhost**: expected level rendered as 14px outlined dot in `--warning` dashed border + label "Role expects: Advanced" (`t-caption`, `--text-secondary`) — positioned after the word label, never inside the editable group. Read-only variant (profile): dots 14px, non-interactive, `aria-hidden` + adjacent visible level word.

**C-MeaningPanel** — `--accent-soft` tint panel, radius `--r-md`, padding 12×14, `t-body-lg` `--accent-ink`; shows definition of hovered/selected level; D (nothing selected): neutral text on `--bg-sunken` "Select a level to see what it means."; updates on radio focus (not only change) so keyboard users hear/see meaning; `aria-live=polite`.

**C-CatChip** — pill, category tint bg + ink text, 12px dot of category ink before label; sizes md (h 28) / sm (h 24, `t-caption`). Checkbox variant (setup sheet): leading 16px checkbox, border 1px category ink when checked, tint bg; unchecked: white + `--border-strong`.

**C-StatusChip** — pill h 24, icon 12px + word: Draft (info), In progress (info), Submitted (success), Not started (muted), Due soon (warning), Gap (warning), On track (success), Development required (warning), Critical gap (error). Tint bg + ink text + icon; solid variants only in report header.

**C-Progress** — track `--bg-sunken` h 8 radius pill; fill `--accent`; text right: "18 of 24 skills · 75%" `t-caption`; determinate only; `role=progressbar` + aria-valuenow/text.

**C-Wheel** — SVG 260×260 (desktop) / 200 (mobile): hexagonal radar, 6 axes = categories in fixed order; grid rings at levels 1–5 (`--bg-sunken` strokes, ring labels 1..5 as text); value polygon fill `--accent` 22% + stroke `--accent` 2px; axis end labels = category short names (`t-caption`, `--text-secondary`) + value word outside; ∅ (no data): rings only + centered text "Not assessed yet"; print: hidden, sibling `<table>` (category · level word) shown.

**C-JourneyRail** — horizontal ol; milestone dots 12px (done `--success`, current `--accent` 16px halo, future white+`--border-strong`), connector 3px `--bg-sunken` (done segment `--success`); labels below: date mono + event word; ≤640: horizontal scroll with snap, current milestone auto-centered.

**C-ProgressRail (wizard)** — vertical ol 240px: station row = dot 14px + label + right-aligned coverage text ("4/5"); done: `--success` dot + tick glyph; current: `--accent` dot + `--accent-soft` row bg + `--accent-ink` label; upcoming: hollow; reflection station last with flag glyph. ≤999 becomes **C-Stepper**: horizontal, dots + "Category 2 of 5 · Behavioral" caption, full-width, sticky under momentum header; ≤640 same, labels truncated to category short name.

**C-SkillCard** — white card radius `--r-lg` shadow-1 padding 20; contains h3, C-MeaningPanel, C-Dots + ghost, C-Textarea, gap preview. H: none (container, not clickable) — lift reserved for clickable cards. Rated state: 3px left inset bar `--accent` + tick caption "Rated"; unrated: left bar `--border`.

**C-PersonCard** — white card: C-Avatar md + name (`t-h3`, 1-line ellipsis + title attr) + role line (`t-sm` secondary, 1-line ellipsis); C-Progress slim (competency coverage); chip row: department C-CatChip-style neutral chip + gap-count chip (warning tint, "3 gaps") or success chip "On track"; status caption line ("Assessed 12 Sep 2026" / "Assessment due 30 Sep"). H: shadow-2 + translateY(−1px); F (card is a link): ring; A: translateY(0).

**C-SkillRow (profile)** — grid `1fr auto` rows: name (`t-body`, wraps 2 lines then ellipsis) | C-Dots read + level word; second row: C-Spark + gap bracket (full-width below on ≤640). Row separator 1px `--border`.

**C-Spark** — 56×18 SVG: two points (previous, current assessment average) joined by 2px line; up = `--success` + ▲ glyph, flat = `--text-muted` + ◆, down = `--warning` + ▼ + word ("+0.4 since Mar"); first assessment: single dot + "first assessment" caption.

**C-GapBracket** — the direction's signature: inline flex: [filled dots current] [bracket SVG 24×14 `--warning`] [outlined target dot] + sentence `t-sm`: "Gap: 2 levels — development plan suggested" / "Meets role expectation" (success ink + tick) / "Exceeds by 1 level" (info). Compact variant omits sentence on ≥1000 (sentence in title attr + always in profile/report). Words always present somewhere in the row.

**C-RecItem / C-RecSuggest** — list item: checkbox (suggest) or bullet, action text (`t-body`), meta row: type chip (Training / Mentorship / Certification / Practice / Stretch assignment), owner, target date mono. Suggest variant editable: select of alternative actions + date input inline.

**C-AssessCard** — row-card: avatar+name+dept | C-Progress | status chip | updated/due mono | single contextual primary button + overflow (⋯) menu with secondary actions (View report, Discard draft→modal).

**C-Timeline** — vertical ol, 2px `--bg-sunken` spine, event dots; item: cycle label, evaluator, status chip, delta caption ("overall +0.6"), link "View report".

**C-Avatar** — sizes sm 28 / md 40 / lg 56 / xl 80; radius 50%; photo `object-fit:cover`; missing photo → monogram initials on `--accent-soft` with `--accent-ink` (deterministic tint by id from category palette allowed); broken image fallback = monogram (onerror swap); ring 2px white + 1px `--border`.

**C-Toast** — bottom-center, white card shadow-2 radius `--r-md`, icon + text + optional action link; auto-dismiss 4s (none for error); enter/exit translateY 8px + fade `--dur`; region `aria-live=polite`; max 2 stacked.

**C-Modal** — overlay rgba(18,40,58,.45); panel white radius `--r-md` shadow-3 max-w 440; title `t-h2`, body `t-body`, actions right-aligned (ghost cancel + primary/destructive confirm); F: trap, Esc closes, focus returns to trigger; backdrop click = cancel.

**C-Sheet (mobile ≤640 & setup on all sizes)** — bottom-anchored panel radius top `--r-md`+`--r-md`, shadow-3, slide-up `--dur-slow`; drag handle bar; same focus rules as modal; used for: assessment setup, filters on mobile, comments overflow.

**C-FilterBar** — white card radius `--r-lg` padding 12; row of C-Search + selects + C-Switch + result count caption; ≤640: collapses to search + "Filters" button opening C-Sheet with the controls stacked; active-filter chips row appears below with ×-to-clear.

**C-Switch** — 44×24 track pill; off `--bg-sunken` + `--border-strong` border; on `--accent`; knob 18 white; label text beside (never icon-only); role=switch.

**C-Empty** — centered block in card: 24px stroke icon, `t-h3` line, `t-sm` helper line, single CTA button; variants: no employees, no results (filters), no assessments, no data in wheel, no gaps ("All skills meet role expectations" + success tick).

**C-Skeleton** — `--bg-sunken` bars radius pill, shimmer disabled under reduced motion (static tint instead); layouts mirror card structure (avatar circle + 2 lines; wheel = hexagon outline); shown when render >300ms (simulated latency in prototype).

**C-Accordion (settings/chapters on mobile)** — header button (chevron rotate 180, `--dur`), panel max-height transition `--dur`; one-open-at-a-time in settings; chapters independent; `aria-expanded/controls`.

---

## 5. RESPONSIVE BEHAVIOR (explicit transformations)

**Topbar**: ≥1000 full tabs inline; 641–999 tabs inline if they fit else drawer (measure: at ≤820 switch to drawer); ≤640 drawer + condensed brand + avatar only; search becomes icon opening full-width overlay row.

**Home**: ≥1000 hero 5/7, shelves 2-col; 641–999 hero stacks (wheel card full, now-next full), shelves 2-col until 800 then 1-col; ≤640 single column order: pagehead → now-next (actions first!) → wheel → journey (scroll-snap) → strengths → focus → feed (3 items + view all). Rationale: on mobile the action and status matter before the visual.

**People**: ≥1000 wall grid 3–4 cols; 641–999 2 cols; ≤640 **list rows** (avatar left, name/role middle, gap chip + chevron right; progress as 4px underline bar) — cards do not merely shrink.

**Profile**: chapters full-width at all sizes; ≤640 skill rows stack (name → dots+word → spark/gap line); profile header: avatar+name row, meta becomes 2-col dl, actions become sticky bottom bar (primary + overflow).

**Wizard**: ≥1000 rail left 240px sticky; 641–999 rail becomes horizontal C-Stepper above station; ≤640 stepper compact + momentum header condenses to "2/5 · 60% · Saved"; skill cards full width; dots remain 44px targets; textarea full width; station bar sticky bottom with safe-area padding.

**Report**: ≥1000 metadata 3-col; ≤999 2-col; ≤640 1-col key/value rows; wheel 200px centered; tables become grouped definition lists at ≤640 (skill name bold, current/expected/gap as labeled rows); print always single-column A4 with page-break rules.

**Settings**: tables → stacked group cards at ≤640 (role select sticky top of card).

---

## 6. INTERACTION SPECIFICATION

- **Hover**: clickable cards lift −1px + shadow-2; buttons darken one step; chips tint one step; links underline; non-clickable containers (skill card, meaning panel) do not react.
- **Focus-visible**: 2px accent ring offset 2 everywhere; drawer/modal trap; sequential DOM order matches visual order (rail before station).
- **Active**: buttons translateY(1px); chips scale .98; dots scale .96 then settle.
- **Loading**: button spinner + aria-busy; page-level skeletons; autosave text cycle "Saving… → Saved 14:03" (aria-live polite, text not color).
- **Success**: toast for submit/save-changes; station dot fills `--dur-slow`; submitted chip swaps in place.
- **Error**: inline field message + summary block on reflection listing unrated skills with jump links; toast error persists until dismissed; network-less prototype: localStorage quota failure → toast error "Could not save locally".
- **Empty/∅**: C-Empty variants per §4; wheel ∅; spark "first assessment"; no gaps → success empty state.
- **Disabled**: 40% opacity, not-allowed, reason exposed (title + inline caption for submit button: "3 skills left to rate").
- **Autosave**: wizard changes debounce 800ms → localStorage; `beforeunload` guard only when dirty & unsaved.
- **Keyboard map**: Tab/Shift+Tab natural; arrows within dots (native radios); Enter/Space activate; Esc closes sheet/modal/popover; Ctrl/Cmd+S saves draft in wizard; "/" focuses search; anchor chips = links (Enter jumps).
- **Scroll**: chip nav smooth-scroll except reduced motion; sticky elements: topbar, momentum header, station bar (mobile), report toolbar.
- **Transitions**: only color/background 120ms, transform/shadow 180ms, structural 240ms; no parallax, no scroll-jacking, no decorative animation. Wheel draws once per page load (stroke-dashoffset 240ms), skipped under reduced motion.

---

## 7. STATES & EDGE CASES MATRIX

| Case | Behavior |
|---|---|
| Long employee/skill names | 1-line ellipsis + `title` on cards; 2-line clamp then ellipsis in rows; report wraps fully |
| Missing/broken photo | Monogram initials, deterministic tint; `onerror` swap; alt = name |
| No assessment data | Wheel ∅ text; dots unfilled read-only; history C-Empty; dashboard primary = "Start assessment" |
| Incomplete assessment | Coverage progress everywhere; "Continue" resumes at first unrated skill's station; draft chip |
| Rating equals/exceeds expectation | Gap line: success "Meets…" / info "Exceeds…"; excluded from focus shelf & gap report section |
| Large employee list | Page size 24 + "Load more"; search debounce; count caption "Showing 24 of 187" |
| Large skill list (>12/category) | Chapter cards scroll naturally; category header sticky within card on desktop; wizard stations unchanged (one category per screen keeps cognitive load flat) |
| 320px screens | Container padding 12; chips wrap; dots keep 44px via 2-row wrap; tables fully stacked; no horizontal scroll except journey rail & wheel labels (both snap/scroll-afforded) |
| Slow loading | Skeletons ≥300ms; buttons L state; no layout shift (reserved heights for wheel/avatar) |
| Validation | Setup: ≥1 category required (inline error); wizard submit: 100% coverage of selected categories; settings: unique non-empty skill names, level required; dates: target ≥ today (warning not error if past) |
| Concurrent draft (same employee) | Single draft per employee enforced in store; opening existing draft shows info toast "Resuming draft from 12 Sep" |
| Print with no data | Report blocks generation: button disabled + reason; queue row action hidden |

---

## 8. ACCESSIBILITY (WCAG 2.2 AA — testable)

1. Landmarks: `header/nav/main/footer(as page foot)`, one `h1` per page, no skipped levels. *Test: axe headings/landmark rules pass.*
2. All interactive controls are native (`button, a, input, select, textarea`) or ARIA-correct (radiogroup dots, listbox switcher, switch). *Test: tab through every page; every stop has visible ring and accessible name.*
3. Dots: real radio inputs → arrow-key semantics free; `fieldset/legend` = skill name; selected word rendered visibly adjacent. *Test: keyboard-only rating completion.*
4. No color-only meaning: chips carry words + icons; dots carry adjacent words; gap carries sentence + bracket geometry; wheel has text labels + print table. *Test: grayscale screenshot review — all statuses still distinguishable.*
5. Contrast: all pairs §3.1 ≥4.5:1 text, ≥3:1 meaningful borders/dots. *Test: contrast audit against token table.*
6. Forms: labels visible; errors `aria-describedby` + `aria-invalid`; reflection summary links focus to field; required = word "(required)" visually hidden + asterisk-free.
7. Live regions: autosave text, toasts, meaning panel = `aria-live=polite`; submit errors = `role=alert`.
8. Motion: `prefers-reduced-motion` kills lifts/draws/smooth-scroll; content never auto-moves otherwise.
9. Touch: ≥44px targets (dots via padding, icon buttons, switch, chips-in-forms).
10. Modals/sheets: focus trap + Esc + focus restore; backdrop `aria-hidden` sibling content via `inert`.
11. Skip link first in DOM; visible on focus.
12. Print report: wheel→table swap, black-on-white, `page-break-inside: avoid` per section.

---

## 9. DATA MODEL & SEED CONTENT (realistic, fictional)

```
Level { id 1..5, label, definition }            // Beginner..Expert, editable in Settings
Category { id, name, tintKey, blurb, skills[] } // 6 fixed: Technical, Behavioral, Management, Leadership, Compliance, Social Impact
Skill { id, categoryId, name }
Role { id, title, expectations: { skillId: levelId } }
Employee { id "EMP-1042", name, photo?, roleId, department, email, joinedAt, managerId }
Assessment { id, employeeId, evaluatorId, cycle "2026-H2", status: draft|submitted,
             createdAt, updatedAt, submittedAt?, categoryIds[],
             items: { skillId, levelId?, expectedLevelId, comment? }[],
             summary?, recommendations: { text, type, ownerId?, due? }[] }
Event feed derived from assessments.
```
Seed: 12 employees across Engineering, Product, Design, People Ops, Data; departments map to roles (Senior Engineer, Product Manager, UX Designer, People Partner, Data Analyst…); 4–6 skills per category (e.g. Technical: "JavaScript / TypeScript", "System design", "Cloud & CI/CD", "Code review quality"; Behavioral: "Communication", "Teamwork", "Adaptability", "Time management", "Problem solving" — vocabulary aligned with reference domain, rewritten originally); 4 assessments: 1 submitted complete (Ananya Menon, evaluator R. Iyer, comments + 4 recommendations), 1 draft ~60% (Rohan Deshpande), 1 draft 15%, 1 not started; activity events with believable dates (Aug–Sep 2026). Level definitions example — Intermediate: "Handles day-to-day work independently; needs help only on unusual problems."

---

## 10. VANILLA JS ARCHITECTURE (no build)

```
index.html
css/ tokens.css · base.css · components.css · pages.css · print.css
js/  data.js (seed) · store.js (localStorage v1 read/write, migrations key) ·
     router.js (hash routes, render dispatch, scroll restore) ·
     ui.js (component builders: chip, dots, wheel SVG, spark, toast, modal, sheet) ·
     pages/ home.js · people.js · profile.js · assessments.js · wizard.js · report.js · settings.js ·
     gaps.js (delta + severity rules: 0 meet, 1 monitor→"development suggested", ≥2 or critical-role skill → "development required") ·
     app.js (boot, topbar, view-as switcher)
```
Rules: event delegation on `main`; render = pure string/DOM builders from state; no global CSS-in-JS; all icons inline SVG sprite in `index.html`; localStorage schema-versioned (`pathway.v1`); "Reset demo data" in Settings footer.

---

## 11. QA CHECKLIST (senior review gate before done)

- [ ] Every route reachable by keyboard only, end-to-end assessment completable without mouse.
- [ ] Grayscale pass: statuses/gaps still unambiguous.
- [ ] 320 / 375 / 768 / 1024 / 1440 screenshots: no overflow, no shrunken-desktop layouts, sticky bars don't overlap (safe-area).
- [ ] Token audit: no hex values outside tokens.css in component/page CSS.
- [ ] States audit: every component in §4 demonstrated in D/H/F/A/X/L/S/E/∅ where defined (spec sheet file).
- [ ] Print: report = clean A4, wheel table swapped, no nav/toolbar ink.
- [ ] Reduced motion: no transforms/draws; content identical.
- [ ] localStorage: reload mid-wizard resumes exact station & values; quota error path toast.
- [ ] Human test: hierarchy obvious, one primary per view, density calm, reads as a real L&D product — not a template.

---

*End of specification. On approval, Phase 6 implements exactly this document with HTML5 + CSS3 + vanilla JavaScript only.*
