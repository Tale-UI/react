# @tale-ui/react

Opinionated React component library forked from [MUI Base UI](https://github.com/mui/base-ui), styled with [@tale-ui/core](https://github.com/Tale-UI/core) design tokens.

Components are headless by nature (courtesy of Base UI) with styles applied separately via `@tale-ui/react-styles`.

---

## Packages

| Package                 | Description                                   |
| ----------------------- | --------------------------------------------- |
| `@tale-ui/react`        | Headless component primitives (Base UI fork)  |
| `@tale-ui/react-styles` | CSS layer using `@tale-ui/core` design tokens |
| `@tale-ui/utils`        | Shared utilities (Base UI utils fork)         |

---

## Installation

```bash
npm install @tale-ui/react @tale-ui/react-styles
```

**Requirements:** React 17, 18, or 19. Node 22+.

---

## Usage

Import a component and its styles:

```tsx
import { Button } from '@tale-ui/react/button';
import '@tale-ui/react-styles/button';

export function MyButton() {
  return <Button className="tale-button tale-button--primary">Click me</Button>;
}
```

Or import all styles at once:

```tsx
import '@tale-ui/react-styles';
```

---

## Components

Accordion, Alert Dialog, Autocomplete, Avatar, Button, Checkbox, Checkbox Group, Collapsible, Combobox, Context Menu, Dialog, Drawer, Field, Fieldset, Form, Input, Menu, Menubar, Meter, Navigation Menu, Number Field, Popover, Preview Card, Progress, Radio, Radio Group, Scroll Area, Select, Separator, Slider, Switch, Tabs, Toast, Toggle, Toggle Group, Toolbar, Tooltip

---

## Styling

Styles live in `packages/styles/src/` and are separate from component source. Components use [BEM](https://getbem.com/) class names prefixed with `tale-`:

```css
.tale-button { ... }
.tale-button--primary { ... }   /* variant */
.tale-button--sm { ... }        /* size */
.tale-button[data-disabled] { opacity: 0.45; }
```

State is exposed via Base UI's `data-*` attributes — `data-disabled`, `data-open`, `data-checked`, `data-selected`, `data-highlighted`, and others — which can be targeted directly in CSS.

---

## Development

```bash
pnpm install                 # install all workspace deps
pnpm start                   # install + launch playground
pnpm playground:dev          # run vite playground
pnpm storybook               # run storybook
pnpm build                   # build all packages
pnpm test:jsdom              # unit tests (jsdom)
pnpm test:chromium           # unit tests (browser)
pnpm typescript              # type check
pnpm eslint                  # lint
```

### Automated npm publishing

Publishing is automated from GitHub Releases via [.github/workflows/release-publish-npm.yml](.github/workflows/release-publish-npm.yml).

Requirements:

- Repository secret `NPM_TOKEN` with publish permissions for the `@tale-ui` npm scope.
- Create a GitHub release tag in the format `vX.Y.Z` (example: `v0.0.3`).

When a release is published, the workflow:

- Syncs `@tale-ui/utils`, `@tale-ui/react`, and `@tale-ui/react-styles` to the same version from the release tag.
- Publishes each package to npm (skips any package/version that already exists).

### Syncing upstream Base UI changes

```bash
git remote add upstream https://github.com/mui/base-ui
git fetch upstream
git cherry-pick <commit>
```

---

## License

MIT
