# Vendor bundle

`md-bundle.js` is Material Web components (buttons, icon buttons, dialog,
switch, outlined text field, circular progress, divider, icon) bundled
with esbuild from the source of
[material-components/material-web](https://github.com/material-components/material-web)
at @material/web 2.5.0 plus labs utility additions. A pinned local copy lives
in the `material-design` vendor repo at `~/Documents/material-design/material-web`.

`md-tokens.css` is the MD3 color scheme (light/dark/auto) generated from
seed `#2d5d8f` with @material/material-color-utilities (SchemeTonalSpot).

To regenerate:

```bash
cp -R ~/Documents/material-design/material-web mw && cd mw   # or clone upstream
npm ci && npm run build:sass && npm run build:css-to-ts
cd .. && npm init -y && npm i esbuild lit tslib @material/material-color-utilities
# entry.js imports the component set listed above
npx esbuild entry.js --bundle --minify --format=esm --target=es2021 \
  --alias:@material/web=./mw --outfile=md-bundle.js
```
