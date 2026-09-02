<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/logo-dark.svg">
    <img src="assets/logo-light.svg" alt="mythicalOS" width="84" height="84">
  </picture>
</p>

<h1 align="center">mythical-design</h1>

<p align="center">
  <strong>The design foundation of mythicalOS — canonical tokens, self-hosted fonts, and brand assets. Published as <code>@mythicalos/tokens</code>.</strong>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-Apache_2.0-blue.svg" alt="License: Apache-2.0"></a>
  <a href="https://www.npmjs.com/package/@mythicalos/tokens"><img src="https://img.shields.io/npm/v/@mythicalos/tokens.svg?logo=npm&color=cb3837" alt="npm: @mythicalos/tokens"></a>
  <a href="https://mythicalos.ai"><img src="https://img.shields.io/badge/part_of-mythicalOS-0F6B66.svg" alt="Part of mythicalOS"></a>
</p>

---

This package owns *look*, not behaviour — no components, no framework code. It is consumed by every
mythicalOS frontend and by the component library
([`@mythicalos/ui-core`](https://github.com/mythicalOS/mythical-ui)), which builds its components on
top of these tokens.

## Install

```sh
npm add @mythicalos/tokens
```

```js
import "@mythicalos/tokens/styles";   // canonical tokens, both themes (light + heritage dark)
```

Logo SVGs/PNGs and woff2 fonts are exported as package paths under `assets/*`.

## What it ships

| Path | What it is |
|------|------------|
| `tokens.css` | The canonical CSS custom properties — petrol accent, warm-paper light + heritage dark, Inter + JetBrains Mono, type scale, spacing, radii, status colours, motion. Import them; never fork the values. |
| `assets/fonts/` | Self-hosted Inter + JetBrains Mono (SIL OFL 1.1; licence files included). |
| `assets/logo-*.{svg,png}` | The mythical brand mark, light and dark. |
| `design.md` | The living brand + product design book — the authoritative reference. |
| `COMPONENTS.md` | The component registry (one row per component: ID · spec version · status). |

## The five hard rules

Enforced in review across the family:

1. **Tokens only** — no hard-coded hex, px font-size, or radius downstream.
2. **Shape encodes interactability** — a squared radius is clickable; a pill is a non-interactive tag.
3. **Status colours are status-only**, never decorative (amber = warn only).
4. **One focus ring** — 2px petrol, `outline-offset: 2px`, on every control, both themes.
5. **No inline styles** (strict CSP) — dynamic visuals ride classes or SVG attributes, never `style=`.

Components and their live previews live in the [component library](https://github.com/mythicalOS/mythical-ui),
not here — this repo stays stable and framework-free.

> **The `ds/` card set is load-bearing**, not a stale duplicate: it is the sync source for the
> claude.ai Design pane. Don't delete it, and apply shared-card edits by hand in both repos. See
> [`design.md`](design.md).

## License

**Apache-2.0** — see [LICENSE](LICENSE) and [NOTICE](NOTICE); bundled fonts are SIL OFL 1.1
(`assets/fonts/LICENSE-*`). The licence covers the code, not the mythicalOS name and marks
([TRADEMARK.md](TRADEMARK.md)). Everything here is open and stays open — nothing is held back for the
separate, private paid tier. Contributions welcome under a DCO sign-off, no CLA — see
[CONTRIBUTING.md](CONTRIBUTING.md).
