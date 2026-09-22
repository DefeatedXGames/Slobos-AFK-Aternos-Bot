# AGENTS.md

## Project Overview
Minecraft AFK bot (Node.js) that keeps an Aternos server online 24/7. Uses mineflayer to connect to a Minecraft server and Express to serve a web dashboard.

## Setup
- **Runtime**: Node.js (no build step — plain `node index.js`)
- **Dependencies**: `npm install` at startup (express, mineflayer, mineflayer-pathfinder, minecraft-data)
- **Config**: All configuration lives in `settings.json` (server IP, port, bot credentials, movement/anti-AFK options). No `.env` or external secrets required.
- **Port**: Express listens on `process.env.PORT || 5000`. The Base44 compose sets `PORT=3000` and maps host `3000:3000`.

## Running
```
docker compose -f docker-compose.base44.yml up -d
```

## Verification
- `curl http://localhost:3000/` → dashboard HTML (HTTP 200)
- `curl http://localhost:3000/health` → JSON with bot status
- The bot will show "disconnected" in the sandbox (it cannot reach the Aternos Minecraft server), but the dashboard, logs, tutorial, and command endpoints all function normally.

## Notes
- No external secrets needed. Discord webhook is disabled by default. `RENDER_EXTERNAL_URL` is optional (self-ping only runs on Render.com).
- The bot continuously attempts to reconnect to the Minecraft server configured in `settings.json` — this is expected behavior, not an error.
- Live reload is not configured (no dev server watcher); use `reload_preview` after code changes, or restart the service: `docker compose -f docker-compose.base44.yml restart bot`.
