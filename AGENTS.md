# AGENTS.md — Minecraft AFK Bot

## Overview
Node.js Minecraft AFK bot (mineflayer) with an Express web dashboard. The dashboard is server-rendered HTML (no frontend build step). Config lives in `settings.json`.

## Running
- `docker compose -f docker-compose.base44.yml up -d` starts everything.
- The Express server listens on port 3000 (`PORT=3000` set in compose).
- Nodemon watches for file changes and restarts automatically.
- No external secrets required — all config is in `settings.json`.

## Key files
- `index.js` — main app: Express server + mineflayer bot logic + all routes.
- `settings.json` — bot config (server IP/port, account, movement, modules).
- `logger.js` — in-memory log ring buffer (300 entries) used by dashboard.

## Behavior notes
- The bot tries to connect to the Aternos server from `settings.json`. If the server is offline, the dashboard shows "Disconnected" and the bot retries with exponential backoff. This is normal.
- Discord webhook integration is disabled by default (`discord.enabled: false`).
- No `.env` file needed; `RENDER_EXTERNAL_URL` self-ping is skipped when unset.

## Routes
- `GET /` — dashboard (status, uptime, coords, start/stop controls)
- `GET /health` — JSON status endpoint
- `GET /logs` — live log viewer with console input
- `GET /tutorial` — setup guide
- `POST /start`, `POST /stop` — bot controls
- `POST /command` — send chat/commands to the bot
