# Changelog — mythical-design

Package versioning tracks the design book: **minor = book version** (`0.5.x` ↔ book
v0.5), **patch = fixes/additions that don't change locked design decisions — plus
maintainer-authored amendments recorded as numbered rows in the `design.md` §8 decision log**
(the log, not the version number, is the lock ledger). Component spec versions live
per-row in `COMPONENTS.md` and are independent of the package version.

## 0.5.20 — 2026-08-13

- **No token values change.** Corrects the rationale comment on `--my-term-memory` and the matching
  §3 note in `design.md`, both shipped in 0.5.19, which stated the convex-hull argument in a form
  that is **self-referentially false**: "the palette's minimum `b` is `term-dim`'s -0.0187, so no
  mix of terminal tokens can produce this one". Once `--my-term-memory` *is* a terminal token the
  palette's minimum `b` is its own `-0.0914`, and a mix of terminal tokens reproduces it trivially.
  The claim is only true of the **other seven** tokens — the ones that existed before it — which is
  the form that carries the actual point: it had to be minted because nothing already in the
  palette could reach it. Also softens `dim 92.5% + del` from "hue 315.3°" to "about 315°": at
  exactly 92.5% the mix lands at 315.13°, and 315.34° needs 92.469%. Caught in cross-model review.

## 0.5.19 — 2026-08-13

- **`--my-term-memory`** (`#D79DF0`) — the label hue for the terminal's bookkeeping rows,
  declared in `:root` ONLY like the rest of the terminal palette (7 → 8 tokens), so it is
  theme-invariant by construction (rule 3). MINTED rather than derived, and provably so: an oklab
  `color-mix` is a **convex combination**, so every mix's `b` coordinate is at least the smallest
  `b` among its operands. Every **other** terminal token has `b ≥ -0.0187` (`term-dim` is the
  smallest); the new one sits at **b = -0.0914**. It lies outside the hull of the seven tokens that
  existed before it, so no mix of *those* could ever have produced it, at any ratio, nested or not
  — which is precisely why it had to be minted. *(Scope matters: the claim is about the other
  seven. The palette including this token trivially contains it.)* The occupied chromatic hues are
  `term-del` 23.0°, `term-user` 69.6°, `term-add` 159.3° and `term-assistant` 187.2°, while
  `term-ink`, `term-dim` and `term-bg` are effectively achromatic (chroma ≤ 0.019) and occupy no
  hue. *(Two weaker forms of this argument are **false** and are recorded so they are not repeated.
  "No mix can reach the empty 200°–360° hue region": `dim ≈92.5% + del` lands at about 315°, though
  only at chroma 0.016, and the most any two-token mix of the earlier palette reaches in that band
  is 0.048 against this token's 0.130. And "the band is empty, therefore unreachable": hue is a
  direction, and near the neutral axis every direction is available. The `b`-coordinate hull
  argument above is the one that actually holds.)* Measured at
  oklab L 0.780 / chroma 0.130 / hue 315.3°, its minimum oklab ΔE to any other terminal token is
  **0.141** (nearest `--my-term-dim`) and it clears AA on `--my-term-bg` at **9.29:1**. It
  replaces a `color-mix(in oklab, var(--my-term-assistant) 45%, var(--my-term-user))` that
  resolved to a muted olive-grey ≈`#B2B17D`, only **0.098** ΔE from `--my-term-dim` — which is
  the very colour the bookkeeping row's own body uses, so label and body barely separated.
  Named `term-memory` (not a new status colour) on purpose: it is a row-kind hue scoped to
  terminal surfaces, so rule 2 stands.

## 0.5.18 — 2026-07-29

- **`--my-term-add` / `--my-term-del`** — terminal diff hues (added/removed lines), declared in
  `:root` ONLY like the rest of the terminal palette (5 → 7 tokens), so they are theme-invariant
  by construction (rule 3). The values are precisely the heritage (dark-theme) `--my-ok`/
  `--my-error` — what a diff fence renders on `--my-term-bg` today in both app themes, so the
  mint changes zero pixels; measured on the term bg they clear AA for small mono text
  (8.83:1 / 5.00:1), where the light-theme pair would fail (3.75:1 / 2.99:1) — which is why the
  terminal's own comment block refuses to re-point ok/error and a green/red must be MINTED into
  the term palette, not borrowed from the theme-flipping status set. Named `term-add`/`term-del`
  (not `term-ok`/`term-error`) on purpose: these are DIFF-SEMANTIC hues scoped to terminal
  surfaces, not new status colors (rule 2 stands).

