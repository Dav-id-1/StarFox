# CLAUDE.md

Guidance for Claude Code (and other AI agents) working in this repository.

## What this is

**STARWING — Gyro Flight** is a single-file, browser-based 3D rail shooter
(a Star Fox–style "all-range" game). You tilt/roll your phone to fly, and tap
an on-screen button to fire. It runs entirely client-side with no build step.

## Layout

- `docs/index.html` — **the entire game**: HTML, CSS, and JavaScript in one
  file. This is the single source of truth and the file GitHub Pages serves.
- `docs/.nojekyll` — disables Jekyll so Pages serves the static file as-is.
- `README.md` — project readme.

There is intentionally **no** duplicate copy of the game elsewhere. Edit the
game only in `docs/index.html`.

## Tech

- [three.js](https://threejs.org/) r128, loaded from a CDN (`cdnjs`). The game
  therefore needs an internet connection to load, even though it is otherwise
  self-contained.
- Plain ES2017+ JavaScript, no framework, no bundler, no package manager.

## Hosting / deployment

- Hosted on **GitHub Pages** at https://dav-id-1.github.io/StarFox/
- Pages is configured as **Deploy from a branch → `main` → `/docs`**.
- Every push to `main` triggers the "pages build and deployment" Action.
  There is no custom workflow file; it's GitHub's built-in Pages builder.
- `.nojekyll` is required — without it Pages runs Jekyll and the build fails
  trying to compile a theme stylesheet the project does not have.

## How to run / test locally

It's a static file. Either open `docs/index.html` directly, or serve it:

```bash
python3 -m http.server -d docs 8000   # then visit http://localhost:8000/
```

Gyro/tilt steering only works on a real touch device served over **https**
(mobile browsers block motion sensors on `file://` and plain http). On
desktop the game falls back to keyboard (arrows/WASD + space).

To sanity-check the inline script after editing:

```bash
awk '/<script>/{f=1;next} /<\/script>/{f=0} f' docs/index.html > /tmp/game.js
node --check /tmp/game.js
```

## Controls / steering model (important)

Steering is derived from the **gravity vector** via the `devicemotion`
event (`accelerationIncludingGravity`), **not** from Euler `beta`/`gamma`
angles. This is deliberate: raw beta/gamma gimbal-lock when the phone is held
upright (the natural playing position), which makes one axis hyper-twitchy and
the other dead. The gravity-vector approach stays smooth and decoupled.

- Roll right/left → steer right/left (screen-right component of the up-vector).
- Tip forward → dive, tip back → climb (screen-normal `z` component).
- Tuning constants live near the top of the script: `GYRO_LP`, `GYRO_DEAD`,
  `GYRO_FULL`, `GYRO_EXPO`, `GYRO_RESP`. A live debug overlay (`#gyrodbg`)
  shows the current steer values on device.

If a tilt axis ever feels inverted, flip the sign of the corresponding
`gyroTarget` assignment in `onMotion()` — that's the only change needed.

## Conventions

- Keep the game a single self-contained file. Don't introduce a build step,
  framework, or package manager without being asked.
- Match the existing code style (2-space indent, terse comments).
- Branch/commit/push per the user's instructions; don't open PRs unless asked.
