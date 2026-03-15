# FC Sport 2026

3D football game — single HTML file, Three.js, zero build step.

## Architecture

- Everything lives in `index.html` — HTML, CSS, JS, game logic. Do NOT split into separate files.
- Three.js r128 loaded from CDN (`cdnjs.cloudflare.com`). No npm, no bundler.
- Screens: Menu → Team Select → Game (3D match) → End. Toggled via `.screen.active` class.
- Game state in global `G` object. Persisted to `localStorage` key `fc26`.

## Code style

- Compact/minified CSS — single-line rules, short class names
- JS uses `var`-free style (let/const), short variable names in game loop for performance
- All user-facing text is in Russian (`lang="ru"`)
- Touch-first controls with keyboard and PS5 gamepad fallback

## Key systems

- **Input**: Virtual joystick (left) + action buttons (right), keyboard WASD+QE, DualSense gamepad
- **AI**: `runAI()` — opponent and teammate positioning/passing/shooting
- **3D rendering**: `init3D()` → `draw()` loop. Scene has field, goals, stadium with fans, ad boards
- **Audio**: Web Audio API for crowd sounds, SpeechSynthesis for Russian commentary
- **Cards**: FIFA-style player cards drawn on `<canvas>`, shop/collection system

## Testing

No test framework. Open `index.html` in browser and play. Check:
- Touch controls work on mobile/tablet (iPad especially)
- Keyboard + gamepad on desktop
- No console errors

## Deployment

- GitHub Pages: https://slyapustin.com/fc-sport-2026/
- Push to `main` auto-deploys
- Repo: https://github.com/slyapustin/fc-sport-2026

## Common pitfalls

- IMPORTANT: Do not add build tools, bundlers, or package managers
- IMPORTANT: Do not split into multiple files — everything stays in `index.html`
- Two `resize()` functions exist — the second one (Three.js) overrides the first (legacy 2D, dead code)
- iOS Safari needs `100dvh` not `100vh`, and `position:fixed` on body to avoid address bar issues
- Audio requires user interaction to initialize (`initAudio()` called on first tap/click)