## 0.5.17 — 2026-07-29

- **`--my-tone-ink`** — label ink on status-tone fills, for the new tone-filled button
  (`.btn--tone` + `data-tone`, ships in `@mythicalos/ui-core`; spec originated pane-side as
  `SPEC-btn-tone.md` during the mockup rewire). `#FFFFFF` in light (the AA-nudged status hues
  are deep enough for white); heritage flips it to `var(--my-bg)` because the heritage status
  hues are light. `data-tone="error"` remains governed by rule 9 — danger fills only in the
  lifecycle confirm flows.
- **`--my-hairline`** — the sub-border separator rule, moved upstream from the product
  mockups: SAGA defined `color-mix(in oklab, var(--my-border) 72%, var(--my-surface))`
  locally; SKULD and BROKKR consumed `var(--my-hairline)` **undefined** (28 + 3 uses whose
  borders fell back to `currentColor`) — upstreaming fixes both via the next design-system
  binding refresh, no page edits. Derived from border+surface, so one `:root` definition
  adapts per theme (and stays out of the theme-flipping token set by construction).

## 0.5.16 — 2026-07-28

- **`--my-ok` / `--my-warn` (light) darkened ~2%** — #1A7F4B→#197C4A, #B45309→#B05109 — so
  their soft pairs clear AA 4.5:1 for small text (4.35→4.52, 4.39→4.55); hue unchanged,
  visually near-invisible. Maintainer ruling 2026-07-28, decided on measured evidence.
- **Rule 10 gains its first ruled exception** (maintainer, 2026-07-28): the theme-toggle
  family keeps its card's pill — segmented track/options/knob and switch track; the inputs
  Toggle stays squared. Scoped, not a precedent. Same ruling round confirmed the chip count
  stays **undimmed** (any opacity puts the small mono digits under AA on every tone).
- **`--my-shadow-knob`** — the raised-knob shadow (theme toggle's segmented knob and switch
  knob), light + heritage values: a lift of value, never a glow.
- **`ds/components-theme-toggle.html`** — new card (maintainer-authored in the Design pane):
  one component, three members — segmented (System first-class), icon (sun↔moon cross-fade,
  skips System), switch (settings rows only). 28 cards. Revised same day: the labelled
  segment raises the checked button itself (no measured knob), knob motion is left-based,
  and the icon caption matches the CSS — it shows the theme you're in. Ported to
  `@mythicalos/{ui-core,preact-ui,react-ui}` (unpublished 0.5.0/0.6.0/0.4.0).

- **The tags family folds back into Chip** (maintainer redesign, same day it shipped):
  `ds/components-tags.html` removed; `ds/components-chip.html` is now the family card —
  `.my-chip` (pill, neutral-by-default on `--my-surface-hover`, 8 tones, xs/md, dot/count/
  removable ×), `.my-chip-flag` (squared mono machine facts), `.my-chip-dd` (the interactive
  member). Registry row updated; the Tag-vs-Chip decision is closed.
- `ds/foundations-shape.html`: rule-10 wording says "chips", matching the retired Tag concept.

## 0.5.15 — 2026-07-28

