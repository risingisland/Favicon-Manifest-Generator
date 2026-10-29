# Favicon & Web App Manifest Generator

A single-file, browser-based tool that turns one source image into a complete favicon package: 22 PNG icons, a multi-resolution `favicon.ico`, a `manifest.json`, a `browserconfig.xml`, and a ready-to-paste HTML `<head>` snippet.

Everything runs client-side. Your image is never uploaded to a server.

## Features

- **One image in, full package out**: 22 PNGs plus `favicon.ico`, `manifest.json` and `browserconfig.xml`
- **Multiple input methods**: click to browse, drag and drop, or paste from the clipboard (Ctrl+V / ⌘+V)
- **Accepted formats**: PNG, JPG, SVG, GIF, WebP
- **Built-in sample templates** (Rocket, Logo, Lightning) for testing without a file
- **Three fit modes**:
  - *Aspect Fit*: scales the whole image inside the square, keeping its proportions
  - *Square Crop*: center-crops the image to fill the square
  - *Maskable Safe*: adds at least 10% outer margin so the artwork stays inside Android's 80% safe circle
- **Inner padding slider** (0–40%)
- **Canvas background**: keep transparency, or fill with a custom color (presets: White, Dark, Indigo)
- **Live mockups**: browser tab (16×16), Apple touch icon, and Android adaptive icon with an optional safe-zone overlay
- **Manifest settings**: app name, short name, theme color, background color, tile color, display mode
- **Export options**: download everything as `favicons.zip`, save icons one by one, or copy the HTML snippet

## Usage

1. Open `favicon_generator_suite.html` in a modern browser. No build step or server is needed.
2. Upload an image, or pick one of the sample templates.
3. Adjust fit mode, padding and background.
4. Fill in the manifest metadata (name, colors, display mode).
5. Click **Generate Favicon Package**.
6. Download the ZIP, or use the **Generated Icons**, **HTML `<head>` Code** and **Config Files** tabs to grab individual items.
7. Upload the `favicons/` folder to your site and paste the HTML snippet into your page `<head>`.

> **Tip:** For best results, start with a square image of at least 512×512 px (or an SVG).

## Generated files

| Category  | Files | Sizes |
|-----------|-------|-------|
| Android   | `android-icon-{size}.png` | 36, 48, 72, 96, 144, 192 |
| Apple     | `apple-icon.png`, `apple-icon-{size}.png`, `apple-icon-precomposed.png` | 57, 60, 72, 76, 114, 120, 144, 152, 180, 192 (`apple-icon.png` and `apple-icon-precomposed.png` are 192) |
| Standard  | `favicon-{size}.png` | 16, 32, 96 |
| Standard  | `favicon.ico` | 16, 32 and 48 embedded in one file |
| Microsoft | `ms-icon-310x310.png` | 310 |
| Config    | `manifest.json`, `browserconfig.xml` | n/a |

The ZIP places everything inside a `favicons/` folder.

## Integrating into your site

Copy the snippet from the **HTML `<head>` Code** tab. It links every icon plus the manifest and browserconfig, for example:

```html
<link rel="shortcut icon" type="image/x-icon" href="favicon.ico">
<link rel="icon" type="image/png" sizes="32x32" href="favicon-32x32.png">
<link rel="apple-touch-icon" sizes="180x180" href="apple-icon-180x180.png">
<meta name="msapplication-config" content="browserconfig.xml">
<link rel="manifest" href="manifest.json">
```

**Paths:** the generated snippet uses bare filenames (e.g. `href="favicon.ico"`), so it assumes the icons sit next to your HTML page. If you put them in a `favicons/` folder, prefix each path accordingly (`href="favicons/favicon.ico"`). Note that the generator page's own `<head>` already uses the `favicons/` prefix.

## Technical notes

- **Rendering**: each icon is drawn on an HTML5 `<canvas>` with high-quality smoothing and exported as PNG.
- **ICO file**: `favicon.ico` is built by hand (ICONDIR header, three ICONDIRENTRY records, PNG-compressed payloads), so it is a genuine multi-resolution ICO, not a renamed PNG.
- **manifest.json**: lists the six Android icons; the 192×192 icon is marked `purpose: "any maskable"`. Defaults to `display: standalone`.
- **browserconfig.xml**: references `ms-icon-310x310.png` and uses your chosen tile color.
- **Dependencies** (loaded from CDNs): Tailwind CSS, JSZip 3.10.1, Font Awesome 6.4.2. The page needs an internet connection the first time it loads, though your images still never leave the browser.

## Browser support

Any modern browser with Canvas, Blob and File API support (current Chrome, Edge, Firefox, Safari).

## Limitations

- The manifest only references the Android icon set. If you want 512×512 PWA icons for install prompts, add one separately.
- Output is a fixed set of sizes; there is no custom-size option.
- Non-square sources are letterboxed in *Aspect Fit* mode (or cropped in *Square Crop* mode); pre-crop your image if you need precise framing.
