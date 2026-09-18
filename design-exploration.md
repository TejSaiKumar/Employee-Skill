# Employee Skills Assessment & Skill Management Platform
## Phase 1–3: Reference Reverse-Engineering, Design DNA, and Design Directions

Reference artifact: `Employee Skill Assessment Template _ Free Download (Paged).pdf` — 7 pages, A4 portrait (595 × 842 pt), a paged capture of a single scrolling web page (the Edstellar "Employee Skills Assessment Template" resource page).

Legend used throughout Phase 1:
- **OBSERVATION** — directly visible/measurable in the PDF (pixels, embedded fonts, extracted text).
- **INFERENCE** — reasoned interpretation, not explicitly provable from the PDF.

---

# PHASE 1 — REFERENCE ANALYSIS

## 1.0 Capture provenance (important context for everything below)

- **OBSERVATION:** All 7 pages are slices of one continuous vertical page: form fields split mid-fieldset across page breaks (e.g. "Evaluator Name *" appears at the bottom of page 1 and top of page 2; "Email *" at the bottom of page 2 and top of page 3).
- **OBSERVATION:** The header (page 1) shows logo + search icon + hamburger icon only — no horizontal navigation links. Buttons (page 4) are full-width and stacked. Footer link groups (pages 5–6) run in 1–2 columns.
- **INFERENCE:** The capture viewport was narrow (≈790–800 CSS px, i.e. tablet/large-phone breakpoint), so the PDF predominantly shows the reference's *responsive* presentation, not its desktop composition. Desktop layout must therefore be inferred cautiously or not at all.
- **OBSERVATION:** Page 7 shows a cookie-consent overlay and a floating chat launcher with an "Currently Away / Offline now, write to us" tooltip — third-party overlays captured mid-session.

## 1.1 Page-by-page observations

### Page 1 — Header, title block, first fieldset
- White header band: concentric-ring logo mark + lowercase wordmark at left; search (magnifier) icon and 3-line hamburger at right. Thin gray hairline under the header.
- Light gray page band containing a single centered white content card with generous internal padding.
- Inside the card, top: a wide banner box — pale blue fill (`#F0F6FE`), 2 px chartreuse border (`#B4CF33`), ~10 px radius — containing only a short chartreuse dash (an eyebrow/breadcrumb placeholder).
- Page title: bold royal-blue H1 ("Employee Skills Assessment Template") followed by a full-content-width chartreuse rule (~3 px).
- Subtitle: two lines of gray (`#666666`) body text, loose line height.
- First fieldset panel: very pale blue-gray tint (`#F8FAFD`-ish), 1 px light border, ~8 px radius; cyan-blue bold panel heading ("Company & Evaluator Information").
- Fields: "Company Logo" label (blue, semibold) above a lime-bordered box wrapping a *native* file input ("Choose File / No file chosen"); "Company Name *"; "Assessment Date *" as a native date input (value 18-09-2026, calendar icon at right); "Evaluator Name *" begins at the page break.
- Inputs: full-width, ~48 px tall, ~8 px radius, near-white fill, thin chartreuse border.

### Page 2 — Fieldset continuation + second fieldset
- "Evaluator Name *", "Evaluator Designation *" complete the first panel.
- Second panel "Employee Information" (same tinted-panel pattern): "Employee Photo" (native file input in lime-bordered box), "Employee Name *", "Employee ID *", "Designation *", "Email *".
- Consistent vertical rhythm: label → control → ~28–32 px gap; ~40–48 px between panels.

### Page 3 — Employee fields complete + category selector
- Remaining employee fields: "Department *", "Job Title/Position *", "Date of Joining" (native date input, `dd-mm-yyyy` placeholder).
- Third panel: "Select Skill Categories to Assess" (cyan heading) + an information callout: pale blue fill with 1 px blue border and blue body text ("Select one or more categories below. You can also add custom skills within each selected category.").
- Category cards begin: white cards, 1 px gray border (`#E1E7EC`), ~8 px radius; each = native checkbox + blue bold category name + gray comma-separated skill list. First: "Behavioral Skills — Communication, Teamwork, Adaptability, Time Management, Problem Solving".

### Page 4 — Category list completes + action triad
- Remaining category cards: Technical (Job-specific Expertise, Software Proficiency, Technical Knowledge, Tools Mastery); Management (Analytical Thinking, Leadership, Resilience, Creative Thinking, Team Management); Leadership (Strategic Vision, Decision Making, Team Building, Mentoring, Change Management); Compliance (Regulatory Knowledge, Policy Adherence, Risk Awareness, Ethical Standards); Social Impact (Sustainability, Diversity & Inclusion, CSR, Community Engagement, Ethics).
- Six category cards total, each pairing a bold category name with a plain-language list of its skills.
- Below the card, three full-width stacked buttons, equal size, ~8 px radius, ~56 px tall:
  1. "Save Progress" — chartreuse `#B4CF33` fill, dark text.
  2. "Generate Report" — royal blue (≈`#2461D9`) fill, white text.
  3. "Clear Form" — teal `#0FB981` fill, white text.

### Page 5 — Footer contact band + footer body
- Darker navy contact band (`#232558`): "Reach Us:" + email with envelope icon; three phone numbers prefixed by country flag glyphs (US, UK, India).
- Footer body navy (`#2D2E6A`): white logo; one-paragraph company description; "FOLLOW US" label + five rounded-square social tiles (lighter navy `#383972`); "Quality and Compliance" with two chartreuse check-circle icons (ISO 9001:2015, ISO 27001:2022); two-column link groups with bold white headings ("Learning & Development", "Organizational Development") and light-lavender links.

