# Netbox

Local network stability monitor with a Python backend and Vue dashboard. Netbox runs configurable HTTP/S, TCP, ICMP, and DNS checks from your machine, persists history in SQLite, and helps separate local network issues from upstream/ISP problems.

## Quick start

```bash
make setup   # optional on first clone
make run
```

| Service | URL |
| --- | --- |
| Dashboard | http://localhost:5177 |
| API | http://localhost:4177 |

Backend options can be passed through `make run`:

```bash
make run ARGS='--wifi-name "Office Wi-Fi"'
```

## Commands

| Command | Description |
| --- | --- |
| `make setup` | Prepare folders, `.env`, deps, and seed data (optional) |
| `make run` | Dev dashboard + API |
| `make test` | Backend and frontend tests |
| `make lint` | Ruff and `vue-tsc` |
| `make build` | Build `frontend/dist` |
| `make clean` | Remove build output and test caches |
| `make desktop` | Electron dev shell |
| `make package` | Desktop installers (`.dmg`, `.exe`, AppImage) |

`make clean DATA=1` also deletes the local SQLite database.

## Layout

```text
backend/                 Python package, tests, and monitor entrypoint
backend/monitor.py       Run the backend locally
config/                  JSON configuration
frontend/                Vite + Vue dashboard
frontend/public/         Static assets served by Vite (NDT7 workers)
docs/                    API, architecture, and development notes
scripts/dev.py           Hot-reload dev stack runner
data/                    Local SQLite database (gitignored; created on first backend start)
```

## Configuration

Defaults live in `config/*.json`. Runtime values load from `.env` (local, gitignored), optional `.env.local`, then `.env.<NETBOX_ENV>` files.

| Path | Purpose |
| --- | --- |
| `config/backend.json` | Host, port, monitor thresholds, database path |
| `config/frontend.json` | App name and dev-server defaults |
| `config/targets.json` | Default target seeds for first startup |
| `config/speed.json` | NDT7 speed-test policy |
| `config/security.json` | Bind allow-list and security headers |
| `.env.example` | Committed template - first `make run` copies this to `.env` |
| `.env` | Local development overrides (gitignored) |
| `.env.production` | Production overrides when `NETBOX_ENV=production` (gitignored) |
| `.env.local` | Private secrets and machine-specific overrides (gitignored) |

Optional Pexels wallpaper support proxies nature images through the backend. Set `PEXELS_API_KEY` in `.env.local`. Never commit real API keys.

## Dashboard features

- **Configurable targets** - CRUD for HTTP/S, TCP, ICMP, and DNS checks
- **Live monitoring** - gateway and external checks with SSE updates
- **History and incidents** - persisted timeline, target breakdown, and paginated incident log
- **Speed tests** - user-initiated M-Lab NDT7 runs with policy limits
- **Theme** - light, dark, or system preference (stored in browser localStorage)
- **Wallpaper** - optional Pexels nature backgrounds with glass-style cards when enabled

UI preferences such as collapsed dashboard sections sync to SQLite via `/api/preferences`.

## Git and secrets

`.gitignore` excludes local env files, secrets, runtime data, build output, and tool caches. Safe to commit: source, `config/`, and `.env.example`. Keep `.env`, `.env.production`, and `.env.local` on your machine only.

Before your first push, run `git add -A && git status` and confirm no secrets or `data/` files are staged.

## Production

```bash
make build
.venv/bin/python backend/monitor.py --host 127.0.0.1 --port 4177
```

## Desktop app

Netbox can run as a tray-backed Electron app with a bundled Python backend (no cloud required).

```bash
make desktop    # local development shell
make package    # build installers
```

See [`docs/ELECTRON.md`](docs/ELECTRON.md) for packaging details and code-signing notes.

## Documentation

- [`docs/API.md`](docs/API.md) - REST and SSE endpoints
- [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) - system design and security
- [`docs/DEVELOPMENT.md`](docs/DEVELOPMENT.md) - workflow, layout, and standards

## Authorship

By [Marvelous Solomon](https://github.com/solomonmarvel97).
