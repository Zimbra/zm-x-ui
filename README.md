## @zimbra/x-ui
This repository contains the LESS variables and mixins used by [`zm-x-web`](https://github.com/Zimbra/zm-x-web) and Zimbra X Zimlets.

### Install

```sh
npm install @zimbra/x-ui
```

### Usage

Requiring LESS variables (see [variables.less](https://github.com/Zimbra/zm-x-ui/blob/master/variables.less)):

```css
// Gives access to @brand-primary, @icon-size-md, etc.
@import '~@zimbra/x-ui/refs.less';
```

Requiring Mixins (see [helpers.less](https://github.com/Zimbra/zm-x-ui/blob/master/helpers.less)):

```css
// Gives access to `fit`, `fill`, etc.
@import '~@zimbra/x-ui/helpers.less';
```

Requiring CSS Variables (recommended for Zimlets, see [css-variables.less](https://github.com/Zimbra/zm-x-ui/blob/master/css-variables.less)):

```css
// Gives access to `--brand-primary`, `--brand-secondary`, etc.
@import '~@zimbra/x-ui/css-variables.less';
```
