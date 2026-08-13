# mythical / brokkr — design book — v0.5 (canonical)

> **This is the living, canonical design reference for all mythical-family UI work.**
> It lives in its own repo — **`mythical-design`** — versioned and consumable as a package; this file + `tokens.css` +
> `assets/` are what you implement against. The records that fed it stay in the
> maintainers' internal design archive.
>
> Consumers: the brokkr local web UI (:7480), the setup wizard, the Command Center
> (React — visual language only), and the other family-product UIs (skuld,
> saga). Implement THIS book, not vibes.
>
> Component preview: **`preview.html`** — every component below on one page,
> self-contained (viewable anywhere; theme toggle + accent retint in a real
> browser). Generated from `preview.src.html` by `scripts/generate-preview.sh` — the
> git-hash stamp at its top says which token state it was built from.

## 1. Direction

**Fresh · light · modern · enterprise** — but not another blue-gray SaaS clone.

- **Light-first.** The product UI defaults to a warm paper-white workspace. Enterprise
  buyers read it as calm and trustworthy; it photographs well in decks.
- **Heritage as the dark theme.** The M8 mark language (near-black `#0B0C0F` tile,
  cream `#ECE7DE`, amber ring `#EFA443`) is the dark theme — named **"heritage"** in
  the theme toggle — and the terminal's native look in both themes. Continuity, not reset.
- **Petrol as signature.** Deep teal `#0F6B66`: calm precision, enterprise-native,
  semantically clean (no warn-collision), clearly apart from both the consumer-blue
  crowd and Anthropic's warm palette (a harness-agnostic product must not read as a
  Claude skin). Amber survives ONLY as the warn status color. Used sparingly: accents,
  nodes, focus states — never large fills.
- **The spine as motif.** The product's most distinctive mechanism (lineage/vertebrae)
  doubles as the visual motif: a continuous line with nodes — logo, gauges, lineage
  views, empty states.