- **`--my-fs-nano: 10px`** — a new smallest type-scale point at the low end of the
  micro-label band (§4's 9.5–11px). Minted so the tags family's `xs` size and the
  flag's mono face resolve through a token instead of snapping up to `--my-fs-micro`.
- `ds/components-tags.html`: the count (`.num`) is no longer opacity-dimmed — the
  `ok`/`warn` soft pairs sit at ≈4.4:1 undimmed, so any dimming pushes small mono
  digits below AA; hierarchy stays with the mono face. The removal `×` rests at
  `.8` (was `.6`) so the control clears 3:1 non-text contrast on every tone.

## 0.5.14 — 2026-07-28

A docs-and-cards release. **No token change — `tokens.css` is byte-identical to
0.5.13.** The patch version is claimed because `design.md` and `COMPONENTS.md`
both ship in the package and both changed; the `ds/` card work rides along but is
outside the npm `files` whitelist and reaches no consumer.

The registry now says which components are PACKAGED and which live in exactly one
product — it had drifted in both directions, which made it undispatchable.

- Seven components graduated into `@mythicalos/*` and their rows now name the
  package instead of a path inside one product: `popover` (specified here but
  never built anywhere), `save-bar`, `stat-tiles`, `git-chip`, `session-card`,
  the `terminal`/`queue-row`/`send-bar` set, and `file-explorer`. Also repointed:
  `app-shell`, `button`, `input`, `gauge`, `confirm-dialog`, `toast`,
  `empty-state`, `select`.
- Five rows had claimed **NEW — unimplemented** while the component was in fact
  already built — one even said the Memory page "does not exist yet". Those now
  read *built in-product, NOT packaged*, which is the honest state and marks them
  as extraction candidates. `timeline` is the only row still genuinely
  unimplemented anywhere.
- Column 4 pointed at anchors in a `preview.html` this repo no longer ships, so
  every `#c-*` link was dangling. Rows now cite their card under `ds/`, and the
  dispatch prose follows.

Two new preview cards, a select popup fix, and the family narrowing to four
products.

- **The family is four products** — brokkr, skuld, saga, asgard. The fifth is
  retired and is not to be reintroduced in this book, its cards, or any consumer.
  - `ds/product-glyphs.html` — the retired product's row is dropped from all
    three bands (light art, heritage dark, 22px optical) and from the
    construction caption, which loses the concentric ring+dot exception with it.
    Four glyphs per band; asgard remains the one deliberately ring-less mark.
  - `design.md`, `COMPONENTS.md` — the consumer lists name the surviving
    family UIs.
  - `ds/foundations-shape.html` — the retired name was also a sample chip label
    in the shape card; swapped for a live product.

- `ds/components-select.html` — the popup is pinned to both edges and sized
  `border-box`. It was `min-width:100%` under content-box, so the popup's own
  6px padding and 1px border were added outside that 100% and it rendered 14px
  wider than the control it belongs to. The implementation carries the same fix
  (`@mythicalos/ui-core` 0.2.2) — spec row `select` stays v3, since the geometry
  is a defect fix and not a spec change.

- `ds/components-timeline.html` (Components) — one component for anything on a
  clock: a forward schedule and a backward trace, hollow dot = not yet, filled =
  happened. The dot derives from the same two custom properties as the rail
  (`--tl-gutter`, `--tl-rail`), so it cannot drift off the line.
- `ds/components-file-explorer.html` (Components) — the file-tree rail and
  markdown preview, specifying two distinct tree modes (all-mounts, where the
  roots are the container's bind mounts; project mode, where the selected
  project's repos are the roots). Previously only in the product design project
  and never in this repo, though an earlier copy of it was implemented natively
  in brokkr's Files page.
- Both registered in `COMPONENTS.md` as unimplemented, anchored to the card path
  rather than `preview.html` — that file no longer exists in this repo, so every
  older `#c-*` anchor in the registry is a dangling reference awaiting a sweep.
- `tokens.css` deliberately NOT synced: the pane's copy differs only by
  `/* @kind … */` annotations its token classifier injects on the five
  font-weight/motion tokens. Every value is identical, so the repo's copy stays
  the source of record rather than importing editor metadata.

## 0.5.13 — 2026-07-24

Preview cards live in-repo again; book wording made release-neutral.

- `ds/` restored: the full 24-card preview set (brand, foundations, components,
  layouts — select v3), including the new `ds/product-glyphs.html`: the five
  product glyphs (brokkr/skuld/saga/edda/asgard) in light, heritage-dark and
  22px-tile tunings, with the construction rules in-card.
- asgard glyph recentered — geometry shifted up 21 units so the arch sits on the
  optical center like the other four glyphs (all shipping surfaces updated).
- `design.md`, `COMPONENTS.md`, changelog and the `tokens.css` header now describe
  history and provenance neutrally (internal design archive); no token values,
  component specs or locked decisions changed.

## 0.5.12 — 2026-07-24

Provenance release workflow (npm Trusted Publishing / OIDC); no design changes.

## 0.5.11 — 2026-07-23

New token: `--my-fs-micro: 11px`.

- The book (§4) has always spec'd micro-labels (section rulers, chips, badges)
  at 9.5–11px / 650 / uppercase / +0.4px letter-spacing, but the scale had no
  token for it — so tokens-only consumers were forced to snap micro-labels up
  to `--my-fs-caption` (12px), off the band's own ceiling (surfaced building
  `@mythicalos/ui-core` styles: `.my-chip` 10→12px, `.my-card__title` 11→12px).
  One scale point at the band ceiling closes the gap; no locked decision changes.

## 0.5.10 — 2026-07-22

Component sources move to `components/`.

- `mythical-select.js` + `mythical-select.d.ts` now live in `components/`
  (future registry components land there too). The package SUBPATHS are
  unchanged — `mythical-design` and `mythical-design/mythical-select` resolve
  through the exports map, so consumers need no changes (verified in-consumer:
  typecheck, tests, bundle). Updated: entry import, exports/files targets,
  test fixtures, the template's script src (preview generator now accepts
  subdir paths), the card's `@dsInline` marker (now the honest repo-relative
  path — the checker resolves marker names as paths), and docs. The design.md
  §8 ledger keeps its original wording as history.

