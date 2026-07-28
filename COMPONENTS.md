# Component registry — mythical design system

One row per component. **This is the dispatch surface**: to update a product's UI, point a
coding agent at a row (its ID + version), its card under `ds/`, and the implementation — see "Dispatching a coding agent" below.

Spec version semantics: `v1` = the v0.5 book (`design.md`). A design change bumps the
version here and the component's card under `ds/`. An implementation is **current** when its
"implements" column matches the spec version.

**Packaged vs product-local.** A row reading *PACKAGED* ships from `@mythicalos/*` and every
product consumes the same code. A row reading *built in-product* exists in exactly one product
and is a candidate for extraction — the distinction is the point of this table, because a
second product wanting that component is the moment the duplication starts costing.

## Components

| ID | Spec | Status | Spec anchor (card in `ds/`) | Implementation | implements |
|----|------|--------|------------------------------|------------------------------|------------|
| `app-shell` | v1 | shipped | `ds/layouts-app-shell.html` | **@mythicalos/shell** — `TopBar`, `NavTabs`, `ProductSwitcher`, `WorkspaceSplit`, `Rail*`, `SettingsLayout`/`SettingsNav` | shell 0.4.0 |
| `button` | v2 | shipped | `ds/components-buttons.html` | **@mythicalos/preact-ui** `Button` · **@mythicalos/react-ui** `Button` (classes from **@mythicalos/ui-core** `buttonClass`) | preact-ui 0.4.0 — v2 secondary boundary is `--my-control-border` (decision #21) |
| `input` / `secret-slot` / `toggle` / `chip-dropdown` | v3 | shipped | `ds/components-inputs.html` | **@mythicalos/preact-ui** `Input`, `Toggle`, `Checkbox`, `MaskedSecretInput` (+ the opt-in password reveal) · react-ui mirror | preact-ui 0.4.0 — v3 control-border resting boundary; chip-dropdown grew a dedicated atom in 0.5.x — see the `tags` row |
| `gauge` (arc + bar) | v1 | shipped | `ds/components-gauges.html` | **@mythicalos/preact-ui** `Gauge` (geometry + tone from **@mythicalos/ui-core** `gaugeGeom`/`gaugeTone`) | preact-ui 0.4.0 — thresholds are the package's; a consumer must not redefine them |
| `session-card` | v1 | shipped — PACKAGED | `ds/components-session-card.html` | **@mythicalos/preact-ui** `SessionCard` · react-ui mirror (logic in **@mythicalos/ui-core**) | preact-ui 0.4.0 — absence is not zero and unknown is not idle are pinned by tests |
| `terminal` / `queue-row` / `send-bar` | v2 | shipped — PACKAGED | `ds/components-terminal.html` | **@mythicalos/preact-ui** `Terminal`, `QueuePanel`, `QueueRow`, `SendBar` · react-ui mirror | preact-ui 0.4.0 — pane is heritage-dark in BOTH themes; ASAP takes the first turn gap (never "interrupts") |
| `wizard` | v1 | built in-product, NOT packaged | `ds/components-wizard.html` | product-local: `ui/src/components/WizardFrame.tsx` + the wizard page | — six steps derived from facts, not a stored counter |
| `confirm-dialog` | v1 | shipped | `ds/components-dialogs.html` | **@mythicalos/preact-ui** `ConfirmDialog`/`Scrim` (+ `typedNameMatches` in ui-core) | preact-ui 0.4.0 |
| `toast` | v1 | shipped | `ds/components-toasts.html` | **@mythicalos/preact-ui** `ToastProvider`/`useToast` | preact-ui 0.4.0 |
| `empty-state` | v1 | shipped | `ds/components-empty-states.html` | **@mythicalos/preact-ui** `EmptyState` | preact-ui 0.4.0 |
| `save-bar` | v1 | shipped — PACKAGED | `ds/layouts-settings.html` | **@mythicalos/preact-ui** `SaveBar` · react-ui mirror | preact-ui 0.4.0 — takes human labels, renders null when clean |
| `family-tile` | v1 | built in-product, NOT packaged | `ds/components-org-family.html` | product-local: the settings Family section | — detection is real; copy is design-owned |
| `stat-tiles` | v1 | shipped — PACKAGED | `ds/components-stat-tiles.html` | **@mythicalos/preact-ui** `StatTiles` · react-ui mirror | preact-ui 0.4.0 — takes a generic tile list, not a product view-model |
| `git-chip` | v1 | shipped — PACKAGED | `ds/components-git-chip.html` | **@mythicalos/preact-ui** `GitChip` · react-ui mirror | preact-ui 0.4.0 — an unavailable summary never renders as a zero-count clean row; unpushed is error-toned |
| `tag` / `flag` / `chip-dropdown` | v1 | shipped — PACKAGED | `ds/components-tags.html` | **@mythicalos/preact-ui** `Tag`, `Flag`, `ChipDropdown` · react-ui mirror (classes from **@mythicalos/ui-core** `tagClass`/`flagClass`/`chipDropdownClass`) | preact-ui 0.5.0 — pill = never interactive (the removal `×` is a focusable child, not a clickable tag); `Flag` is mono + tabular for machine facts; `ChipDropdown` owns no menu — pair it with `Popover` |
| `chip` | v1 | shipped — PACKAGED | `ds/components-chip.html` | **@mythicalos/preact-ui** `Chip` · react-ui mirror (classes from **@mythicalos/ui-core** `chipClass`) | preact-ui 0.5.0 — predates the tags family; Tag is a superset in all but the quieter neutral fill. **OPEN: ratify Chip as the variant-free badge, or deprecate toward Tag** — until ruled, new work prefers Tag, existing consumers stay |
| `card` | v1 | shipped — PACKAGED | — (no card yet) | **@mythicalos/preact-ui** `Card` · react-ui mirror | preact-ui 0.5.0 |
| `avatar` | v1 | shipped — PACKAGED | — (no card yet) | **@mythicalos/preact-ui** `Avatar` · react-ui mirror | preact-ui 0.5.0 |
| `status-line` | v1 | shipped — PACKAGED | — (no card yet) | **@mythicalos/preact-ui** `StatusLine` · react-ui mirror (classes from **@mythicalos/ui-core** `statusLineClass`) | preact-ui 0.5.0 |
| `search-input` | v1 | shipped — PACKAGED | — (no card yet) | **@mythicalos/preact-ui** `SearchInput` · react-ui mirror | preact-ui 0.5.0 |
| `banner` | v1 | shipped — PACKAGED | — (no card yet) | **@mythicalos/preact-ui** `Banner` · react-ui mirror (tone + required glyph from **@mythicalos/ui-core** `BANNER_ICON`) | preact-ui 0.5.0 — rule #7: the glyph always rides along |
| `token-gate` | v1 | shipped — PACKAGED | — (no card yet) | **@mythicalos/shell** `TokenGate` — the family unlock screen | shell 0.4.0 — placeholder never states a token length/alphabet (formats differ per product) |
| `tier-bar` / `memory-card` | v1 | built in-product, NOT packaged | `ds/components-memory.html` | product-local: the Memory page exists (`ui/src/pages/memory.tsx`) — never extracted | — registry previously claimed the page did not exist |
| `project-hero` | v2 | built in-product, NOT packaged | `ds/components-project-hero.html` | product-local: `ui/src/pages/projects.tsx` detail hero | — registry previously claimed unimplemented |
| `popover` | v1 | shipped — PACKAGED | `ds/components-popover.html` | **@mythicalos/preact-ui** `Popover` · react-ui mirror (placement + roving focus in ui-core) | preact-ui 0.4.0 |
| `select` | v3 | shipped — PACKAGED | `ds/components-select.html` | **@mythicalos/ui-core** `<mythical-select>` — form-associated web component, native `<select>` parity, groups, progressive mode, `variant="chip"`. Framework-agnostic: usable as-is from Preact or React. | ui-core 0.3.2 — v3 APG combobox pattern; popup is border-box and pinned to both edges (0.3.x) |
| `org-card` | v1 | built in-product, NOT packaged | `ds/components-org-family.html` | product-local: the settings rail footer | — registry previously claimed unimplemented |
| `timeline` | v1 | NEW — unimplemented | `ds/components-timeline.html` | not built — the natural consumer is a scheduler's forward/backward view | — dot and rail derive from the same two custom properties so the dot cannot drift off the line |
| `file-explorer` / `markdown-preview` | v1 | shipped — PACKAGED | `ds/components-file-explorer.html` | **@mythicalos/preact-ui** `FileTree`, `FilePreview`, `FileScopePicker` · react-ui mirror | preact-ui 0.4.0 — read-only; unavailable/empty/too-large/binary are first-class states |

Other consumers: **the Command Center** is legacy — it adopts this system wholesale
in the React rebuild, not component-by-component; don't retrofit it. The **setup
wizard** and the other family-product UIs (skuld/saga) implement from the same rows.

## Dispatching a coding agent

Prompt skeleton for "replace/implement component X in product Y":

> Implement/replace component **`<id>` v`<n>`** in `<product>` per the mythical design
> system (repo `mythical-design` —
> prefer consuming the package over copying). Read, in order: `design.md` (rules),
> `tokens.css` (canonical tokens — import the package's `tokens.css`, never fork values),
> and the component's card under `ds/` (open it in a browser to see it rendered, both themes). Reproduce **all** shown states (hover/focus/
> disabled/error/loading where applicable) using the product's established component
> patterns — this is a recreation in the target stack, not a copy-paste of the reference
> HTML. Verify against the rules-of-use block at the bottom of tokens.css (focus ring,
> status-with-icon, terminal `--my-term-*`, danger-fill scope). Run the package's
> typecheck + tests. Then update the component's row in `COMPONENTS.md`
> ("implements" column) — and nothing else in this repo.

Screen-level context (how components compose on a page): the handoff prototype in the
maintainers' internal design archive. When a dispatch reveals a spec gap, fix the spec first (template + regenerate),
then implement — never let a product fork the design.