- **Honest product voice** *(v0.5)*. Copy states the truth plainly and calmly:
  "Unconfigured — a valid state.", "The team is asleep.", telemetry shows its exact
  payload, the org page carries honesty panels ("local works forever", "data
  exportable"). Empty ≠ error; teases ≠ locks. Copywriting in the handoff prototype is
  deliberate product voice — keep it verbatim unless product says otherwise.

## 2. Brand — logo, wordmark, product label

**Logo (LOCKED, maintainer 2026-07-07): "M8"** — the spine-M drawn as one continuous stroke;
filled petrol nodes = distills; the right leg ends in a **stacked-hollow 8** (small ring
over large ring) — the whole 8 reads as the live lineage tip. Monogram codename **M8**
(mythical = 8 letters; the 8 doubles as ∞ — the lineage never ends).

- Canonical assets: `assets/logo-light.svg` · `assets/logo-dark.svg` (petrol/heritage).
- **Favicon rule:** below ~24 px the 8 goes blobby — use the plain single-node M
  reduction, never the 8-tip.
- **Wordmark:** `mythical` lowercase + terminal petrol node → `mythical●`. The wordmark
  never says "M8" — M8 is the monogram's codename, not the product name.
- **Product label** *(v0.5)*: the app brand block is logo + `mythical●` wordmark with a
  small all-caps **BROKKR** product label under it, right-aligned to the wordmark text.
  `mythical` is the family/OS mark; `brokkr` is this product (the developer container).
  Family products (skuld, saga, asgard) follow the same pattern: shared mark, own label.
- Exploration record: earlier previews and superseded logo variants are kept in the
  internal design archive — deliberately, historical only.

## 3. Color tokens (canonical source: `tokens.css` v0.5)

Light (default):
- `--my-bg: #FAF9F6` (warm paper) · `--my-surface: #FFFFFF` · `--my-border: #E7E3DB`
  · `--my-track: #EDEAE3` *(v0.5 — gauge/meter rails)*
- `--my-ink: #16181D` · `--my-muted: #5A5F6A`
- `--my-accent: #0F6B66` (petrol) · `--my-accent-strong: #0B5450` (text-safe on light)
  · `--my-accent-soft: #E0F0EE` (fills/hover)
- Status: ok `#1A7F4B` · warn `#B45309` (amber lives here and ONLY here) ·
  error `#B3261E` · info `#2B5FAA`
- Disabled: bg `#F1EFE9` · ink `#9AA0AC`

Dark ("heritage"):
- `--my-bg: #0B0C0F` · `--my-surface: #14161C` · `--my-border: #262B36`
  · `--my-track: #20242D`
- `--my-ink: #ECE7DE` (cream) · `--my-muted: #9AA0AC`
- `--my-accent: #3FB8AE` · `--my-accent-strong: #7FD1C9` · `--my-accent-soft: #10302E`
- Status: ok `#4CC38A` · warn `#EFA443` (heritage amber, demoted to warn) ·
  error `#E5484D` · info `#7AA7E8`

Terminal (BOTH themes — the terminal never goes light):
- bg `#0B0C0F` · text `#ECE7DE` · assistant `#3FB8AE` · user `#EFA443` · dim `#9AA0AC`
- *(v0.5.18)* add `#4CC38A` · del `#E5484D` — diff-semantic hues, the heritage ok/error
  values frozen for the always-dark pane; **not** new status colors (rule 2 stands)
- *(v0.5.19)* memory `#D79DF0` — the bookkeeping row's label hue. Minted, not mixed, and
  provably unmixable: an oklab `color-mix` is a convex combination, so every mix's `b` is
  at least the smallest `b` among its operands; the palette's minimum is dim's -0.0187 and
  this token is -0.0914, outside the hull. Min oklab ΔE 0.141 to any other terminal token;
  9.29:1 on the terminal bg

This list is the palette in full — the terminal set is **not** five colors. Adding a
terminal hue means adding a token here and in `tokens.css` together; `color-mix` reaches
only the convex hull of the tokens it mixes, so it can shade between them but cannot mint
a colour outside them. (Careful: "outside them" is about position, not hue — a mix CAN
land on an unoccupied *hue*, because near the neutral axis every hue is available; what it
cannot do is get there with usable chroma.)

Accent tweakability *(v0.5)*: the accent is user-tweakable from **curated swatches
only** (never a free color picker); `--my-accent-soft`/`-strong`/`-hover` derive from
the chosen accent via `color-mix(in oklab, …)` so the discipline survives retinting.
Theme switch: `data-theme="dark"` swaps the token set.

Contrast floor: all text pairs ≥ WCAG AA (4.5:1 body, 3:1 large). `--my-accent` never
carries body text on light; `--my-accent-strong` does. Known mild adjacency: ok-green
vs petrol — accepted (v0.2 decision); status colors always pair with icon/label, never
color-alone.

## 4. Typography

- **UI:** Inter (variable), fallback `Inter, -apple-system, "Segoe UI", Roboto,
  sans-serif`. Self-hosted (`assets/fonts/`, OFL licenses alongside) — the UI is
  CSP-strict/offline-capable, no CDN fonts ever.
- **Terminal / data / ids / numbers:** JetBrains Mono, fallback
  `ui-monospace, SFMono-Regular, Menlo`. `tabular-nums` on all counters/gauges/metrics.
- Scale (px @16 base): 12 caption · 13 mono-body · 14 **body (base)** · 16 body-lg ·
  20 h3 · 24 h2 · 32 h1 · 44 display. Weights: 400/500/**650** (650 = headings/bold;
  mono 650 resolves up to 700 per CSS font-matching — intentional, don't vendor SemiBold).
- *(v0.5)* Headings 20–26px at 650 carry letter-spacing **−0.3 to −0.4px**.
  **Micro-labels** (section rulers, chips, badges): 9.5–11px, 650, uppercase,
  letter-spacing **+0.4px**.
- `@font-face` block lives in `tokens.css` (relative `assets/fonts/…` URLs — keep
  tokens.css and assets/ siblings).

## 5. Shape, space, motion

- Radius *(v0.5 retune)*: **6** buttons/inputs · **7–10** cards/rows (10 = card
  default; nested rows may step 7–9) · **16** modals/hero · **99** pills —
  **non-interactive status tags only** *(v0.5.1, decision #20)*: anything the user
  can click is squared `--my-r-control`; a pill silhouette must never invite a
  click. The logo tile's big radius (~20%) is brand-only, not UI chrome.
- Spacing: 4-based scale (4/8/12/16/24/32/48/64).
- Borders over shadows on light (hairline `--my-border`); shadow only for
  overlays/modals: `0 16px 48px rgba(22,24,29,.16)` (dark: `rgba(0,0,0,.5)`).
- Interactive control **resting boundaries** use `--my-control-border` *(v0.5.4,
  decision #21)* — ≥3:1 non-text contrast (WCAG 1.4.11) against surface and bg in
  both themes; `--my-border` remains the divider/card hairline. Hover/focus/error
  border states are unchanged.
- Motion *(v0.5 retune)*: `--my-t-fast: 120ms ease` · `--my-t-base: 200ms ease`.
  Spine/lineage views may animate node pulses (≤1/s); `prefers-reduced-motion`
  respected.

## 6. App shell & information architecture *(v0.5 — supersedes the earlier 2-page shell)*

- **Top bar 56px**: brand block (§2) + page nav **Sessions · Projects · Memory ·
  Files · Settings**; right side: container status chip, theme toggle (light /
  "heritage" dark), overflow menu (re-run setup, give-back stats, sign out).
- **The window never scrolls.** Every page is `height: calc(100vh − 56px)`; each pane
  scrolls independently.
- **Rail + detail** is the master layout: left rail (Sessions 320px, Settings 260px,
  Projects/Memory alike) of compact cards; detail pane flexes (content max-widths
  ~1100–1160px where prose/heroes apply). Pin paired header heights (e.g. 76px/81px)
  so rail and detail borders align.
- Pages (screen-level truth = the handoff prototype, kept with its behavioral
  spec in the internal design archive):
  1. **Sessions** — the control room: session cards (avatar, status dot, context gauge
     bar), detail with arc gauge + stat tiles, git status, always-dark terminal with
     merged stop-turn control, per-agent delivery queue (ASAP/ON-DONE), send bar.
  2. **Projects** — active/inactive rail, gradient hero with inline rename + curated
     retint swatches, editable dropdown chips (review lane / authority rhythm / push
     flow), agents + repos grids, team default settings.
  3. **Memory** — three-tier framework (Project ◈ / Agents ◉ / Roles ⌘): tier filter +
     distribution bar, memory cards with recall-strength, detail with bijection line,
     audit trail, persona views. Fading is rank-only, reversible, nothing deleted.
  4. **Files** — tree explorer + markdown preview; two roots in all-projects mode
     (work dir + worktrees), per-project git repos otherwise.
  5. **Settings** — flat rail list (General · Models · Keys · Roles · Teams · Memory ·
     Git repos · Messaging · MCP & Skills · Family Products · Lifecycle-in-red) + the
     org card below a rule (the canonical upgrade page lives behind it).
- **Setup wizard**: 6 dependency-ordered steps (Paths → Project & repos → Model & key →
  Team & review → Telemetry → Review & **Activate**, single atomic write), durable and
  resumable. Auth screen: token entry with the 503 `ui_token_unset` banner.

## 7. Components & interaction rules

Canonical visuals: `preview.html` (every component, both themes, anchored per component)
plus the `ds/` cards (in this repo) and the handoff prototype (internal design archive) for
in-context behavior. **Each
component carries an ID + spec version in `COMPONENTS.md`** — the registry that maps
spec → implementation per product and defines how to dispatch an agent to update one.
v0.5 additions from the control-room design: `stat-tiles`, `git-chip`, `tier-bar` +
`memory-card`, `project-hero`, `popover`, `org-card`, `chip-dropdown`. Spec highlights:

- **Buttons**: primary = ink fill; accent reserved for focus/selection/highlights;
  danger is outline **everywhere except** the lifecycle confirm dialogs (the only
  danger fills in the product).
- **Gauges**: **arc = detail** views, **bar = compact** rows; same thresholds — fill
  flips accent → warn ≥75% → error ≥90% (whole-fill hue flip, never gradient;
  thresholds from contextmonitor, tweakable via `contextHighAt`/`contextCriticalAt`).
- **Session liveness**: `wakeLive:false` ⇒ dashed border, pulsing status dot,
  "reconnecting…" + disconnected banner in the terminal.
- **Terminal**: heritage-dark in both themes (§3 terminal palette), JetBrains Mono,
  blinking caret, tool/user/dim row styling, noise-filter label; stop-turn merged into
  the terminal chrome, appends a dim "— turn interrupted (^C) —" row.
- **Destructive confirms**: typed-name match (delete agent, remove repo, uninstall
  model, recreate container). Modals: 16px radius, scrim click + Esc dismiss.
- **Toasts**: bottom-center, tone-colored soft fill + icon + message, auto-dismiss 2.6s.
- **Dropdown popovers**: absolute, high z, `--my-shadow-modal`; ancestors must not
  clip (no `overflow:hidden` up the chain — round inner gradients instead).
- **Shape encodes interactability** *(v0.5.1, decision #20)*: interactive controls —
  buttons, inputs, selects, editable chip-dropdowns, toggles (rounded-rect track) —
  are squared `--my-r-control`; the pill radius is reserved for non-interactive
  status tags (ACTIVE, badges, kind chips). A read-only chip keeps the pill tag
  look and drops the caret — shape alone tells you what's clickable.
- **Focus** = `outline: 2px solid var(--my-accent); outline-offset: 2px` — identical
  everywhere, both themes, never removed without replacement.
- **Hover** = one token step (`--my-*-hover`), 120ms. **Disabled** =
  `--my-disabled-bg/-ink` + not-allowed cursor, never text opacity. **Loading** =
  currentColor spinner + label kept (stable width), `aria-busy`.
- **Error is for failures only** — empty/unset renders neutral ("unconfigured is a
  valid state"); error always pairs icon + message, never border/color alone.
- **Empty states**: muted hollow spine nodes + one petrol node = "where you are";
  unconfigured → "Run setup", asleep → "Good-morning". Never a disabled ghost form,
  never an error tone.

## 8. Decision log

The full taste-gate log (gates 1–11, all resolved) lives in the internal design archive —
the locked outcomes are folded into the sections above. v0.5 additions (#12–#19) were
maintainer-authored (2026-07-14 export) and carry no open gates; #20–#21
are maintainer-authored in-repo amendments (v0.5.1/v0.5.4, 2026-07-15):

| # | v0.5 decision | Section |
|---|---------------|---------|
| 12 | 5-page app IA + no-window-scroll shell (supersedes the 2-page shell, gate 9) | §6 |
| 13 | BROKKR product label under the wordmark; family-label pattern | §2 |
| 14 | `--my-track` token; motion retune 120/200ms ease; pill radius 99 | §3 §5 |
| 15 | Terminal palette fixed as tokens (both themes) | §3 |
| 16 | Curated-swatch accent tweak via `color-mix(in oklab, …)` derivation | §3 |
| 17 | Honest-voice copy rules (empty ≠ error, teases ≠ locks, verbatim copy) | §1 §7 |
| 18 | Component registry + per-component versioning (`COMPONENTS.md`); 7 new components specced from the control-room export | §7 |
| 19 | `mythical-select` — form-associated web component with native `<select>` parity (attributes, API, events, groups; progressive mode wraps a real select; `variant="chip"`); reference impl `mythical-select.js` | §7 |
| 20 | *(v0.5.1, maintainer 2026-07-15 — amends the pill-chip look in #19)* Shape encodes interactability: pill radius = non-interactive tags only; chip-dropdown / toggle / `variant="chip"` squared to `--my-r-control`. `mythical-select` v2: SVG caret, ✓ current mark, group separators, viewport flip-up, reduced-motion pop-in, no-`ElementInternals` native fallback | §5 §7 |
| 21 | *(v0.5.4, maintainer 2026-07-15)* Semantic `--my-control-border` token (≥3:1 non-text contrast both themes, WCAG 1.4.11) — interactive controls' resting boundary; `--my-border` demoted to dividers/cards; measured ratios recorded in tokens.css | §5 §7 |