## 0.5.9 — 2026-07-22

Build tooling moved to `scripts/`; tree cleanup.

- `generate-preview.sh`, `check-ds.sh` and `subset-ds-fonts.sh` now live in
  `scripts/` (root anchoring via `cd "$(dirname "$0")/.."`). Every reference
  updated: package.json script aliases, README/AGENTS/design.md/COMPONENTS
  docs, the preview provenance stamp, the checker's violation hints, and the
  tool-owned `@dsFonts` marker text — the latter rewrote all 23 cards via
  `--fix` (font payloads unchanged; marker text only). Subsetter verified by a
  full regeneration run from its new location (outputs byte-identical,
  manifest note text updated). Historical CHANGELOG entries keep the old
  paths, as history.
- Earlier same-day cleanup: the stale tracked preview archive removed
  (archives now gitignored — local working aids, recoverable from git
  history) and the source-review ledger retired from the tree (outcomes live
  here; full text in history).

## 0.5.8 — 2026-07-22

Review r6 finding (1 MED) — regression introduced by the 0.5.7 unification.

- The 0.5.7 `css_rules` replacement in `subset-ds-fonts.sh` was applied with a
  block-regex that also swallowed the six initializations sitting between the
  function and the next `def` (`TOKENS` + `FAM_UI`/`FAM_MONO`/`SIZE_TOK`/
  `IMPORTANT_RE`/`VAR_SHAPE`) — font REGENERATION aborted with a NameError
  while everything routinely checked (cards, checker, component suite) stayed
  green, because nothing in `npm run check` executes the subsetter. Restored
  verbatim from f8f7cd7 and verified the honest way: a full regeneration run —
  all four subset files byte-identical, manifest content-identical (date stamp
  only), checker clean ×23. Lesson encoded: any edit to subset-ds-fonts.sh
  must be followed by an actual regeneration run (venv one-liner in its
  header); the checker cannot cover a tool that needs fontTools.

## 0.5.7 — 2026-07-22

Source-review r5 finding (1 MED) — the last bespoke CSS parser unified onto the
shared lexer.

- `css_rules` (the token-DEFINITION scan's structure parser, mirrored in the
  subsetter) tracked quotes with its own non-escape-aware state: valid
  `content:"foo\"bar"` corrupted it and hid a later drifted `--my-*`
  definition from the mismatch check (Chromium-confirmed valid-and-applied
  CSS, checker silent). It now parses structure over the shared escape-aware
  `css_tokens` lexer — string tokens are opaque, so braces/semicolons/colons/
  escaped quotes inside strings can never corrupt rule structure. Exact-repro
  negative fires the definition mismatch; reference-scan and fonts probes
  unchanged; cards byte-identical.