### Page 6 — Footer link columns continue
- Further groups: "Talent Assessments" (Behavioral & Psychological Profiling; **Skill Gap & Competency Benchmarking**; Organizational Alignment & Multi-Rater Feedback; Assessment and Development Center), "Training Needs Analysis Services", "Skills-based Organization", "Training Programs" (IT & Technical, Management, Leadership, Behavioral, Compliance, Social Impact, "Browse All 2,000+ Courses"), "Resources" (Blog, L&D Resources, Sitemap), "Company" (About us, Who we Serve, Pricing, Training Delivery, Partner, Careers, Contact us), "Trainers".
- **OBSERVATION (domain signal):** the reference vendor's own service taxonomy includes skill-gap benchmarking, competency mapping, and training-needs analysis — the same problem space as our product, confirming the category vocabulary (behavioral / technical / management / leadership / compliance / social impact) is industry-plausible.

### Page 7 — Footer tail + overlays
- Tail links (Trainer, Join as Trainer, Write for Us), copyright line.
- Cookie-consent overlay: dark slate panel (`#2D4054`), white text, "Learn more" underlined link, solid-white "Accept All" button and white-outlined "Preferences" button.
- Floating chat launcher: green circle with speech-bubble glyph, bottom-right, with an offline tooltip card.

## 1.2 Layout analysis

- **OBSERVATION:** Single centered content column inside a white card on a light band; content width ≈ 66–70 % of the captured viewport; no multi-column working area anywhere in the capture.
- **OBSERVATION:** Information is chunked into stacked, self-contained panels (fieldsets) with tinted backgrounds and their own headings — a "sectioned document" rhythm rather than a grid of cards.
- **OBSERVATION:** Header is a thin utility band (logo + icon actions); footer is a deep, multi-group mega-footer; the working area between them is one long scroll.
- **OBSERVATION:** Alignment is strictly left-aligned; the only centered elements are button labels and the cookie-overlay text.
- **INFERENCE:** On desktop the same page likely widens the container and possibly two-columns the fieldsets, but the PDF provides no evidence for this; we must not assume a desktop composition we cannot see.
- **INFERENCE:** The sectioned-panel rhythm (panel = heading + related fields) is the reference's core organizing idea and is breakpoint-independent — this is the principle worth carrying forward.

## 1.3 Typography analysis

- **OBSERVATION:** Embedded font set includes `ArialMT`; remaining text uses subset (Type3) fonts whose rendered letterforms are a neutral neo-grotesque consistent with the Arial/Helvetica family. No serif, no display face, no monospace.
- **OBSERVATION:** Measured type sizes (PDF pt): 23.9 (page title), 19.9/17.9 (panel headings / footer group headings), 15.9/15.1 (sub-headings, footer links), 13.9 (labels, body), 12.0 (small text).
- **INFERENCE:** At the capture scale this maps to an approximate web scale of 32 / 26 / 21 / 18 / 16 px — a gentle ~1.2–1.25 ratio, i.e. hierarchy by modest size steps plus weight and color, not dramatic scale contrast.
- **OBSERVATION:** Weights used: regular (body, descriptions) and bold (titles, labels, headings). Labels are bold blue; descriptions are regular gray; this color+weight pairing does the work of a larger type scale.
- **OBSERVATION:** Line height is loose (≈1.6–1.7 for body/descriptions); letter-spacing appears default; text density is low — few words per screenful.
- **OBSERVATION:** Required fields are flagged with a trailing asterisk in the label; no legend for the asterisk is visible.

## 1.4 Color analysis (sampled hex values)

| Role | Hex | Where observed |
|---|---|---|
| Canvas / header / cards | `#FFFFFF` | header, content card, category cards |
| Page band behind card | light warm gray (≈`#ECEDEF`) | margins around content card |
| Panel tint | `#F8FAFD`–`#F9FAFD` | fieldset panels, input fills |
| Primary blue | `#2461D9` | H1, field labels, primary button |
| Secondary cyan-blue | `#0EA5E9` | panel headings |
| Accent chartreuse | `#B4CF33` | title rule, banner border, input borders, "Save Progress", logo ring, ISO checks |
| Tertiary teal | `#0FB981` | "Clear Form" |
| Callout fill / border | `#E7F1FF` / blue 1 px | info callout, top banner (`#F0F6FE` fill) |
| Card border | `#E1E7EC` | category cards |
| Body gray | `#666666` | subtitles, skill lists |
| Muted lavender-gray | `#B8BAC8` | footer-adjacent muted text |
| Ink | `#1C1C1C` / `#000000` | native control text, chat tooltip |
| Footer navy | `#232558` (band) / `#2D2E6A` (body) / `#383972` (tiles) | footer |
| Cookie overlay | `#2D4054` | consent panel |

- **OBSERVATION:** Three saturated action colors (chartreuse, royal blue, teal) sit on an otherwise white/pale-blue canvas; navy appears only in the footer.
- **INFERENCE:** The palette intent is "friendly corporate": blue = trust/primary, chartreuse = brand spark, teal = safe/neutral action. The equal-weight treatment of the three buttons, however, cancels most of that hierarchy (see 1.9).

## 1.5 Component inventory

**OBSERVED components:** utility header (logo + search + hamburger); bordered banner/eyebrow box; underlined page title; tinted fieldset panels with colored headings; text labels with required asterisks; text inputs; native file input wrapped in styled box; native date inputs; info callout; selectable category cards (checkbox + title + description); three stacked full-width buttons; mega-footer with contact band, social tiles, certification badges, link columns; cookie consent overlay; chat launcher with tooltip.

**NOT observed anywhere in the capture:** tables, tabs, modals, drawers, tooltips on content, breadcrumbs, pagination, progress indicators, empty states, loading states, avatars, charts, badges/chips, hover states (static capture), skill ratings or proficiency scales (the template captures only category *selection*, not rating).

## 1.6 Interaction language

- **OBSERVATION:** Interaction affordances visible are native browser controls (file picker, date picker, checkboxes) — the page delegates control behavior to the platform.
- **OBSERVATION:** Overlay patterns exist: cookie consent (blocking panel) and chat (persistent floating launcher).
- **INFERENCE:** Hover/focus/transition behavior cannot be observed in a static capture; any claim about them would be fabrication. The native-control choice implies default browser focus rings and default hover behavior on those controls.
- **INFERENCE:** The single long scroll + sectioned panels implies a linear, top-to-bottom completion model: the user is expected to fill everything in order and act once at the end (Save / Generate / Clear).

