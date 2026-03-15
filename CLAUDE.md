# FC Sport 2026

Single-file 3D football game built with Three.js.

## Architecture

- **Single file**: Everything lives in `index.html` — HTML, CSS, JS, and game logic
- **Three.js r128** loaded from CDN — no build step, no bundler
- **Screens**: Menu → Team Select → Game → Result, toggled via `.screen.active` class

## Key conventions

- Keep everything in `index.html` — do not split into separate files
- No build tools or package managers — the game runs by opening the file directly
- Touch-first controls with keyboard fallback
- Russian locale (`lang="ru"`)

## Deployment

- Hosted on GitHub Pages at https://slyapustin.com/fc-sport-2026/
- Pushes to `main` auto-deploy
