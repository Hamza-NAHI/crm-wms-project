# Deployment preparation verification

Verified on 24 October 2026 against the existing CRM WMS source.

| Check | Result |
| --- | --- |
| Codebase and Git audit | React/Vite/TypeScript frontend; no backend, API, database, required application environment variables, or pre-existing uncommitted changes. Existing local history preserved. |
| `npm ci --no-audit --no-fund` | Passed; 158 packages installed from the existing lockfile. |
| `npm run typecheck` | Passed. |
| `npm test` | Passed all four engine tests: five-carton dispatch, allocation invariants, receiving/capacity, continuous movement. |
| `npm run build` | Passed; existing large JavaScript bundle warning remains. |
| Lint | Not configured; not run. |
| `npm ls --depth=0` | Passed; direct dependencies resolve. |
| `git diff --check` | Passed before commit. |
| `docker compose config --quiet` | Passed; one web service, loopback port 3000 to container 8080. |
| Docker build and startup | Blocked by host engine availability. `docker compose build` and `docker compose up --build -d` both fail before a build/container can start. |
| Docker container health/logs | Not verified. `docker compose ps` and `docker compose logs --tail=30` cannot contact the engine. |

Docker CLI 27.3.1 and Compose v2.29.7 are installed. Docker Desktop was started, but the selected `desktop-linux` context reports:

```text
open //./pipe/dockerDesktopLinuxEngine: The system cannot find the file specified.
```

This is a host prerequisite blocker, not evidence that the Docker image builds or fails. Start/repair Docker Desktop's Linux engine (including its WSL2/virtualization prerequisites where applicable), then rerun:

```bash
docker version
docker compose up --build -d
docker compose ps
docker compose exec web nginx -t
docker compose exec web wget -q -O - http://127.0.0.1:8080/healthz
docker compose logs --tail=100 web
```

Open http://localhost:3000/ and follow the README's browser smoke test. The Node 22/Nginx multi-stage image, non-root runtime, read-only filesystem, and shutdown/health behavior remain unverified until those checks run against an available engine.
