[![NPM Downloads](https://img.shields.io/npm/dm/@zimbra/x-ui.svg?style=flat)](https://www.npmjs.com/package/@zimbra/x-ui)
[![NPM Version](https://img.shields.io/npm/v/@zimbra/x-ui.svg?style=flat)](https://www.npmjs.com/package/@zimbra/x-ui)

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


### Updating the Icon font using icomoon

To support all platforms we require several versions of our icon font.
Updating the full set of files is easily accomplished by the use of the **icomoon** tool.

1. Check out the desired version of this repo.

2. Run the icomoon app in Chrome: https://icomoon.io/app
This will open to the Selection screen by default.

3. If you have a `zimbra-icons` set from a previous iteration, **Remove Set** to start with a blank slate.

4. Click **Import Icons** then navigate to your checked out repo and open `selection.json`.
A confirmation dialog will ask:

   > Your icon selection was loaded.
   Would you like to load all the settings stored in your selection file?

   Answer **Yes**, so that the existing codepoints for the icon font will be retained.

5. From the Hamburger menu for the imported zimbra-icons set, select **Import to Set** then navigate to and open the new icon svg file(s) that you want to add to the set.

6. To export as a font, first select all the glyps in the set. (Hamburger, **Select all**.)
Click on the **Generate Font** tab along the bottom of the display.
Review the resulting glyphs and metadata.

7. When you're happy with what you see, click the **Download** action that's now on the **Font** tab, to download `zimbra-icons.zip`.

Update the repo with the contents of the zip file by running [import-icomoon.sh](https://gist.github.com/pl12133/aadc10ad45be4952336b62b39c9e8c3a). 

1. Set environment variable `ZM_X_UI_DIR` to point to your `zm-x-ui` directory; the default is `$HOME/github/Zimbra/zm-x-ui`

   ```sh
   $: export ZM_X_UI_DIR="$HOME/path/to/zm-x-ui"
   ```

2. Run script with path to `zimbra-icon.zip` file as argument:
 
   ```sh
   $: ./import-icomoon.sh ./path/to/zimbra-icon.zip
   ```
