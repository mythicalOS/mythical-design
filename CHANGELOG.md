# Changelog — mythical-design

Package versioning tracks the design book: **minor = book version** (`0.5.x` ↔ book
v0.5), **patch = fixes/additions that don't change locked design decisions — plus
maintainer-authored amendments recorded as numbered rows in the `design.md` §8 decision log**
(the log, not the version number, is the lock ledger). Component spec versions live
per-row in `COMPONENTS.md` and are independent of the package version.

## Unreleased

Two new preview cards plus a select popup fix, synced down from the design pane.
No token or package change — `ds/` is outside the npm `files` whitelist, so
nothing here ships to consumers and no version is claimed.

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
