# WebApps Plugin Repository

Official plugin repository for [WebApps](https://github.com/hastagaming/webapps) — a multi-container Android browser.

Plugins let you customize WebApps' appearance (theme colors, UI tweaks) without modifying the app itself. This repo hosts the plugin catalog and files that WebApps downloads directly inside the app.

## How it works

1. You write a plugin **source file** (`.wp`, plain text TOML) inside `plugins-src/<type>/`.
2. A GitHub Actions workflow automatically:
   - Packages your source into a distributable `.wp` file (a ZIP archive containing `manifest.toml`) inside `plugins/`.
   - Updates `index.toml`, the catalog WebApps reads to display available plugins.
3. WebApps' in-app Plugin Browser (**Settings → Plugins**) fetches `index.toml`, lets users download and apply plugins.

You never need to run any script manually or hand-edit `index.toml` — just add your source file and push.

## Repository structure
```
WebApps-plugin/
├── index.toml              # Auto-generated catalog. Do not edit manually.
├── plugins/                 # Auto-generated distributable .wp (zip) files.
├── plugins-src/              # Where you add new plugins.
│   └── theme/
│       └── catppuccin-mocha.wp   # Plain-text TOML source, NOT zipped.
└── .github/workflows/
└── build-plugins.yml     # Builds plugins-src/ into plugins/ + index.toml
```

> **Note on `.wp` files:** files inside `plugins-src/` are plain TOML text (source). Files inside `plugins/` are ZIP archives with the same `.wp` extension (distributable format WebApps actually downloads). They share a name and extension but are different formats — don't confuse them.

## Adding a new plugin

1. Pick a plugin type. Currently supported: `theme`.
2. Create a file at `plugins-src/<type>/<your-plugin-id>.wp` (plain text, TOML format).
3. Fill in the required fields (see [Theme Plugin Format](#theme-plugin-format) below).
4. Commit and push to `main`. The workflow runs automatically and builds everything for you.

Your plugin will appear in the app's Plugin Browser within a few minutes of the workflow completing.

## Theme Plugin Format

Example: `plugins-src/theme/catppuccin-mocha.wp`

```toml
name = "Catppuccin Mocha"
description = "Soothing pastel dark theme"
author = "hastagaming"
version = "1.0.0"
previewColorHex = "#CBA6F7"
colorPrimary = "#CBA6F7"
colorBackground = "#1E1E2E"
colorSurface = "#313244"
colorSurfaceVariant = "#45475A"
colorOnSurface = "#CDD6F4"
colorOnPrimary = "#1E1E2E"
colorError = "#F38BA8"
uiCornerRadiusDp = 12
uiGridMinTileWidthDp = 100
```

|Field | Required | Description|
|---|---|---|
|name | Yes | Display name shown in the Plugin Browser|
|description | Yes | Short one-line description|
|author | Yes | Your name or GitHub username|
|version | Yes | Semantic version, e.g. 1.0.0|
|previewColorHex | Yes | Hex color shown as a preview swatch before download|
|colorPrimary | Yes | Primary accent color (buttons, highlights)|
|colorBackground | Yes | App background color|
|colorSurface | Yes | Card/surface background color|
|colorSurfaceVariant | Yes | Secondary surface color (e.g. dividers, disabled states)|
|colorOnSurface | Yes | Text/icon color on top of surfaces|
|colorOnPrimary | Yes | Text/icon color on top of primary-colored elements|
|colorError | Yes | Error state color|
|uiCornerRadiusDp | No (default 12) | Corner radius for cards and buttons, in dp|
|uiGridMinTileWidthDp | No (default 100) | Minimum width for container grid tiles, in dp|

All color fields must be valid hex codes (#RRGGBB).

## Plugin ID rules

- The plugin ID is derived from the filename (without .wp), e.g. - catppuccin-mocha.wp → id catppuccin-mocha.
- Use lowercase letters, numbers, and hyphens only.
- IDs must be unique across the entire repository, regardless of type folder.

## Contributing

Pull requests are welcome! To submit a plugin:
- Fork this repository.
- Add your .wp source file under the appropriate plugins-src/<type>/ folder.
- Open a pull request. Once merged into main, the workflow will build and publish it automatically.

Please make sure your plugin:
- Has readable contrast between colorOnSurface/colorOnPrimary and their respective backgrounds.
- Uses a unique, descriptive plugin ID.
- Includes accurate author attribution.

## License
Plugins in this repository are contributed under [MIT](./LICENSE) License unless otherwise noted by the plugin author. By submitting a plugin, you agree to license it under these terms.