## 1.7 Visual personality

**Verdict: corporate-utility with a friendly tri-accent palette — a marketing-clean "template" page wrapped around an honest, unadorned form.**

Why: the canvas discipline (white card, pale tints, hairline borders, generous whitespace) reads SaaS-marketing clean; but the working area uses raw native controls, asterisk-required labels, and stacked equal-size buttons — the vocabulary of a practical downloadable tool, not a designed product UI. The chartreuse rule and tri-color buttons add approachability without playfulness. There is no editorial typography, no experimental layout, no data-viz personality.

## 1.8 Accessibility observations

- **OBSERVATION:** Every input has a visible text label above it; categories pair a bold name with a plain-language description (good for non-expert evaluators); touch targets are large (≈48–56 px); body gray `#666666` on white ≈ 5.7:1 (passes AA); primary blue on white ≈ 4.9:1 (passes AA); white on footer navy passes easily.
- **OBSERVATION:** Cyan panel heading `#0EA5E9` on tint `#F8FAFD` ≈ 2.9:1 — below AA for normal text (mitigated only by its large bold size); chartreuse input borders on white ≈ 1.9:1 — borders carry meaning (field boundary) with insufficient contrast; button importance is communicated by color alone.
- **OBSERVATION:** Required-state is an asterisk with no visible explanation; no skip link, no visible focus indicators (static capture), no stated semantics.
- **INFERENCE:** The reference prioritizes legibility and target size over systematic contrast discipline — a common pattern in marketing-built forms.

## 1.9 Design DNA

### A. Visual philosophy
The core visual idea is **"a clean sheet of paper with colored margins of meaning"**: one white document on a quiet band, with color used as annotation (blue = label/primary, cyan = section, chartreuse = brand accent/rule) rather than as surface. **OBSERVATION** for the palette roles; **INFERENCE** for the "annotation" reading.

### B. Layout philosophy
**Stacked, self-describing sections.** Each panel states what it is ("Company & Evaluator Information") and contains only its own fields. One column, one direction of travel, no competing regions. (OBSERVATION)

### C. Typography philosophy
**Hierarchy by color and weight first, size second.** A modest 5-step scale; bold blue labels vs regular gray descriptions create scannability without large type. Loose leading keeps the long form calm. (OBSERVATION)

### D. Color philosophy
**Restraint in surfaces, confidence in accents.** Surfaces stay white/pale; three saturated colors appear only on interactive or branding elements; deep navy is reserved for the footer, giving the page a "frame". Emphasis comes from accent placement, not from shadows or gradients. (OBSERVATION + INFERENCE on intent)

### E. Component philosophy
**Bordered containers everywhere, native controls inside.** Panels, inputs, cards, and buttons are all rounded rectangles with 1–2 px borders; behavior is delegated to native inputs. Components are honest and simple but stylistically uneven (styled box around an unstyled file input). (OBSERVATION)

### F. Interaction philosophy
**Linear completion with a terminal action triad.** The user progresses top-to-bottom and resolves the task with one of three end buttons. Overlays (cookie, chat) are bolted on, not integrated. (OBSERVATION for structure, INFERENCE for intent)

### G. Spacing philosophy
Recurring rhythm: ~16 px label→control, ~28–32 px between fields, ~40–48 px between panels, ~64–80 px card padding, full-width controls. A consistent 4/8 px-based vertical system. (OBSERVATION measured from renders; the 4/8 base is INFERENCE)

### H. Accessibility philosophy
Visible consideration: labels everywhere, descriptive category text, large targets, sane body contrast. Gaps: low-contrast meaningful borders, color-only button hierarchy, unexplained asterisks. (OBSERVATION)

### I. Strengths (worth carrying forward)
1. Sectioned panels that name themselves — instant orientation in a long task.
2. Plain-language skill lists under each category — lowers evaluator expertise barrier.
3. Calm single-column reading rhythm with generous whitespace.
4. Clear label/required convention and large touch targets.
5. Accent discipline on surfaces (white/pale) keeps forms legible.
6. Information callout explaining what to do before a complex selection. (All OBSERVATION)

### J. Potential problems (do NOT copy)
1. **Three equal-weight stacked primary-colored buttons** — ambiguous primary action; importance signaled by color alone.
2. **One long undifferentiated scroll** for a multi-part task — no progress, no save-state visibility, no way to resume; the exact "long boring questionnaire" failure mode.
3. **Unstyled native controls inside styled wrappers** — inconsistent component language.
4. **Low-contrast meaningful borders** (chartreuse input borders, cyan headings on tint).
5. **Single narrow column wastes desktop** and forces serial data entry where parallel comparison (current vs expected) is needed.
6. **No proficiency/rating model visible at all** — category selection only; the assessment heart of the problem is absent.
7. **Asterisk-required without legend; no validation, empty, loading, or error states visible.**
8. **Mega-footer and cookie/chat overlays** are marketing-site baggage irrelevant to a product UI. (All OBSERVATION; the "failure mode" framing is INFERENCE)

---

# PHASE 2 — EMPLOYEE SKILLS PRODUCT INTERPRETATION

## 2.1 Users
1. **HR / L&D administrator** — configures categories, skills, proficiency model, role expectations; monitors completion.
2. **Evaluator (manager / team lead)** — runs assessments, rates skills, writes observations, issues reports.
3. **Employee** — views own skill profile, gaps, and development recommendations.
4. **Leadership (secondary)** — reads organization-level competency and gap summaries.

## 2.2 Main goals
- Maintain a truthful, current map of each employee's skills and proficiency.
- Make assessment a structured, resumable, auditable activity — not a form marathon.
- Surface **current → expected → gap** instantly and accessibly.
- Turn gaps into concrete development recommendations and a shareable report.

