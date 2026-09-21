# AGENTS.md

## Overview
Minecraft AFK bot (mineflayer) with an Express web dashboard. The bot connects to an Aternos server configured in `settings.json` and serves a status dashboard via Express.

## Running
- `docker compose -f docker-compose.base44.yml up -d` brings up the app on port 3000.
- The Express server listens on `process.env.PORT || 5000`; compose sets `PORT=3000`.
- Dependencies install automatically on container startup (`npm install`).
- Nodemon watches `.js`/`.json` files for live reload.

## Key files
- `index.js` — main entry: Express server + bot logic (all inline, ~65k chars).
- `settings.json` — bot config (server IP/port, auth, movement, modules). Edit this to change bot behavior.
- `logger.js` — in-memory log ring buffer (300 entries) used by the `/logs` page.

## No external secrets required
- Bot uses offline (cracked) auth — no Microsoft/Mojang credentials.
- Discord webhook is disabled by default in `settings.json`.
- No database or other infrastructure services needed.

## Notes
- The bot will attempt to connect to the Minecraft server on startup. If the server is offline, the bot retries with exponential backoff — the web dashboard still works regardless.
- The dashboard is at `/`, logs at `/logs`, setup guide at `/tutorial`, health JSON at `/health`.
