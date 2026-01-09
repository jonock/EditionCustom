# AGENTS.md

Instructions for AI agents working on this codebase.

## Project Overview

This is **Edition**, a newsletter theme for the [Ghost](https://ghost.org) publishing platform. It's a customized version of the official Ghost Edition theme with additional features like a statement box component.

## Tech Stack

- **Templating**: Handlebars (`.hbs` files)
- **CSS**: PostCSS with autoprefixer and cssnano
- **JavaScript**: Vanilla JS with minification via Gulp/Uglify
- **Build Tool**: Gulp 4
- **Package Manager**: Yarn
- **Testing**: gscan (Ghost theme validator)
- **Target Platform**: Ghost CMS >= 5.0.0

## Directory Structure

```
/
├── *.hbs                    # Page templates (index, post, page, tag, author, etc.)
├── partials/                # Reusable template components
│   ├── icons/               # SVG icon partials
│   └── *.hbs                # Component partials (loop, cover, pagination, etc.)
├── assets/
│   ├── css/                 # Source CSS files (organized by category)
│   │   ├── general/         # Base styles (fonts, buttons, forms, icons)
│   │   ├── site/            # Layout, header, cover styles
│   │   ├── blog/            # Post feed, single post, author, taxonomy styles
│   │   ├── misc/            # Utilities, animations, third-party overrides
│   │   └── screen.css       # Main entry point (imports all other CSS)
│   ├── js/
│   │   ├── lib/             # Third-party JS libraries
│   │   └── main.js          # Custom JavaScript
│   ├── built/               # Compiled output (DO NOT edit directly)
│   ├── fonts/               # Web fonts (Lora, Mulish)
│   └── images/              # Theme images
├── locales/                 # Translation files (JSON)
├── package.json             # Theme config and dependencies
└── gulpfile.js              # Build configuration
```

## Development Commands

```bash
# Install dependencies
yarn

# Build and watch for changes (development)
yarn dev

# Run theme validation
yarn test

# Build and create distribution zip
yarn zip
```

## Key Conventions

### Handlebars Templates

- Use Ghost's Handlebars helpers: `{{#is}}`, `{{#if}}`, `{{#foreach}}`, `{{#match}}`, etc.
- Access site settings via `@site` (e.g., `@site.title`, `@site.logo`)
- Access custom theme settings via `@custom` (e.g., `@custom.navigation_layout`)
- Include partials with `{{> "partial-name"}}` syntax
- Use `{{t "string"}}` for translatable strings

### Custom Theme Settings

Theme settings are defined in `package.json` under `config.custom`. Current custom options include:
- `navigation_layout`: Logo positioning (left/middle/stacked)
- `title_font` / `body_font`: Font family selection
- `publication_cover_style`: Fullscreen or half screen
- `feed_layout`: Post list style (Expanded/Right thumbnail/Text-only/Minimal)
- `show_featured_posts`, `show_author`, `show_related_posts`: Toggle features
- `statement_box_title` / `statement_box_text`: Custom statement box content

### CSS Guidelines

- Source CSS lives in `assets/css/` - never edit `assets/built/`
- Main entry point is `assets/css/screen.css` which imports all other CSS
- Uses CSS custom properties (variables) from Ghost's shared theme assets
- Imports are processed by `postcss-easy-import`
- CSS is autoprefixed and minified during build

### JavaScript Guidelines

- Custom JS goes in `assets/js/main.js`
- Third-party libraries go in `assets/js/lib/`
- JS is concatenated and minified to `assets/built/main.min.js`

## Testing

Run `yarn test` before committing to validate the theme with gscan. This checks:
- Required files and structure
- Valid Handlebars syntax
- Ghost API compatibility
- Accessibility issues

## CI/CD

The GitHub Actions workflow (`.github/workflows/main.yml`) automatically deploys the theme to Ghost when pushing to `main` or `master` branches. It requires these secrets:
- `GHOST_ADMIN_API_URL`
- `GHOST_ADMIN_API_KEY`

## Common Tasks

### Adding a New Partial

1. Create `.hbs` file in `partials/`
2. Include it in templates with `{{> "partial-name"}}`

### Adding a New Custom Setting

1. Add the setting definition to `package.json` under `config.custom`
2. Use it in templates via `@custom.setting_name`

### Adding a New Icon

1. Create `.hbs` file in `partials/icons/` with SVG content
2. Include it with `{{> "icons/icon-name"}}`

### Modifying Styles

1. Edit appropriate CSS file in `assets/css/`
2. Run `yarn dev` to compile and watch for changes
3. Compiled output appears in `assets/built/`

## Important Notes

- The `assets/built/` directory contains compiled assets and should not be edited manually
- Image sizes are configured in `package.json` under `config.image_sizes`
- The theme supports Ghost's membership features (sign in, subscribe, account)
- Translation strings should use the `{{t}}` helper for internationalization
