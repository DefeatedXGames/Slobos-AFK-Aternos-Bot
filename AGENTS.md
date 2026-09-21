# AGENTS.md — Base44 Dev Notes

## Project Overview
Node.js Minecraft AFK bot (Mineflayer) with an Express web dashboard. The bot keeps an Aternos server alive by auto-joining; the Express server serves a status dashboard and control API.

## Stack
- **Runtime**: Node.js (use `node:22` base image)
- **Dependencies**: express, mineflayer, mineflayer-pathfinder, minecraft-data
- **Entry point**: `index.js` (`npm start`)
- **Config**: `settings.json` (bot account, server address, anti-AFK options)

## Running in Base44
- Compose: `docker-compose.base44.yml` — single `bot` service, bind-mounted source, `npm install && node index.js` at startup.
- Web dashboard served on **port 3000** (via `PORT=3000` env var; default is 5000).
- No external secrets required — bot runs in offline mode. No `.env` needed.
- Health check: `GET /health` returns JSON bot status. Dashboard at `GET /`.
- The bot will attempt to connect to the Minecraft server in `settings.json`; connection failures (server offline, duplicate login) are expected and don't affect the dashboard.

## Key Endpoints
- `GET /` — Dashboard UI
- `GET /health` — JSON bot status
- `GET /logs` — Recent bot logs
- `POST /start` / `POST /stop` — Control the bot
- `GET /tutorial` — Setup guide page

## Notes
- The bot username in `settings.json` may collide with an existing player on the target server, causing "duplicate_login" kicks. This is a config issue, not a code bug.
- Self-ping (Render.com keep-alive) is disabled when `RENDER_EXTERNAL_URL` is not set — expected in local/dev.
