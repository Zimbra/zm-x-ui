# LESS Consumer Migration Plan

## Goal

Move stylesheet assets that do not require LESS evaluation to CSS while keeping
the remaining compile-time APIs explicit. This release changes the package entry
point from `index.less` to `index.css` and converts the font and icon styles to
CSS. The remaining LESS files are intentionally retained because they expose
LESS variables, mixins, or compile-time client overrides.

## Package changes

| Previous path | New path | Consumer action |
|---|---|---|
| `@zimbra/x-ui/index.less` | `@zimbra/x-ui/index.css` | Update the import path. |
| `@zimbra/x-ui/fonts.less` | `@zimbra/x-ui/fonts.css` | Update direct imports, if any. |
| `@zimbra/x-ui/icons.less` | `@zimbra/x-ui/icons.css` | Update direct imports, if any. |
| `@zimbra/x-ui/css-variables.less` | `@zimbra/x-ui/css-variables.css` | Correct the obsolete path documented by earlier releases. |

The `main` and `style` package fields now resolve to `index.css`.

## LESS that remains

| File | Why it cannot be renamed directly |
|---|---|
| `variables.less` | Exposes the five breakpoint constants still used by `helpers.less`; CSS custom properties cannot be used in ordinary media-query conditions. |
| `refs.less` | Uses a LESS reference import to expose those breakpoint constants without emitting rules. |
| `helpers.less` | Exposes callable mixins, nested selectors, and media queries that consume LESS breakpoint variables. |

## Consumer rollout

### 1. Inventory imports

Search every consumer repository before upgrading:

```sh
git grep -nE '@zimbra/x-ui/(index|fonts|icons|css-variables)\.less'
git grep -nE '@zimbra/x-ui/(refs|variables|helpers)\.less'
git grep -n '"@zimbra/x-ui"'
```

Public GitHub code search at the time of this migration found 55 Zimbra package
manifests depending on `@zimbra/x-ui`, 95 files referencing `refs.less`, and 312
files referencing `helpers.less`. These counts are discovery aids, not a complete
dependency inventory; private repositories and generated sources may add more.

### 2. Adopt the CSS entry points

Replace imports of the converted files:

```diff
-@import '~@zimbra/x-ui/index.less';
+@import '@zimbra/x-ui/index.css';

-@import '~@zimbra/x-ui/fonts.less';
+@import '@zimbra/x-ui/fonts.css';

-@import '~@zimbra/x-ui/icons.less';
+@import '@zimbra/x-ui/icons.css';

-@import '~@zimbra/x-ui/css-variables.less';
+@import '@zimbra/x-ui/css-variables.css';
```

If JavaScript owns the global stylesheet import, use:

```js
import '@zimbra/x-ui/index.css';
```

Confirm that the consumer's CSS pipeline:

- resolves CSS imports from `node_modules`;
- copies font URLs referenced from package CSS;
- does not scope dependency CSS as local CSS Modules; and
- preserves the order of theme variables and component styles.

### 3. Verify rendered assets

For each application or Zimlet:

1. Build its production bundle.
2. Confirm the `zimbra-icons` font files are emitted and load without 404s.
3. Verify representative icon glyphs, including the replacement glyph for an
   unknown icon.
4. Verify Roboto weights 100, 300, 400, 500, and 700 in normal and italic styles.
5. Compare the generated CSS and screenshots with the previous package version.

### 4. Migrate the remaining LESS APIs separately

Do not remove the final three LESS files until consumers have moved away from
their compile-time APIs:

- Replace removed color, spacing, typography, and dimension aliases with their
  existing `var(--token-name)` equivalents.
- Replace breakpoint variables with literal shared breakpoints or a consumer
  build-time custom-media solution; CSS custom properties cannot be used in
  ordinary media-query conditions.
- Replace helper mixin calls with component-local declarations or deliberately
  named utility classes.
- Replace removed `DEFAULTCLIENT` and `CLIENT` variable overrides with CSS
  custom-property override stylesheets loaded after the base token stylesheet.

### 5. Completion criteria

The ecosystem is ready to remove LESS entirely when:

- no consumer imports `refs.less`, `variables.less`, or `helpers.less`;
- no consumer calls a mixin defined by `helpers.less`;
- client-specific themes override CSS custom properties rather than LESS values;
- all consumer builds exclude `less` and `less-loader`; and
- production smoke tests cover fonts, icons, responsive breakpoints, and branded
  themes.
