# @mythicalos/tokens

The **foundation layer** of the mythicalOS design system — canonical design
tokens, self-hosted fonts, and brand assets. This package owns *look*, not
behavior: no components, no framework code. It is consumed by every mythicalOS
frontend and by the component library (`@mythicalos/ui-core` + its Preact/React
bindings), which builds its components on top of these tokens.

## Install

```sh
npm add @mythicalos/tokens
```

```js
import "@mythicalos/tokens/styles";   // canonical tokens, both themes (light + heritage dark)
```

`assets/*` (logo SVGs/PNGs, woff2 fonts) are exported as package paths.

## What it ships

| Path | What it is |
|------|-----------|
| `tokens.css` | Canonical CSS custom properties — petrol accent, warm-paper light + heritage dark, Inter + JetBrains Mono, an 8-step type scale, radii, spacing, status colors, motion. Import from the package — never fork the values. |
| `assets/fonts/` | Self-hosted Inter + JetBrains Mono (SIL OFL 1.1; license files included). |
| `assets/logo-*.{svg,png}` | The mythical brand mark, light and dark. |
| `design.md` | **The living brand + product design book** — direction, brand, tokens, type, shell/IA, component rules, and the decision log. The authoritative reference. |
| `COMPONENTS.md` | The component registry — one row per component (ID · spec version · status). The dispatch index for the design system. |
| `CHANGELOG.md` | Version history. The package minor tracks the book version (`0.5.x` ↔ book v0.5). |

## Theming

Light is the default; heritage dark activates under `[data-theme="dark"]` on
the document root. The terminal palette (`--my-term-*`) is fixed in both themes.

## Hard rules (enforced in review across the family)

1. **Tokens only** — no hard-coded hex, px font-size, or radius downstream.
2. **Shape encodes interactability** — squared radius = clickable; a pill is a
   non-interactive tag only.
3. **Status colors are status-only**, never decorative. Amber = warn only.
4. **One focus ring** — 2px petrol, `outline-offset: 2px`, on every control, both themes.
5. **No inline styles** (strict CSP). Dynamic visuals ride SVG presentation
   attributes or classes — never `style=`.

## Components & previews

Components (the `<mythical-select>` web component, the Preact/React atoms, the
family shell) and their live previews live in the component library repo, not
here — this repo is deliberately stable and framework-free.

**`ds/` is the exception, and it is load-bearing: it is the sync source for the
claude.ai Design pane.** The 27 self-contained cards there are what the pane
renders — do not delete them as stale duplicates of the component library's own
preview cards. The component library keeps its own smaller card set for local
preview and drift control against its component sources; the two sets overlap but
are **not** interchangeable, because each card's `@dsInline` marker names a
canonical path that only its own repo resolves. Content edits to a shared card
must be applied in both places, by hand, in the same wording.

Note the cards' `@dsInline`/`@dsFonts` markers reference `components/` and
`scripts/check-ds.sh` — neither has ever existed in this repo. The cards are
generated elsewhere and committed here; there is no in-repo tooling to
regenerate or verify them, and CI does not check them.

## Licence and the paid tier

**Apache-2.0** — see [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE). Bundled fonts
are licensed separately under the SIL Open Font License 1.1 (see
`assets/fonts/LICENSE-*`). The licence covers the code, not the names and
branding — see [`TRADEMARK.md`](TRADEMARK.md).

Everything this package ships is open and stays open. It is not a reduced build
of a paid one, and no token, asset, or rule is held back for a commercial tier.
mythicalOS does sell a hosted, multi-user tier — that is separate, private
software, and it consumes these packages on exactly the terms you do.

Apache-2.0 lets you use, modify, redistribute, and build commercial products on
this package — including products that compete with ours — provided you keep the
licence and attribution notices intact. Contributions are accepted under the same
licence with a [DCO](https://developercertificate.org/) sign-off and **no CLA**
(see [`CONTRIBUTING.md`](CONTRIBUTING.md)): we take no copyright assignment and
no relicensing right, so this project cannot be moved off Apache-2.0 without
every contributor's agreement, and anything already released under it stays
available under it.
