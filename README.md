# StarFox

**STARWING — Gyro Flight**: a single-file, browser-based 3D rail shooter.
Tilt/roll your phone to fly, tap to fire.

▶️ **Play:** https://dav-id-1.github.io/StarFox/

## Run locally

```bash
python3 -m http.server -d docs 8000
# then open http://localhost:8000/
```

The whole game is `docs/index.html`. Tilt steering needs a real phone served
over https; on desktop it falls back to keyboard (arrows/WASD + space).

See [CLAUDE.md](./CLAUDE.md) for developer/agent notes.
