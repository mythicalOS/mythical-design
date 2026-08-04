# AGENTS.md — mythical-design

The **foundation layer** of the mythicalOS design system, published to npm as
`@mythicalos/tokens`: canonical design tokens (`tokens.css`), self-hosted fonts, brand assets,
and the living design book (`design.md`) + component registry (`COMPONENTS.md`). This package
owns *look, not behavior* — no components, no framework code, no build step.

## Authority & precedence

Repository orientation, not a role contract. If a role, playbook, or system prompt governs your
session, that contract is authoritative and supersedes anything here. This file grants no edit,
run, commit, push, publish, or release permission.

## Layout

`README.md` carries the shipped-file table and the five hard rules (tokens only; shape encodes
interactability; status colors are status-only; one focus ring; no inline styles). `design.md`
is the authoritative design book; the package minor tracks the book version.

## Commands

There is no build, test, or typecheck script — this is a content package. **Publishing is a
release action**: it rides a `v*` git tag through `.github/workflows/release.yml` (npm trusted
publishing). Never `npm publish` locally (a local `npm whoami` 401 is expected and fine), and
never push a `v*` tag without explicit release authority.

## Boundaries & gotchas

- **`ds/` is the sync source for the claude.ai Design pane, and it is load-bearing.** The cards
  are generated elsewhere and committed here; there is **no in-repo tooling** to regenerate or
  verify them, and CI does not check them — drift here is silent, so treat card edits as
  deliberate, mirrored changes (see README "Components & previews").
- The cards' `@dsInline`/`@dsFonts` markers name paths (`components/`, `scripts/check-ds.sh`)
  that have **never existed in this repo**. Do not "fix" the markers, and do not create those
  paths — this repo must never depend on the component library (no dependency cycle).
- A card shared with the component library's own preview set is edited **in both repos, by
  hand, in the same wording** — never bulk-copied in either direction, and neither set is ever
  deleted as a "duplicate" of the other.
- **`tokens.css` values are canonical** — downstream consumers import them and never fork them;
  the same applies here: changing a token is a design-book decision, not a spot edit.
- CI runs a `docs-bar` content gate on this public repository — keep all content and commit
  messages free of internal project vocabulary.
- This repo is consumed as a pinned submodule by a private downstream workspace: land and push
  on `main` here first, then the consumer bumps its pin.
