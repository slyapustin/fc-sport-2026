# FC Sport 2026

3D football game — single HTML file, Three.js, zero build step.

## Architecture

- Everything lives in `index.html` — HTML, CSS, JS, game logic. Do NOT split into separate files.
- Three.js r128 loaded from CDN (`cdnjs.cloudflare.com`). No npm, no bundler.
- Screens: Menu → Team Select → Game (3D match) → End. Toggled via `.screen.active` class.
- Game state in global `G` object. Persisted to `localStorage` key `fc26`.
- `CLAUDE.md` is a copy of this file (not a symlink — Jekyll breaks on symlinks).

## Code style

- Compact/minified CSS — single-line rules, short class names
- JS uses `var`-free style (let/const), short variable names in game loop for performance
- All user-facing text is in Russian (`lang="ru"`)
- Touch-first controls with keyboard and PS5 gamepad fallback

## Key systems

- **Input**: Virtual joystick (left) + action buttons (right), keyboard WASD+QE+Tab+Esc, DualSense gamepad
- **Player switching**: Tab key / L3 gamepad — switches to nearest teammate to ball
- **Pause**: Esc key / Options gamepad — overlay with resume/menu
- **AI**: `runAI()` — opponent and teammate positioning/passing/shooting
- **3D rendering**: `init3D()` → `draw()` loop. Scene: field, goals, stadium with fans, ad boards
- **Ball physics**: `ball.vx/vy` for ground movement, `ball.vz/z` for vertical arc (gravity = 0.4/frame)
- **Kit clash**: `resolveKitClash()` detects similar jersey colors (distance < 120) and assigns contrasting away kit
- **Audio**: Web Audio API for crowd sounds, SpeechSynthesis for Russian commentary
- **Cards**: FIFA-style player cards drawn on `<canvas>`, shop/collection system

## 3D scene structure

- **Players**: `createPlayerMesh()` returns group with body, head, hair, eyes, neck, shorts, thighs, legs (socks), boots, arms, hands, jersey stripe. Colors set per-frame in `draw()` via material refs.
- **Fans**: `makeFan()` in `buildStadium()`. Y position MUST be top of stand surface (y=4+row*4 for main stands). Fans use `MeshStandardMaterial` with `emissiveIntensity` for visibility.
- **Ad boards**: `adBoards` array — MUST be global (was `let` inside `buildStadium` causing black screen bug). Animated via canvas texture in `draw()`.
- **Animation**: Player meshes use position lerp (0.25) with snap-if-far (dist²>100) for smooth movement. Rotation lerps via angular interpolation.

## Testing

No test framework. Open `index.html` in browser and play. Check:
- Touch controls work on mobile/tablet (iPad especially)
- Keyboard + gamepad on desktop
- No console errors
- Teams with similar colors get distinct kits (e.g. Лацио vs Ман Сити)

## Deployment

- GitHub Pages: https://slyapustin.com/fc-sport-2026/
- Push to `main` auto-deploys (`.nojekyll` file skips Jekyll build)
- Repo: https://github.com/slyapustin/fc-sport-2026

## Common pitfalls

- IMPORTANT: Do not add build tools, bundlers, or package managers
- IMPORTANT: Do not split into multiple files — everything stays in `index.html`
- IMPORTANT: Do not use symlinks — GitHub Pages Jekyll breaks on them
- IMPORTANT: Do not use `position:fixed` on body — breaks Three.js container dimensions
- Two `resize()` functions exist — the second one (Three.js) overrides the first (legacy 2D, dead code)
- iOS Safari needs `100dvh` not `100vh`, and `overscroll-behavior:none` for bounce prevention
- Audio requires user interaction to initialize (`initAudio()` called on first tap/click)
- Variables used in `draw()` MUST be global scope — `let` inside `buildStadium()` etc. is not visible
- Fan Y positions must sit ON TOP of stand blocks, not inside them (stand top = center_y + height/2)
- Player mesh lerp must snap (not interpolate) when distance > threshold, to avoid sliding from origin on reset
- `ball.z` and `ball.vz` must reset to 0 in `giveBall()` to prevent floating ball after pickup
