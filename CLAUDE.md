# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static single-page website for **Punto Remeras**, a wholesale clothing manufacturer based in Mar del Plata, Argentina. Hosted on GitHub Pages. No build step, no framework, no package manager — everything is plain HTML/CSS/JS in `index.html`.

## Development

Open `index.html` directly in a browser, or serve it locally:

```bash
python3 -m http.server 8080
# then visit http://localhost:8080
```

There are no tests, linters, or build commands.

## Deployment

Push to the branch being used by GitHub Pages (typically `main`). Changes on `claude/dreamy-clarke-EIrRI` must be merged to that branch to appear live.

## Architecture

Everything lives in `index.html` — CSS in `<style>`, markup in `<body>`, JavaScript in `<script>` at the bottom. Product images are in `img/`.

**Key CSS variables** (`:root`): `--primary: #1B3A6B`, `--accent: #F97316`. All spacing, shadows, and colours use these variables.

**Page sections** (in order): Navbar → Hero → Clients bar → Estampado → Products tabs → Configurador → Pricing table → Quiénes → Cómo funciona → Contact form → Footer.

**Products tab system**: Three tabs (`remeras`, `buzos`, `pantalones`) controlled by `switchTab(name, el)`. Tab panels use `id="panel-{name}"` with class `tab-panel active`.

**"Armá tu diseño" configurator** — 7-step wizard:
- State lives in `cfgState` object (`step`, `prenda`, `color`, `colorName`, `details`, `estampado`, `posicion`, `cantidad`, `talles`, `logoUploaded`).
- Navigation: `cfgNext()` / `cfgBack()`.
- Step 3 details are built dynamically by `buildDetailStep(prenda)` using the `detailDefs` object — add/remove detail options there.
- SVG garment preview (`#cfgSvg`, viewBox `0 0 200 240`) updates live. Colour changes go through `.g-fill { fill: ... }` via `document.querySelectorAll('.g-fill')`. Garment groups: `#gRemera`, `#gBuzo`, `#gPantalon`. Collar variants: `#gRRound` / `#gRVneck`. Detail visibility is toggled by `refreshSvgDetails()` which calls `svgShow(id, bool)`.
- SVG uses `<defs>` gradients (`gSide`, `gHL`, `gBtm`, `gHoodHL`, `gPL`, `gPR`) and a `feDropShadow` filter (`fDrop`) for the 3D fabric effect. Each garment shape has three stacked `<path>` overlays (side shadow, centre highlight, bottom shadow) using the same `d` as the base path.
- Logo upload: `handleLogo(e)` reads the file with `FileReader`, sets `href` on `#cfgLogoImg`, hides `.logo-r` / `.logo-t` placeholders.
- Final step builds a WhatsApp URL via `wa.me/5492234224837` with a pre-filled encoded message.

**Contact / WhatsApp number**: `5492234224837` — appears in several `wa.me` links throughout the file.
