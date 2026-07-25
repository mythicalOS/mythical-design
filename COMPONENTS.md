# Component registry — mythical design system

One row per component. **This is the dispatch surface**: to update a product's UI, point a
coding agent at a row (its ID + version), the spec anchor in `preview.html`, and the
implementation path — see "Dispatching a coding agent" below.

Spec version semantics: `v1` = the v0.5 book (`design.md`). A design change bumps the
version here + the `.ver` chip in `preview.src.html` (then regenerate). An implementation
is **current** when its "implements" column matches the spec version.

## Components

| ID | Spec | Status | Spec anchor (`preview.html`) | brokkr web UI (:7480, Preact) | implements |
|----|------|--------|------------------------------|------------------------------|------------|
| `app-shell` | v1 | shipped | `#c-app-shell` | `src/app.tsx` (top bar; 2-page shell predates the 5-page IA) | v0.4 — **5-page IA + BROKKR brand block pending** |
| `button` | v2 | shipped | `#c-button` | `src/components/Button.tsx` | v0.4 — re-verify against v0.5; v2 = secondary resting boundary → `--my-control-border` (decision #21) |
| `input` / `secret-slot` / `toggle` / `chip-dropdown` | v3 | shipped (chip-dropdown NEW) | `#c-input` | `src/components/Input.tsx`, `MaskedSecretInput.tsx` | v0.4 — chip-dropdown not yet built; **v2 = shape rule #20: editable chips + toggle squared to `--my-r-control` (read-only chips keep the pill tag look)**; v3 = `--my-control-border` resting boundary (decision #21) |
| `gauge` (arc + bar) | v1 | shipped | `#c-gauge` | `src/components/Gauge.tsx`, `src/render/gauges.ts` | v0.4 — add `--my-track` rail |
| `session-card` | v1 | shipped | `#c-session-card` | `src/components/SessionCard.tsx` | v0.4 — re-verify 5 states vs v0.5 |
| `terminal` / `queue-row` / `send-bar` | v2 | shipped | `#c-terminal` | `Terminal.tsx`, `QueueRow.tsx`, `src/render/term.ts` | v0.4 — terminal must use `--my-term-*` set; v2 = send-bar boundary → `--my-control-border` (decision #21) |
| `wizard` | v1 | shipped | `#c-wizard` | `src/components/WizardFrame.tsx`, `src/pages/wizard.tsx` | v0.4 |
| `confirm-dialog` | v1 | shipped | `#c-confirm-dialog` | `src/components/ConfirmDialog.tsx` | v0.4 |
| `toast` | v1 | shipped | `#c-toast` | `src/components/Toast.tsx` | v0.4 |
| `empty-state` | v1 | shipped | `#c-empty-state` | `src/components/EmptyState.tsx` | v0.4 |
| `save-bar` | v1 | shipped | *(settings layout — internal design archive)* | `src/components/SaveBar.tsx` | v0.4 |
| `family-tile` | v1 | shipped | `#c-org-family` | `src/components/FamilyPanels.tsx` | v0.4 — re-verify friendly-absent tone |
| `stat-tiles` | v1 | **NEW — unimplemented** | `#c-stat-tiles` | suggest `src/components/StatTiles.tsx` (sessions detail header) | — |
| `git-chip` | v1 | **NEW — unimplemented** | `#c-git-chip` | suggest `src/components/GitChip.tsx` (sessions detail header) | — |
| `tier-bar` / `memory-card` | v1 | **NEW — unimplemented** | `#c-memory` | Memory page does not exist yet (5-page IA) | — |
| `project-hero` | v2 | **NEW — unimplemented** | `#c-project-hero` | `src/pages/projects.tsx` (detail header) | — (v2 = shape rule #20: editable policy chips squared; read-only `backend` chip keeps the pill tag look) (editable chips inherit the v3 control-border) |
| `popover` | v1 | **NEW — unimplemented** | `#c-popover` | suggest `src/components/Popover.tsx` (chip-dropdown dependency) | — |
| `select` | v3 | shipped + adopted | `#c-select` | **`components/mythical-select.js` (this repo)** — form-associated `<mythical-select>` web component, native `<select>` attribute/API parity, groups, progressive mode (wraps a real select), `variant="chip"` covers interactive chip-dropdowns (squared per shape rule #20). v2: SVG caret, ✓ current mark, group separators, viewport flip-up, reduced-motion pop-in, no-`ElementInternals` native fallback; v3 = APG combobox a11y pattern + `--my-control-border` + hardened native-parity contract (per-option selectedness/dirtiness — the HTML model), 354-test cross-engine suite (decisions in CHANGELOG 0.5.4). Framework-agnostic: usable as-is in Preact/React (custom element) or recreated per stack. | v2-built sites (mythical-select 0.5.2 consumed directly via `import "mythical-design"`; sites: spawn-modal model/effort, roster editor, settings review-mode/source-type; zero native `<select>`s remain — scan-guarded). v3 verified in-consumer 2026-07-16 (typecheck clean, 1079/1079 tests, v3 confirmed in the bundle) — consumed automatically via the gitlink |
| `org-card` | v1 | **NEW — unimplemented** | `#c-org-family` | settings rail footer (`src/pages/settings.tsx`) | — |
| `timeline` | v1 | **NEW — unimplemented** | `ds/components-timeline.html` | not built in any product — the natural consumer is a scheduler's forward/backward view | — (one component for both directions: a forward schedule of what will run and a backward trace of what ran; hollow dot = not yet, filled = happened. The dot and the rail are positioned off the same two custom properties (`--tl-gutter`, `--tl-rail`) so the dot cannot drift off the line — any port must keep that coupling rather than hard-coding two offsets) |
| `file-explorer` / `markdown-preview` | v1 | **NEW — unimplemented as a shared component** | `ds/components-file-explorer.html` | built natively in brokkr (`ui/src/pages/files.tsx`) against an earlier copy of this card; not extracted to a package | — (two tree modes the card specifies distinctly: all-mounts, where the roots are the container's bind mounts with git repos one level down, and project mode, where the selected project's repos are the roots and a shared repo shows how many projects reference it) |

Other consumers: **the Command Center** is legacy — it adopts this system wholesale
in the React rebuild, not component-by-component; don't retrofit it. The **setup
wizard** and any future family-product UI (skuld/saga/edda) implement from the same rows.

## Dispatching a coding agent

Prompt skeleton for "replace/implement component X in product Y":

> Implement/replace component **`<id>` v`<n>`** in `<product>` per the mythical design
> system (repo `mythical-design` —
> prefer consuming the package over copying). Read, in order: `design.md` (rules),
> `tokens.css` (canonical tokens — import the package's `tokens.css`, never fork values),
> and the component's reference markup in `preview.src.html` at anchor `#c-<id>` (open `preview.html` in a
> browser to see it rendered, both themes). Reproduce **all** shown states (hover/focus/
> disabled/error/loading where applicable) using the product's established component
> patterns — this is a recreation in the target stack, not a copy-paste of the reference
> HTML. Verify against the rules-of-use block at the bottom of tokens.css (focus ring,
> status-with-icon, terminal `--my-term-*`, danger-fill scope). Run the package's
> typecheck + tests. Then update the component's row in `COMPONENTS.md`
> ("implements" column) — and nothing else in this repo.

Screen-level context (how components compose on a page): the handoff prototype in the
maintainers' internal design archive. When a dispatch reveals a spec gap, fix the spec first (template + regenerate),
then implement — never let a product fork the design.
