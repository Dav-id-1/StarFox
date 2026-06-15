# AGENTS.md

This repository follows the [AGENTS.md](https://agentsmd.net/) convention for
giving AI coding agents project context.

The full, authoritative guidance lives in **[CLAUDE.md](./CLAUDE.md)** — please
read it. Quick summary:

- **What:** STARWING — a single-file, browser-based 3D tilt-to-fly rail shooter.
- **Source of truth:** `docs/index.html` (HTML + CSS + JS in one file). Edit the
  game only here; there is no duplicate copy.
- **Run:** `python3 -m http.server -d docs 8000`, then open
  http://localhost:8000/ . Tilt steering needs a real device over https; desktop
  falls back to keyboard.
- **Check JS after edits:**
  `awk '/<script>/{f=1;next} /<\/script>/{f=0} f' docs/index.html > /tmp/game.js && node --check /tmp/game.js`
- **Deploy:** GitHub Pages from `main` → `/docs`. `docs/.nojekyll` must stay.
- **Steering:** gravity-vector (`devicemotion`), not Euler beta/gamma — see
  CLAUDE.md for why and how to tune.
- **Don't:** add a build step, framework, or package manager unless asked; don't
  open PRs unless asked.
