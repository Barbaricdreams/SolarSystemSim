# Solar System Simulator

An interactive 3D solar system simulation built with Three.js and custom GLSL shaders. Planets orbit the Sun with procedurally generated surfaces — no texture images needed.

## Features

- **Procedural planet surfaces** — each planet uses custom fragment shaders (noise, fbm, ellipsoid continent masks for Earth) to generate realistic appearances at runtime
- **Realistic orbital mechanics** — Kepler-like elliptical orbits with proper eccentricity and inclination, delta-time scaled for frame-rate independence
- **Interactive camera** — drag to orbit, scroll to zoom, click any body to fly to it and view detailed info
- **Saturn's rings** — rendered as a separate ring geometry with transparency
- **5-layer sun glow** — additive-blended gradient spheres + corona sprite + lens flares
- **Starfield** — 6,000 procedurally placed stars with temperature-based coloring
- **Info cards** — click any planet (or use the navigation menu) to see distance, orbital period, radius, and a fun fact
- **Toggle controls** — stars, labels, orbit rings, and a bright-mode hemisphere light
- **Speed control** — adjustable simulation speed from 0x to 5x with pause/play

## Controls

| Input | Action |
|---|---|
| Drag | Orbit camera |
| Scroll | Zoom in/out |
| Click planet | Fly to planet, open info card |
| Escape | Close info card or menu |
| ☰ menu | Navigate to any body |
| ⏸ / ▶ | Pause / resume simulation |
| Speed slider | Adjust simulation speed |

## Accessibility

- All interactive elements have `aria-label` and `aria-pressed` attributes
- Planet menu uses `role="menu"` with `role="menuitem"` children
- Keyboard navigation: Escape closes cards and menus, focus is managed automatically
- Respects `prefers-reduced-motion` — decorative animations (sun pulse, glow breathing, flares) are disabled; orbital motion continues

## Tech Stack

- **Three.js 0.160** — 3D rendering via import maps (no build step)
- **Custom GLSL shaders** — simplex noise, fBm, ellipsoid continent mapping (Earth)
- **Vanilla JS** — zero dependencies beyond Three.js
- **HTML/CSS** — glassmorphism UI with backdrop-filter, responsive controls

## Running Locally

This is a static site with no build step. Serve it with any HTTP server (ES modules require one):

```bash
# Python
python3 -m http.server 8000

# Node.js
npx serve .

# PHP
php -S localhost:8000
```

Then open `http://localhost:8000` in your browser.

> **Note:** Opening `index.html` directly via `file://` won't work due to ES module CORS restrictions. Use a local server.

## Deployment

Hosted on [GitHub Pages](https://barbaricdreams.github.io/SolarSystemSim/). Push to `main` and Pages deploys automatically from the repo root.

## Project Structure

```
index.html          — Single-file app: HTML, CSS, and all JS/shaders inline
README.md           — This file
```

## Performance Notes

- Fragment shaders include `precision mediump float` for mobile GPU compatibility
- Shared `BufferGeometry` across all planets (created once, scaled per-body)
- Resize events are debounced (120ms) to prevent layout thrashing
- Delta-time accumulation ensures smooth animation regardless of frame rate
- WebGL availability is checked at startup with a fallback message