## 2.3 Information architecture (product-level, design-agnostic)
- **Dashboard** (employee-centric or org-centric per design direction)
- **Employees** (list, search, filter) → **Employee profile** (identity + skill record)
- **Assessments** (queue + wizard: employee → categories → ratings → review → submit)
- **Skill gaps** (current vs expected vs delta, filterable)
- **Recommendations / development plans**
- **Reports** (on-screen, print, PDF)
- **Settings** (skill library, categories, proficiency model, role expectations)

## 2.4 Core data
`Employee` (name, id, photo, designation, department, position, email, joining date, manager) · `Role` (expected proficiency per skill) · `SkillCategory` → `Skill` · `Assessment` (evaluator, date, status: draft/submitted) · `AssessmentItem` (skill, current level, expected level, comment, improvement flag) · `Recommendation` (action, type, target skill) · `Report` (compiled snapshot).

## 2.5 Proficiency model
Default 5-level scale — **Beginner → Developing → Intermediate → Advanced → Expert** — each level carrying a plain-language definition (the reference's habit of explaining categories in plain words, applied to levels). The model is presented as configurable in Settings (labels and level count editable), never hard-assumed.

## 2.6 Primary vs secondary actions
- **Primary:** Start/continue assessment · Rate skill · Save draft · Submit & issue report.
- **Secondary:** Add comment · Add custom skill · Filter/search · Export/print · Edit expectations.
- Hierarchy rule learned from the reference's mistake: exactly **one** primary action per view; destructive/rare actions demoted to menus or ghost buttons.

## 2.7 The three workflows
1. **Assessment:** select employee → confirm context → per-category rating pass (current level + comment, expected shown as reference) → review sheet → recommendations → submit → report.
2. **Gap:** for each rated skill compute delta(current, expected); classify (Meets / 1 level / 2+ levels / Exceeds); rank by severity × role criticality; attach recommendation.
3. **Reporting:** compile employee details, category tables, gap schedule, recommendations, evaluator comments into a print/PDF-ready document.

---

# PHASE 3 — DESIGN DIRECTIONS

Six genuinely different directions. Each answers: philosophy, visual style, navigation, dashboard, profile, assessment UI, gap visualization, reports, mobile, interaction, reference influence.

---

## OPTION A — "Competency Ledger"

**1. Design philosophy.** Treats every assessment as an official organizational record — a document with clauses, marginalia, and a signature line. The interface behaves like well-set paperwork: quiet paper surfaces, ruled tables, numbered sections, and a single brand gesture (a chartreuse rule) used like a letterhead device. It inherits the reference's deepest truth — that this content is a *document people must trust and re-read* — while fixing its failure modes with a table-of-contents, visible draft state, and one action per moment.

**2. Visual style.** Warm paper canvas `#F7F6F3`; white "sheet" surfaces with hairline `#E4E1DA` borders and a barely-there shadow; ink-navy text `#1C2B4A`; secondary `#5A6478`; one royal-blue interactive accent `#2456C4`; chartreuse `#B4CF33` reserved for the title rule and section numerals only. Neo-grotesque body (system stack), 15–16 px, line-height 1.6; headings bold with tight tracking. Radius 4–6 px; rules (1 px) do the separating instead of shadows; medium-low density; tabular figures in all tables.

**3. Navigation.** Slim top bar: wordmark left; document-style nav center (Dashboard · Employees · Assessments · Reports · Settings); evaluator identity right. No sidebar. Inside assessment and report views, a sticky left **table of contents** (categories/sections with completion ticks) replaces global nav — the document owns the screen.

**4. Dashboard.** A "cover sheet": employee selector rendered as a document header block (name, ID, department, role, evaluator, cycle date); beneath, a three-item summary strip (Overall competency index · Skills assessed · Open gaps) as ruled stat cells, not cards; then two columns: left = category proficiency table (category, level word, trend tick), right = "Marginalia" ledger: numbered gap entries and recommendation entries with dates. Recent assessments appear as a dated register list at the foot.

**5. Employee profile.** A dossier: left identity column (portrait in a stamped frame, metadata as label/value rows, manager, tenure); right = numbered clauses per category ("1. Technical Skills"), each a ruled table: skill · current level word · expected level word · gap note in the margin column. Long names truncate with title attribute; missing photo = initials monogram.

**6. Assessment interface.** One category per screen ("Clause 2 of 5 — Behavioral Skills"). Sticky document header: employee name, progress text ("12 of 24 skills rated"), draft-saved timestamp. Each skill row: name + plain-language level-meaning line; current level chosen via a **segmented word control** (five text segments: Beginner…Expert, radio-group semantics); expected level shown as quiet reference text at row right; "Add observation" expands an inline textarea beneath the row. Footer bar: "Previous clause" (ghost) · "Save draft" (secondary) · "Save & continue" (single primary). Final clause = review sheet: all ratings in one ruled table, recommendation checklist (curated actions per gap severity), evaluator statement textarea, then "Issue report".

**7. Skill gap visualization.** Margin-note pattern, text-first: `Intermediate → Advanced` with an arrow glyph, then a labeled chip "2 levels below expectation — development required", plus a 5-tick ruler where filled ticks = current and a hollow ring tick = expected. Color reinforces but never carries the message; every gap has words.

**8. Reports.** Print-grade letterhead: organization block, metadata grid, category tables, gap schedule, numbered recommendations, evaluator statement and signature/date lines. Dedicated `@media print` stylesheet; "Save as PDF" simply invokes print. On screen it reads exactly as it will print (WYSIWYG).

**9. Mobile.** The sheet becomes a single column; TOC collapses to a jump menu (`<select>`-styled jump list) under the header; ruled tables transform to definition lists (skill name bold, level words as stacked label/value pairs, margin notes become inline notes); segmented word control wraps to two rows of chips; the review sheet stacks clause by clause. Print output unaffected.

**10. Interaction style.** Hover: row tint `#F3F1EC` + rule darkens. Focus-visible: 2 px blue ring, 2 px offset. Active: 1 px press translate. Loading: skeleton rules (gray bars). Success: "Draft saved 10:42" text swap in header, no animation. Error: inline red-brown message under the field + summary list on review. Empty: ruled placeholder row "No assessments recorded for this employee yet." Transitions 120–160 ms on color/background only; `prefers-reduced-motion` disables them.

**11. Why this direction fits the reference.** The reference *is* a document: underlined title, self-naming sectioned panels, plain-language category descriptions, and a terminal "Generate Report" act. Ledger keeps that documentary soul (panels → clauses, title rule → letterhead rule, generate → issue) and repairs the hierarchy and progress failures.

---

## OPTION B — "Skillframe Matrix"

**1. Design philosophy.** Competence is a grid to be worked, not a story to be told. Skillframe is an operator's workspace for HR power users: the competency matrix (employees × skills × levels) is the home screen, and assessment is spreadsheet-fast cell editing. Density, keyboard flow, and filterability beat decoration. It takes the reference's honest utilitarianism — controls that do exactly one job — and gives it a consistent, professional skin.

**2. Visual style.** Cool canvas `#F4F5F7`; white surfaces; 1 px `#E2E5EA` borders; radius 4 px; shadows essentially none (one for popovers). UI type 13–14 px neo-grotesque with tabular numerals; headers 600 weight; monospace (`ui-monospace`) for employee IDs and level codes. Single interactive accent `#1F5ED8`; status palette (green/amber/red) always paired with text labels. High density: 32–36 px row heights, 8 px gutters.

**3. Navigation.** Persistent left sidebar: Dashboard · Employees · Skill Matrix · Gaps · Assessments · Reports · Settings, each with count badges (e.g. "Assessments 7"); collapsible to icon rail. Top bar: global search (employees, skills, departments — one box, grouped results), cycle selector, evaluator menu. Breadcrumbs only inside nested views.

**4. Dashboard.** An org cockpit in three horizontal bands: (1) thin stat strip — employees, assessments completed, pending, median gap index — as inline figures separated by rules, not cards; (2) department × category heat table: cells carry the value as text plus a background tint (never color alone); (3) right-hand queue rail: "Awaiting your assessment" list with due dates and one-click "Open".

**5. Employee profile.** Header bar: monogram/photo, name, ID (mono), department chip, status chip ("Assessment in progress"), actions right ("Continue assessment", "Report"). Body: one grouped table — category group rows, then skill rows with columns Skill · Current · Expected · Gap · Last assessed · Observations (icon + truncated text, expand in place). Column sort and category filter chips above.

**6. Assessment interface.** Category tabs across the top of a rating grid. Each skill is a row; the five proficiency levels are **radio cells** in a matrix column set — click or keyboard (arrows to move, 1–5 to rate). Expected level renders as a faint ring behind the cell so current-vs-expected is visible while rating. A comment toggle per row expands an inline full-width editor row. Header shows autosave state ("Saving… / Saved 14:03") and coverage ("18/24 rated"). "Review & submit" becomes enabled at 100 % coverage; review is a diff table of all ratings with edit jumps.

**7. Skill gap visualization.** In-matrix: current level as a filled dot on a 5-step cell ruler, expected as an outlined target dot; the Gap column states "−2 levels" plus a severity label chip (On track / Monitor / Development required / Critical). A "Gaps only" filter and gap-severity sort make the matrix a gap-finding instrument.

**8. Reports.** Workspace-export style: compact header block, grouped tables identical to screen tables (no restyling), gap schedule, recommendations table; toolbar with Print / Export PDF / Copy summary. Print CSS strips sidebar/topbar and forces black-on-white.

**9. Mobile.** Sidebar → 5-item bottom tab bar (Dashboard, Employees, Matrix, Queue, More). The matrix is the interesting transformation: it becomes **per-skill stacked cards** — skill name, 5-step horizontal segmented ruler for current (44 px targets), expected shown as a labeled marker under the ruler, gap line, comment button opening a bottom sheet. Tables elsewhere become card lists with primary columns only.

**10. Interaction style.** Hover: row tint `#F7F8FA`; cell hover shows level tooltip. Focus-visible: 2 px accent ring inside cell. Active: cell fill flash. Loading: skeleton rows matching column widths. Success: rated cell flashes green 300 ms then settles; autosave text confirms. Error: red cell outline + row-level message; submit blocked with a fix-list popover. Empty: matrix shows a dashed placeholder grid with "No skills configured for this role — add from Settings". Disabled: 40 % opacity cells, cursor not-allowed. Transitions ≤120 ms.

**11. Why this direction fits the reference.** The reference's category→skill listings are a matrix in disguise (categories as row groups, skills as rows); its unadorned native-control honesty and explicit labels match Skillframe's functional ethic. Skillframe keeps the utility but replaces the serial scroll with parallel, comparable structure.

---

## OPTION C — "Pathway"

**1. Design philosophy.** Development, not judgment. Pathway frames assessment as a guided journey that an employee and manager can share: every screen answers "where am I, what's next, and why does it matter?". Momentum is the design material — progress rails, stations, and plain-language level meanings reduce the anxiety of being rated. It is the warmest direction, aimed at organizations where L&D culture matters more than audit culture.

**2. Visual style.** Pale sky canvas `#F1F6FA`; white cards, radius 12 px, one soft shadow (`0 1px 2px rgba(16,42,67,.08)`); ink `#12283A`; body 15–16 px friendly grotesque, line-height 1.65; accent deep teal `#0E7490` for interactive, supportive amber `#B45309` for gaps, green `#15803D` for strengths; category tint chips (six muted hues) used consistently across the product. Medium density with generous card padding (24 px).

**3. Navigation.** Top bar: product mark + primary tabs (Home · My Growth / Employees · Assessments · Reports) + search icon + avatar. Contextual sub-navigation as chip rows (e.g. inside an employee: Profile · Skills · Development · History). No sidebar; depth handled by chips and back-links.

**4. Dashboard.** A "growth map": hero row = current employee/you card with a six-spoke **competency wheel** (one spoke per category, value = average level, labeled), overall status chip, and next-review date; middle = a horizontal **journey rail** (last assessment → today → next scheduled) with milestone dots; lower = two shelves side by side: "Strengths" (skills at/above expectation, green ticks) and "Focus areas" (gaps with amber markers and suggested first action); foot = activity feed ("Priya's assessment submitted by R. Iyer · 12 Sep").

**5. Employee profile.** Story layout: identity header (photo, name, role, department, manager, tenure) with status banner; then category "chapters" as cards: chapter head = category name + tint chip + average level; body = skill rows each with a **5-dot proficiency indicator**, level word, trend sparkline (last two assessments), and an inline gap callout when below expectation ("Expected Advanced for Senior Engineer — 1 level to go").

**6. Assessment interface.** A wizard with a left vertical **progress rail**: categories as stations (done = tick, current = filled dot, upcoming = hollow). One category per screen; each skill is a card: name + "what this level means" panel (definition of the currently hovered/selected level, always visible text); current level via a 5-dot selector with the level word beside it; expected shown as a ghost dot with label; observation textarea with character guidance. Momentum header: "Category 3 of 5 · 60 % · draft saved". Final station = reflection: auto-suggested recommendations (from gap rules) as editable checklist cards, evaluator note, "Submit assessment".

**7. Skill gap visualization.** Dot-ruler duet: filled dots = current, an outlined **target dot** = expected, connected by a small bracket labeled in words ("Gap: 2 levels — development plan suggested"). Everywhere a gap appears, the sentence appears with it; color is reinforcement only.

**8. Reports.** A shareable summary page: wheel at top, category bars with level words, gap list with brackets, recommendations as a checklist with owners and target dates, evaluator comments; friendly header with both names. Print stylesheet converts wheel to a labeled table (no color dependence) and keeps one-page-fit sections.

**9. Mobile.** Progress rail becomes a horizontal stepper pinned under the header; the wheel shrinks to six labeled category chips with values; skill cards stack with 44 px dot targets; journey rail scrolls horizontally with snap; shelves stack. The wizard is the mobile-first surface: one decision per screenful.

**10. Interaction style.** Hover: card lift 1 px + shadow deepen; dot hover shows level definition inline. Focus-visible: 2 px teal ring. Active: dot scale 0.96. Loading: card skeletons. Success: station dot fills with a 180 ms ease-out; toast "Draft saved". Error: amber-red inline message + rail station marked with alert glyph. Empty: encouraging copy ("No assessments yet — start Priya's first assessment") with a single CTA. Transitions 180–220 ms, reduced-motion honored.

**11. Why this direction fits the reference.** The reference's most humane trait is explanation: every category card teaches its contents in plain words, and an info callout prepares the user before a complex choice. Pathway generalizes that teaching instinct — to levels, gaps, and next steps — and keeps the single-purpose flow, but adds the momentum the reference's endless scroll lacks.

---

## OPTION D — "Assessment Studio"

**1. Design philosophy.** Flow state for evaluators running high-volume cycles. Studio pins all context on screen at once — employee on the left, rating queue on the right — so rating never waits on navigation. It is keyboard-driven, autosaves everything, and treats an assessment as a *session* with a lifecycle (queued → in progress → review → issued). The aesthetic is instrument-panel clarity: high contrast, mono details, zero ornament.

**2. Visual style.** White canvas with graphite ink `#111827`; surfaces separated by 1 px `#E5E7EB` hairlines and a light-gray left rail `#F9FAFB`; radius 6 px; no decorative shadows (one for the command palette). Type: 14 px grotesque body, 600-weight section labels in 11 px uppercase with +0.04em tracking; `ui-monospace` for IDs, dates, level codes (B·D·I·A·E). Accent: deep green `#157F5C` for interactive/primary; gaps in amber `#B45309`; critical in `#B42318`. Density toggle (comfortable/compact) persisted in localStorage.

**3. Navigation.** Slim top bar (wordmark, command-palette trigger ⌘K, session indicator, avatar) + left **icon rail** expanding on hover/click: Dashboard, Roster, Sessions, Skills Library, Reports, Config. Command palette navigates anywhere by name (employees, skills, actions) — vanilla-JS fuzzy list.

**4. Dashboard.** A **session board**: four columns — To assess · In progress · In review · Issued — each a list of session cards (employee monogram, name, department, category coverage bar, due date). Columns are plain lists with counts, not kanban theater; clicking a card opens the session in the split view. A thin header strip shows cycle progress ("34 of 50 sessions issued").

**5. Employee profile.** Split-reading view: left pinned context card (identity, role, role-expectation summary as mono level codes per category); right scrollable evidence: skill rows grouped by category with current/expected codes, last rated date, observation excerpt; row click jumps into a live session at that skill.

**6. Assessment interface.** True split-screen. Left pane: employee context + category list with completion ticks + progress ring + autosave state. Right pane: **one skill card at a time** from the active category queue: skill name, plain-language definition, large segmented proficiency control (five labeled segments with mono codes), expected reference line ("Role expects: Advanced [A]"), observation textarea, gap preview that appears the moment a rating diverges from expectation. Keys: `1–5` rate, `Enter` next, `Shift+Enter` previous, `Ctrl/Cmd+S` save. Queue badges skills that ended with a gap. End of queue → review pane: full rating table, recommendation suggestions ranked by severity, submit → session moves to Issued and report compiles.

**7. Skill gap visualization.** Dual-meter bar inside the skill card: solid bar = current, hatched overlay bar = expected, delta stated in words at right ("−2 levels · development required"). In review, gaps sort first with severity chips; nothing relies on hue alone (hatching + words + position).

**8. Reports.** Compiled dossier previewed in a right-side pane (paper sheet on gray), with toolbar: Print, Export PDF, Copy link (local anchor). The dossier itself is typographically plain and print-perfect: metadata block, category tables with mono level codes, gap schedule, recommendations, evaluator statement.

**9. Mobile.** Split collapses: sticky compact context header (monogram, name, coverage bar) above a full-width skill-card stream; left pane becomes a slide-in sheet via a "Session" button; keyboard hints hidden, replaced by visible Prev/Next buttons; command palette becomes a search sheet. Rating segments stay 44 px tall.

**10. Interaction style.** Focus-visible: 2 px green ring, always visible on the active skill card's control. Hover: row/card tint `#F3F4F6`. Active: segment press tint. Loading: pane skeletons + top progress hairline. Success: autosave dot pulses once; session card animates between columns 160 ms. Error: red inline + session flagged in board column header. Empty: board column shows dashed drop-zone copy. Disabled: 40 % tint. All transitions ≤160 ms; reduced-motion respected.

**11. Why this direction fits the reference.** The reference's terminal triad (Save Progress / Generate Report / Clear Form) reveals the real lifecycle — draft, output, reset — but buries it at the bottom of a scroll. Studio promotes that lifecycle to first-class state (autosave, issue, discard) and keeps the reference's label-explicit honesty while giving evaluators the parallel context the single column denied them.

---

## OPTION E — "People Canvas"

**1. Design philosophy.** People before data. Canvas is for HR generalists and smaller organizations where the reviewer knows every employee by name: the team wall of human cards is the home, profiles read like introductions, and assessments are recorded as conversations (observations matter as much as numbers). Warm, humanist, serif-accented — deliberately un-corporate without being cute.

**2. Visual style.** Warm off-white canvas `#FAF7F2`; cards white with hairline `#E8E0D4` borders, radius 14 px, whisper-soft shadows; ink `#26221C`; secondary `#6B6257`; accent terracotta `#C2542E` for interactive, deep pine `#1F6F54` for strengths/success, amber `#9A6B15` for gaps. **Serif display** (system serif stack) for names and page headings; grotesque 15 px for UI/body. Medium density; 20–24 px padding; no gradients, no glass.

**3. Navigation.** Header: serif wordmark left; plain text links (People · Assessments · Skills Library · Reports); search field inline; avatar right. No sidebar, no breadcrumbs; back-links and the header carry wayfinding. Footer-free product chrome.

**4. Dashboard.** The **team wall**: filter bar (search, department select, status select, "gaps only" toggle) above a responsive grid of employee cards — monogram/photo tile, name in serif, role + department line, a slim competency summary bar (colored segments per category with labels on hover/focus), gap-count chip, status line ("Assessed 12 Sep" / "Assessment due"). A quiet summary strip above the wall: headcount, assessed this cycle, open gaps — as text figures, not tiles.

**5. Employee profile.** Canvas header: portrait, serif name, role/department/manager meta row, action row ("Start assessment" primary, "View latest report" secondary, "Edit profile" ghost). Body two columns: left = **skill shelves** — category tabs revealing skill badges (name + level tag pill); right = development column: gap notes ("Communication — Developing; role expects Intermediate"), recommendation list with target dates, and a simple history timeline (assessment events with evaluator names).

**6. Assessment interface.** A dedicated page structured as **category accordions** (one open at a time, completion tick per header). Inside, each skill row expands to a rating panel: level as a **pill radio group** where each pill shows the level word; selecting a pill reveals its plain-language meaning beneath; expected level shown as a ghosted pill with "role expects" label; observation textarea framed as "What did you observe?". Per-category progress in accordion headers; sticky footer bar: Save draft (secondary) · Continue (primary). Review step: all ratings as badge pairs + recommendations editor + submit.

**7. Skill gap visualization.** Badge-pair sentence: solid pill "Current: Intermediate" beside outlined pill "Role expects: Advanced", followed by a warm-gray sentence "One level below expectation — development suggested." On the wall and shelves, gap count chips carry numbers, never bare color.

**8. Reports.** Warm letterhead-lite: serif employee name as report title, metadata as a two-column definition list, category tables with warm rules, gap section as badge-pair sentences, recommendations checklist, evaluator's closing note in a tinted quote panel. Print CSS: single column, black ink, pills become bracketed words.

**9. Mobile.** Team wall → single-column list rows (monogram left, name/role, status right, gap chip); profile columns stack (shelves then development); accordions are naturally mobile; pill groups wrap to two rows with 44 px targets; sticky action bar remains. Search expands full-width in the header.

**10. Interaction style.** Hover: card lift 2 px + border darkens to `#D8CDBA`; link underline slides in. Focus-visible: 2 px terracotta ring. Active: press tint on pills. Loading: card skeletons in wall grid. Success: soft green check fades in beside accordion header; toast "Draft saved". Error: inline terracotta-red text + accordion header alert dot. Empty: wall shows a friendly one-line empty state with "Add your first employee". Transitions ~150 ms; reduced-motion honored.

**11. Why this direction fits the reference.** The reference's category cards pair names with human, comma-separated descriptions — an instinct to make taxonomies readable. Canvas applies that instinct to people and levels (badges with words, meanings on selection) and keeps the approachable accent-on-white confidence, swapping the reference's cool corporate blue for a warmer, people-first palette.

---

## OPTION F — "Clearline HR"

**1. Design philosophy.** Familiarity is a feature. Clearline deliberately speaks the conventional language of enterprise HR suites — utility bar, left nav, breadcrumbs, tabs, steppers, tables — so an HR professional migrating from spreadsheets or a legacy HRIS needs zero training. Originality is spent on clarity and correctness (one primary action, real states, accessible tables), not on novelty.

**2. Visual style.** White surfaces on `#F5F6F8` canvas; primary blue `#1F5ED8`; ink `#1F2430`; secondary `#5B6472`; borders `#D9DEE7`; radius 6 px; standard two-level shadows for menus/modals only; 14 px UI type, 12 px table meta; status colors (green/amber/red/blue) always with icon + text. Conventional density: 40 px rows, 16 px gutters.

**3. Navigation.** Top utility bar: org switcher, help, notifications, user menu. Left nav: Home · Employees · Assessments · Skill Matrix · Reports · Administration (collapsible groups, active indicator bar). Breadcrumb trail under the top bar on all nested pages.

**4. Dashboard.** Classic HR home: greeting + "Your tasks" list (pending assessments with due dates and deep links); KPI tile row (4 tiles: employees, completed, pending, open gaps) with small trend deltas; department competency table (department, headcount, avg level, gap count); recent activity table (employee, event, evaluator, date). Everything links through.

**5. Employee profile.** Header card (photo, name, ID, department, designation, manager, status badge) above **tabs**: Overview · Skills · Assessments · Reports · Development. Skills tab = grouped table with current/expected/gap columns; Development tab = recommendations table with owner and target date; Assessments tab = history table with statuses.

**6. Assessment interface.** Horizontal **stepper**: 1 Employee → 2 Categories → 3 Ratings → 4 Review → 5 Submit. Step 1: employee picker with search. Step 2: category checkboxes as conventional checkbox list with skill counts. Step 3: category sub-tabs; rating table per category: skill · expected (read-only) · current (select dropdown of five labeled levels) · improvement-required (auto checkbox, editable) · comment (inline expand). Validation summary panel lists missing ratings before step 4. Step 4: read-only summary tables + recommendation multi-select. Step 5: confirmation with evaluator attestation checkbox; "Save & exit" available at every step (draft persisted in localStorage).

**7. Skill gap visualization.** Dedicated Gap analysis view and report section: table Skill · Current · Expected · Gap (numeric) · Severity (chip + icon: On track / Monitor / Development required / Critical) · Suggested action. A compact bar-pair chart per department on the dashboard (labeled axes, values as text on hover/focus).

**8. Reports.** Standard report page with toolbar (Print · Export PDF · Share internally) and category filter; sections: employee details grid, category assessment tables, overall summary paragraph block, gap schedule, recommendations, additional comments. Print CSS produces a clean A4 document with page-break rules per section.

**9. Mobile.** Left nav → hamburger drawer with the same groups; utility bar condenses to menu + avatar; stepper → compact "Step 3 of 5" progress bar with label; tables → stacked cards (skill name bold, label/value rows beneath); tabs → horizontal scrollable tab strip; toolbar actions collapse into an overflow menu.

**10. Interaction style.** Conventional and predictable: hover tints `#F2F5FA`; focus-visible 2 px blue ring; active press; loading spinners in buttons + table skeletons; success toasts bottom-right ("Assessment submitted"); error banners top-of-form with field-level messages; empty tables show illustrated-free empty rows with guidance; disabled controls at 45 % with tooltip reason. Transitions 120–150 ms.

**11. Why this direction fits the reference.** The reference is itself corporate-conventional: labeled required fields, checkbox category selection, explicit section headings, a generate-report endpoint. Clearline preserves those conventions exactly where users expect them, and corrects the reference's weaknesses — action hierarchy, progress visibility, desktop layout, and state coverage — without asking anyone to learn a new paradigm.

---

# DESIGN COMPARISON (neutral)

| Criteria | A · Competency Ledger | B · Skillframe Matrix | C · Pathway | D · Assessment Studio | E · People Canvas | F · Clearline HR |
|---|---|---|---|---|---|---|
| Visual density | Medium-low, document-like | High, tabular | Medium, card-based | Medium-high, pane-based | Medium, warm cards | Medium, conventional |
| Dashboard complexity | Low (cover sheet + ledger) | Medium-high (heat table + queue) | Medium (wheel + rail + shelves) | Medium (session board) | Low-medium (team wall) | Medium (tasks + KPIs + tables) |
| Assessment workflow | Sequential clauses with TOC | Grid cell-rating, keyboard-fast | Guided wizard with progress rail | Split-screen session, one skill at a time | Accordions with pill ratings | 5-step stepper wizard |
| Data visibility | Per-employee depth first | Org-wide comparison first | Growth narrative first | Session pipeline first | People-first browsing | Task-and-table first |
| Mobile adaptation | Document → definition lists | Matrix → stacked rating cards | Wizard is mobile-native | Split → stream + sheet | Wall → list rows | Drawer + stacked cards |
| Enterprise feel | Audit/legal gravity | Analyst workspace | L&D-culture friendly | Ops-console efficiency | SMB / people-first | Classic HRIS familiarity |
| Reference influence | Strong (document soul, rule, panels) | Medium (utility honesty, categories as rows) | Medium (explanatory instinct, single flow) | Medium (lifecycle triad promoted) | Medium (readable taxonomies, accent-on-white) | Strong (conventions, required-field culture) |
| Implementation complexity | Medium (print CSS, TOC) | Medium-high (grid keyboarding, heat table) | Medium (wheel SVG, wizard state) | Medium-high (split panes, shortcuts, palette) | Low-medium | Medium (stepper, tabs, tables) |

No direction is objectively best: A and F reward trust and familiarity, B and D reward throughput and comparison, C and E reward adoption and culture. The right choice depends on who assesses, how often, and what the organization wants assessments to *feel* like.

---

# NEXT STEP

Per the agreed workflow, implementation is paused here. Choose one direction (A–F), or compose one (e.g. "B's matrix dashboard with C's assessment wizard" or "A's visual style with D's split-screen assessment"). On selection, Phase 5 (implementation blueprint: page architecture, design tokens, component system, responsive rules, data model, accessibility matrix, edge cases) will be produced for approval, followed by Phase 6 implementation in **HTML5 + CSS3 + vanilla JavaScript only**.
