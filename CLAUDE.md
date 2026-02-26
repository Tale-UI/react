# @tale-ui/react — AI Reference

This is **@tale-ui/react**: a fork of [MUI base-ui](https://github.com/mui/base-ui) with opinionated styling applied via `@tale-ui/core` design tokens.

## Repo Structure

```
packages/
  react/    → @tale-ui/react   — headless component source (forked from @base-ui/react)
  styles/   → @tale-ui/react-styles — CSS per component (uses @tale-ui/core tokens)
  utils/    → @tale-ui/utils   — shared utilities (forked from @base-ui/utils)
playground/ → vite-app         — local dev/testing app
test/       → test infrastructure
```

**Package manager:** pnpm (workspaces). Run `pnpm install` at repo root.
**Upstream:** `git remote add upstream https://github.com/mui/base-ui` — use to cherry-pick upstream fixes.

---

## Styling Architecture

Styling lives in **`packages/styles/src/`** — NOT in the component source. Components remain headless.

### How to style a component

1. Create `packages/styles/src/{component}.css`
2. Import it in `packages/styles/src/index.css`
3. Add the export path to `packages/styles/package.json` exports

**Component CSS pattern:**
```css
/* Use BEM for component internals */
.tale-button { ... }
.tale-button--primary { ... }   /* variant */
.tale-button--sm { ... }        /* size modifier */
.tale-button[data-disabled] { opacity: 0.45; pointer-events: none; }
.tale-button[data-open] { ... } /* state from tale-ui data attrs */
```

**Usage in consuming app:**
```tsx
import { Button } from '@tale-ui/react/button';
import '@tale-ui/react-styles';  // all styles
// or: import '@tale-ui/react-styles/button';  // just button

<Button className="tale-button tale-button--primary">Click</Button>
```

### Tale UI state data attributes (for CSS targeting)

Each Tale UI component uses `data-*` attributes for interactive states:
- `data-disabled` — component is disabled
- `data-open` — popup/overlay is open (Dialog, Popover, Select, etc.)
- `data-checked` — toggle/checkbox is checked
- `data-selected` — item is selected
- `data-highlighted` — item is highlighted (keyboard nav)
- `data-focus-visible` — keyboard focused
- `data-side="top|bottom|left|right"` — positioning side (popovers/tooltips)

Check `packages/react/src/{component}/{Component}DataAttributes.tsx` for the full list per component.

---

## Design System — @tale-ui/core

**Install:** `npm install @tale-ui/core`
**Import:** `@import '@tale-ui/core';`

This is already imported at the top of `packages/styles/src/index.css`.

### Token quick reference — do NOT guess these

**Colors (use `--neutral-*` and `--color-*` in CSS, NEVER `--brand-*`):**
- `--neutral-*` shades: `5 10 12 14 16 18 20 22 24 26 28 30 40 50 60 70 80 82 84 86 88 90 92 94 96 98 100`
- `--color-*` shades: `5 10 20 30 40 50 60 70 80 90 100` — inverts automatically in dark mode
- `--brand-*` shades: `5 10 20 30 40 50 60 70 80 90 100` — **NEVER use in component CSS**
  - `--color-60` = brand-60 in light, brand-40 in dark (always use `--color-*` not `--brand-*`)
- Named colours for specific use: `--red-60`, `--green-60`, etc. (don't invert — use sparingly)

**Spacing:** `--space-4xs` `--space-3xs` `--space-2xs` `--space-xs` `--space-s` `--space-m` `--space-l` `--space-xl` `--space-2xl` `--space-3xl` `--space-4xl`

**Typography:** `--label-s-font-size` `--label-m-font-size` `--label-l-font-size` `--text-s-font-size` etc.
Font stacks: `--label-font-family` `--body-font-family` `--mono-font-family`
Font weights: `--label-font-weight` (500) `--text-font-weight` (400)

**Component colour pattern:**
```css
.component {
  background: var(--neutral-14);        /* subtle bg */
  border: 1px solid var(--neutral-22);  /* subtle border */
  color: var(--neutral-90);             /* body text */
}
.component--primary {
  background: var(--color-60);          /* brand fill (inverts in dark) */
  color: var(--neutral-100);            /* near-white text */
}
```

**Dark mode** is automatic via `data-color-mode="dark"` on `<html>`. `--neutral-*` and `--color-*` invert, `--brand-*` does NOT.

**Utility classes (double-dash syntax):** `.gap--m` `.radius--m` `.shadow--s` `.grid--3` `.display--none`
**Theme classes (single-dash):** `.color-red` `.neutral-cool`

**Full reference:** https://raw.githubusercontent.com/Tale-UI/core/main/packages/css/docs/ai-reference.md
**Contributor guide:** https://raw.githubusercontent.com/Tale-UI/core/main/packages/css/CLAUDE.md

---

## Development Workflow

```bash
pnpm install                    # install all workspace deps
pnpm -C playground/vite-app dev # run playground for visual testing
pnpm build                      # build all packages (uses lerna)
pnpm test:jsdom                 # run unit tests in jsdom
pnpm test:chromium              # run tests in chromium
pnpm typescript                 # type check
pnpm eslint                     # lint
```

### Adding a new component style

1. Check `packages/react/src/{component}/{Component}DataAttributes.tsx` for state attrs
2. Create `packages/styles/src/{component}.css` using `--neutral-*` / `--color-*` tokens
3. Add `@import './{component}.css';` to `packages/styles/src/index.css`
4. Add export to `packages/styles/package.json` exports map
5. Test in `playground/vite-app/`

### Syncing upstream changes

```bash
git fetch upstream
git log upstream/main --oneline | head -20   # see what's new
git cherry-pick <commit>                     # pick specific fixes
```

---

## Component source conventions

Component files follow the original base-ui pattern:
```
packages/react/src/{component}/
  {Component}.tsx                  ← implementation
  {Component}DataAttributes.tsx   ← data-* attribute enum
  {Component}.test.tsx            ← unit tests
  {Component}.spec.tsx            ← browser tests (optional)
  index.ts                        ← public API re-export
```
