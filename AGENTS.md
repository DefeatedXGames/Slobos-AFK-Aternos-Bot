# AGENTS.md — Base44 Dev Environment

## Project Overview
Minecraft AFK bot (Node.js) that keeps an Aternos server online 24/7. Uses `mineflayer` to connect to the Minecraft server and `express` to serve a status dashboard.

## Stack
- **Runtime**: Node.js (node:22-slim in Docker)
- **Entry point**: `index.js` (`npm start` → `node index.js`)
- **Config**: `settings.json` (server IP, port, bot account, modules — no env vars needed)
- **Web dashboard**: Express on `PORT` env (defaults to 5000, set to 3000 in compose)
- **No database, no external credentials required**

## Running
```bash
docker compose -f docker-compose.base44.yml up -d
```
- App served on **port 3000**
- Source is bind-mounted; `node --watch` provides live reload on edits
- `npm install` runs on container startup (deps in a named volume)

## Key Endpoints
- `GET /` — Dashboard UI (status, controls, stats)
- `GET /health` — JSON bot status
- `GET /ping` — Health check (returns "pong")
- `GET /logs` — Live bot logs viewer
- `GET /tutorial` — Setup guide
- `POST /start`, `POST /stop` — Start/stop the bot
- `POST /command` — Send in-game chat/commands

## Notes
- The bot will show "disconnected" in the sandbox because it cannot reach the external Aternos Minecraft server. The dashboard itself is fully functional.
- All bot config (server address, credentials, behavior) lives in `settings.json`, not environment variables.
- `RENDER_EXTERNAL_URL` is optional (self-ping for Render.com hosting; disabled when unset).