## 0.5.6 — 2026-07-22

Checker hardening — three findings from the human source-review round against
`check-ds.sh` (r4), folded without changing any generated card content. No
design decisions changed; cards are byte-identical.

- **Token REFERENCES validated** (source review, MED): the checker validated
  `--my-*` *definitions* in cards against `tokens.css` but never the
  `var(--my-…)` *references* — a card could use `var(--my-doesnt-exist)` and
  pass clean. Every reference in the card's CSS (`<style>` blocks + `style=""`
  attributes) must now resolve against canonical `tokens.css` (light ∪ dark)
  or the card's own local definitions; a `var()` fallback does not excuse a
  dangling reference (`var(--my-x, 10px)` with no `--my-x` anywhere IS the
  token-renamed-canonically drift case). Non-`--my-*` custom properties stay
  out of scope (private card vars), as does script-injected CSS (documented
  static-reach boundary).
- **Relative resources are containment violations** (source review, LOW):
  containment blocked absolute/protocol-relative URLs but permitted relative
  file references — `<img src="missing.png">` passed although a
  self-contained card can never load it. Resource-bearing attributes
  (`src`/`href`/`xlink:href`/`poster`/`background`/`cite`/`action`/
  `formaction`/`longdesc`, `<object data>`, `srcset`/`imagesrcset`
  candidates) now permit only data: URIs, `#fragment` references and the
  empty string — calibrated against the corpus (the 23 cards carry no
  resource-bearing attribute values at all, so no `<a href>` carve-out was
  needed). srcset candidate parsing became WHATWG-approximate so data: URIs
  containing commas survive intact.
- **Comments are not rendered text** (source review, LOW): codepoint
  collection included `<script>`/`<style>` source verbatim, so a JS comment
  containing 漢 forced font regeneration though it never renders. JS comments
  (`/* */` and `//`, outside string/template literals — small documented
  state machine incl. the regex-literal/division approximation) and CSS
  `/* */` comments are now stripped before collection; string/template
  literal text stays collected (the select script injects real glyphs from
  strings). Mirrored byte-for-byte in `subset-ds-fonts.sh` — both collectors
  verified to derive the identical set on all 23 cards. The collected
  used-set shrinks 134 → 132 codepoints (comments contributed only U+00A0
  and U+00B1, both inside the stability floor): the manifest's 333-codepoint
  superset contract is untouched, no subsets regenerated.
