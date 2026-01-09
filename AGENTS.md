# AGENTS.md

Guidance for AI/code agents working in this repository.

## Project overview

This is **Edition**, a newsletter-focused theme for the **Ghost** publishing platform. The theme is primarily:

- Handlebars templates (`*.hbs`, `partials/**/*.hbs`)
- CSS sources in `assets/css/**` compiled to `assets/built/**` via Gulp/PostCSS
- JavaScript sources in `assets/js/**` bundled/minified to `assets/built/main.min.js` via Gulp

The theme targets Ghost `>= 5.0.0` (see `package.json` `engines.ghost`).

Upstream note: this repository is typically synced from the official themes monorepo (`TryGhost/Themes`). If you’re making changes intended for upstream distribution, prefer targeting the monorepo’s workflow.

## Setup

Install dependencies with Yarn:

```bash
yarn
```

## Common commands

- **Build CSS/JS once (recommended for CI-style validation)**:

```bash
npx gulp build
```

- **Watch + live reload during development (long-running)**:

```bash
yarn dev
```

- **Theme validity check (Ghost theme linter)**:

```bash
yarn test
```

- **Create a zip for upload to Ghost Admin**:

```bash
yarn zip
```

Notes:
- `yarn test` runs `gscan .` which validates templates, assets, and theme structure.
- The zip task writes to `dist/<package-name>.zip` (name comes from `package.json`).

## Where to make changes

- **Templates/layout**: root `*.hbs` files such as `default.hbs`, `index.hbs`, `post.hbs`, `page.hbs`, `tag.hbs`, `author.hbs`
- **Reusable template fragments**: `partials/**/*.hbs`
- **Styles**:
  - Edit source in `assets/css/**` (entrypoint: `assets/css/screen.css`)
  - Gulp compiles to `assets/built/screen.css` (+ sourcemap)
- **Scripts**:
  - Edit source in `assets/js/**`
  - Gulp bundles shared assets + local scripts to `assets/built/main.min.js` (+ sourcemap)
- **Theme settings** (customization options exposed in Ghost Admin): `package.json` under `config.custom`
- **Translations**: `locales/*.json`

## Build artifacts (`assets/built`)

This repo includes compiled assets in `assets/built/**`.

- Prefer editing **source** files in `assets/css/**` and `assets/js/**`.
- Regenerate built files with `npx gulp build` (or `yarn dev`) so changes are reflected in `assets/built/**`.
- Do not hand-edit minified files in `assets/built/**` unless you are intentionally patching generated output.

## Conventions & safety checks

- **Keep Handlebars compatible with Ghost**: use Ghost theme helpers and patterns; avoid introducing unsupported Node/SSR logic.
- **Run validation**: after template changes, run `yarn test` and address `gscan` warnings/errors.
- **Avoid breaking theme packaging**: `gulp zip` excludes `node_modules/` and `dist/`; don’t add required runtime files under excluded paths.
- **Stay consistent with existing structure**: prefer updating existing partials/assets over introducing new entrypoints.

## CI / GitHub Actions

This repo contains a workflow that can deploy the theme to a Ghost instance on pushes to `main`/`master` using secrets.
Avoid changes that would require additional secrets or interactive steps.

## Quick “did I break anything?” checklist

```bash
yarn
npx gulp build
yarn test
yarn zip
```