- **Gate round on the above (4 MED, all in the new lexers, all in the
  losing-rendered-content direction)**: CSS comment stripping is now an
  escape-aware lexer that keeps comment-shaped text INSIDE strings
  (`content:"/* 漢 */"` renders); the JS stripper tracks template nesting
  with a context stack (`` `a ${`b // c`} d` `` no longer truncates) and an
  expression-ended state for `/` disambiguation (`"6" / 2` divides — a
  following `"file:///…"` string is untouched); the `var()` reference scan
  shares the escape-aware CSS lexer, so an escaped quote can neither mask a
  real dangling reference nor let string-literal `var()` text false-positive
  (CSS performs no substitution inside strings). Corpus impact: none — the
  23 cards collect the identical 132-codepoint set before and after.
- **Final gate round (2 MED, lexer states; gate CAPPED)**: CSS escapes now
  consume in CODE context too (`foo\"bar` no longer opens a string that hides
  a real dangling `var()` or mis-strips later rendered text), and postfix
  `++`/`--` sets the expression-ended state (`` `${i++ / 2} file:///漢` ``
  keeps its template text). Residual space is full-ECMAScript/CSS lexing,
  bounded by the documented collect-more bias; corpus impact again none
  (identical 132-codepoint set).

## 0.5.5 — 2026-07-21

The `ds/` Design-pane cards get an executable drift control (the source review's
"source-card synchronization is manual" + "cards do not contain the canonical
fonts" findings). No design decisions changed.

- **`check-ds.sh`** (`npm run check:ds`; python3 stdlib only, exits non-zero
  listing every violation): validates the first-line `@dsCard` marker,
  self-containment (no http(s) `src`/`href`, no `<link rel=stylesheet>`, no
  `<script src>`), `--my-*` token identity against the corresponding
  `tokens.css` theme block, byte-exact `@dsInline` embedded sources, and
  `@dsFonts` payload hashes + codepoint coverage against
  `assets/fonts/ds-subset/manifest.json`. `--fix` performs the mechanical
  repairs only (re-embed inline sources, refresh font blocks) — token and
  containment problems stay human decisions.
- **Tool-owned select embed**: `ds/components-select.html`'s inlined component
  now sits between `@dsInline mythical-select.js` markers and is re-embedded
  verbatim by `--fix`. This retires the drift the review flagged — the card was
  carrying a stale 70,178-char pre-v3 copy (host-wide dirty model) of the
  79,986-char canonical source; it now hash-verifies byte-identical.
- **Embedded font subsets**: every card carries a `@dsFonts` block (inside
  `<head>` — a pre-doctype block would force quirks mode) with data:-URI woff2
  subsets of the canonical fonts, so self-contained cards render canonical
  typography instead of host fallbacks. `subset-ds-fonts.sh` (fontTools;
  regeneration-only — the repo needs nothing beyond stdlib at check time)
  subsets to the cards' used characters + a stability floor (333 codepoints):
  Inter variable **99.3 KB** (weight axis kept, no instancing needed),
  JetBrains Mono 400/500/700 **34.2/34.9/35.4 KB** — **203.8 KB** total,
  ~279 KB base64 per card. Italic faces are not embedded (no card uses them);
  glyphs the canonical fonts themselves lack (e.g. emoji) keep falling through
  to system fonts, recorded per file in `manifest.json`.
- **Check wire-up**: `npm run check` is now
  `tsc --noEmit mythical-select.d.ts && ./check-ds.sh`; the four cards whose
  `--my-font-mono` copies had drifted to comma-tight spacing were re-aligned to
  the canonical value (git-chip, memory, project-hero, stat-tiles — functionally
  identical, now byte-comparable).
- **Cross-model review hardening**: `check-ds.sh` gains an `@dsInline`
  allowlist (`--fix` refuses non-allowlisted sources), whole-`@dsFonts`-block
  regenerate-and-byte-compare validation (in-`<head>` position included),
  structural full-line marker location (marker-shaped text inside `<script>`
  is inert), staged `--fix` (repair in memory, re-validate, write only when
  clean), byte-not-text identity (CRLF/LF divergence violates), wider
  containment (protocol-relative URLs, `srcset`, `xlink:href`, CSS
  `url()`/`@import`, `<object data>`, `srcdoc`, meta refresh), manifest
  authority over `ds-subset/` (source-font sha256s, no stale files),
  CSS-derived weight/style coverage against the manifest faces (variable
  Inter axis range, JetBrains Mono 650→700 resolution, italics fail),
  group↔filename-prefix validation, and rendered-attrs-only codepoint
  collection; declined items (escape-obfuscated tokens, token-stream
  comparator) are recorded as a THREAT MODEL note in the script header.
- **Round-2 hardening** (cross-model review): marker CLOSERS are now located
  outside `<script>` spans like openers (an opener without a legitimate closer
  is a violation `--fix` refuses), the `@dsInline` allowlist became a
  requirement (exactly one block — missing/duplicate violates), CSS-correct
  weight parsing (1–1000 incl. fractional, shorthand-reset = 400, `!important`
  stripped, card-local `var()` resolution with unresolvable-typography
  violations), UA-bold `<h1>`–`<h6>`/`<th>` coverage, subsetter
  reject-don't-clamp with decimal-safe instance names, and `--fix` card writes
  gated on global manifest/source integrity.

## 0.5.4 — 2026-07-15

Decision #21 (maintainer) + `mythical-select` v3. Subsumes the 0.5.2 constructable-
stylesheet delivery (v3 rebuilds all rendering on it, plus a DOM-API skeleton
with zero innerHTML sinks).

- **Decision #21 — `--my-control-border`**: semantic token for interactive
  controls' *resting boundary*, ≥3:1 non-text contrast (WCAG 1.4.11) in both
  themes with measured ratios recorded in `tokens.css`; `--my-border` is demoted
  to dividers, cards and non-interactive tags. The preview toggle gains the
  boundary (off-state track was borderless); disabled stays boundary-free.
- **`mythical-select` v3** — APG select-only combobox hardening: labeled option
  groups render as `role=group` containers labelled by their headings; external
  *and wrapping* `<label>` text is mirrored onto the trigger (host subtree
  excluded, refreshed on focus); Tab commits the active option before moving
  focus on; closed Home/End open at the first/last enabled option. Wrapped
  native `<select>`s keep the one-way host→native sync contract, and a native
  moved back out of the host gets its presentation restored (never its `name` —
  the host owns form identity). Live `options` snapshots (current selectedness),
  duplicate-value-safe re-adoption (selected source-element identity wins),
  re-adoption while open closes cleanly (no dangling `aria-activedescendant`). Rendering stays
  CSP/Trusted-Types-safe (no HTML-string sinks; constructable stylesheets —
  note: Firefox 93–100 has `ElementInternals` but not `adoptedStyleSheets` and
  falls back to an inline `<style>`, which a strict `style-src` without
  `'unsafe-inline'` blocks — functional but unstyled there), collision-safe
  registration, WebKit `<label for>`-focus fix.
- **Typed surface**: hand-written `mythical-select.d.ts` (ambient interfaces
  only — no phantom global constructor values; constructor types reachable via
  `customElements.get()`), shipped in `files` and wired as `types` conditions
  on the `.` and `./mythical-select` exports.
- **Per-OPTION selection state (the HTML model)**: selection is tracked exactly
  like native — every option carries a `selectedness` and a `dirtiness`, the
  `selected` *content attribute* writes selectedness only while that option's
  dirtiness is false, a user commit / `value` / `selectedIndex` write dirties
  **only the option it selects**, and structural mutations run the spec's
  ask-for-reset. This replaces a select-wide dirty flag plus an adoption-strength
  enum (`force`/`defaults`/`structural`/`metadata`) that reconstructed selection
  from a fresh `[selected]` scan, and fixes three divergences: a later
  `[selected]` default no longer loses to an unrelated earlier write; a cleared
  selection (`selectedIndex = -1`) no longer resurrects a stale `[selected]`
  option when the list changes; and a MOVE now replays as removal-then-insertion
  so the removal's ask-for-reset lands first. Engine deviations, both followed to
  the 2-engine majority and pinned by differential tests: Firefox ignores a
  `[selected]` add on **index 0** while another option is selected (it honors it
  at every other index); WebKit re-runs a reset on an option MOVE, losing the
  selectedness the spec keeps on the option node.
- **3-engine contract suite**: 118 Playwright tests per engine × 3 engines = 354
  (chromium + firefox + webkit) — native-parity contract, ARIA, strict-CSP and
  no-`ElementInternals` fallback. Mutation/selection tests are
  *native-differential*: each builds a real `<select>` of the same shape, runs
  the same operations and asserts the host matches it on that engine.
- **Preview pipeline**: archives follow `preview.<YYYYMMDD>-<shortsha>[-N].html`
  — the `-N` suffix only on a name collision with different content (`-2`,
  `-3`, …; identical archives are reused); the exact woff2 files tokens.css
  references join the dirty-status check for the provenance stamp (fonts are
  inlined inputs — a touched license or stray file in `assets/fonts` stays
  clean) without being listed as stamp sources.

- **Known limits** (cross-model gate capped at 14 rounds; 83 findings processed,
  80 folded, 3 declined-with-reason + these residuals): a MutationObserver sees
  mutations asynchronously and in batches, so a single task's intermediate DOM
  states are reconstructed, not observed. Ordered child-list replay covers
  insert/remove/move (including a move's removal-reset preceding reinsertion),
  but four contradictory same-task batches resolve to the final DOM where native
  resolves step-by-step: a pristine option's `[selected]` edited while detached
  then reinserted; the same option's `[selected]` added and removed in one task;
  an option removed while another is disabled in one task at `selectedIndex -1`;
  and multi-move (or insert+remove) batches. Each needs mutually-cancelling edits
  within one task. The ordinary reactive-framework shape — moving `[selected]`
  from one option to another on re-render, which is how the first consumer authors it —
  matches native on chromium, firefox and webkit (probed).

## 0.5.3 — 2026-07-15

`ds/` preview-card set — the claude.ai Design pane sync source.

- One self-contained HTML card per component/foundation/layout (first-line
  `<!-- @dsCard group="…" -->` marker), mirroring the `preview.src.html` sections.
  Seeded from the original card set (internal design archive, now history),
  brought to v0.5: inputs card → spec v2 (chip-dropdown + shape rule #20), gauge
  rail → `--my-track`, colors card gains track + terminal palette; NEW cards for
  select v2 (component JS inlined, progressive-mode-first so sandboxed panes
  render a working native select), popover, stat-tiles, git-chip, tier-bar/
  memory-card, project-hero, org-card/family-tile, empty-states, foundations-
  shape, and the app-shell layout. Edit `preview.src.html` first, then keep the
  affected card in step — cards are derived, the template is the source.

## 0.5.2 — 2026-07-15

`mythical-select` CSS delivery is CSP-safe (first-consumer dispatch finding).

- Shadow styles now ship via a shared constructable stylesheet (`adoptedStyleSheets`)
  instead of an injected `<style>` tag: a consumer CSP of `style-src 'self'` (which the
  first consumer serves exactly) blocks inline style elements even inside shadow roots, while
  constructed sheets are exempt by spec. The `<style>`-tag path survives only as the
  fallback for engines without constructable sheets. No visual or API change; spec stays v2.

## 0.5.1 — 2026-07-15

Decision #20 (maintainer — amends the pill-chip look in #19) + `mythical-select` v2.

- **Shape encodes interactability** (tokens.css rule 10; `design.md` §5/§7): pill
  radius (`--my-r-pill`) is reserved for non-interactive status tags. Every
  interactive control is squared (`--my-r-control`) — editable chip-dropdowns,
  toggles (rounded-rect track + squared thumb) and the select chip variant lose
  the pill look; read-only chips keep it (a tag, not a control). Component rows
  `select` and `input/…/chip-dropdown` bumped to v2.
- **`mythical-select` v2** — richer, size-aligned with `.btn`/`.input` (34px /
  r-control): SVG chevron caret (platform-stable, rotates on open), ✓ mark on the
  current option (popover parity), hairline group separators, pop-in motion
  (`prefers-reduced-motion` aware), viewport-aware flip-up, thin styled
  scrollbars, `overscroll-behavior: contain`, iOS tap-highlight/`touch-action`
  hygiene.
- **Cross-browser**: engines that parse the file but lack `ElementInternals` now
  degrade to a *working* native `<select>` — progressive instances keep the one
  they wrap, pure-tag instances get one synthesized (name/required/disabled,
  groups incl. `optgroup[disabled]`, `[selected]`, matching-`value` only;
  non-option children preserved; FOUC-guarded) — submitted forms keep working;
  the JS API and post-connect option mutation are not emulated. Engines too old
  to parse modern class syntax (Safari <15 / Chrome <84 / FF <90) get no upgrade
  at all — use progressive mode where those matter. `:user-invalid` gains a
  `[data-user-invalid]` attribute fallback driven by commit, `reportValidity()`
  and the native blocked-submit `invalid` event (kept as separate CSS rules so
  an unknown pseudo-class can't invalidate the shared rule).

## 0.5.0 — 2026-07-15

Initial extraction from the maintainers' internal design archive into its own repo.

- Design book v0.5 (`design.md`) — v0.4 merged with the
  control-room iteration; decisions #1–#19.
- Canonical tokens v0.5 (`tokens.css`), brand assets (M8 logo SVGs + `.graffle`
  sources, self-hosted Inter/JetBrains Mono, OFL licenses).
- Component registry (`COMPONENTS.md`) — 19 component rows with spec anchors,
  per-product implementation pointers, and the agent-dispatch prompt skeleton.
- `mythical-select` v1 — form-associated web component with native `<select>` parity.
- Preview pipeline: `preview.src.html` (template) → `generate-preview.sh` →
  self-contained `preview.html` (tokens + fonts + component JS inlined, git-hash
  provenance stamp, dated archives).
