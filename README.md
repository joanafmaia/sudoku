# Sudoku Discord Bot

Interactive 9×9 Sudoku for Discord — solo, daily, and multiplayer speedrun challenges.

## Features

- **`/play`** — solo puzzle with difficulty tiers
- **`/daily`** — same difficulty each day, unique puzzle per player (anti-copy; weekday schedule)
- **`/challenge`** — private speedrun (invite players or open Join lobby, 2–5 players)
- Paper & Pencil UI (board image + button stages)
- Sponges 🧽 economy, shop (titles), leaderboards
- Optional MongoDB for challenges / daily claims / session restore
- HTTP `/health` endpoint for free hosting keep-alive (Render + UptimeRobot)

## Setup (local)

1. Create a Discord application + bot at [Discord Developer Portal](https://discord.com/developers/applications)
2. Invite the bot with permissions: Send Messages, Embed Links, Attach Files, Read Message History, Create Private Threads, Send Messages in Threads, Manage Threads, Use Application Commands
3. Enable **Server Members Intent** (recommended for challenges)
4. Clone and install:

```bash
pip install -r requirements.txt
cp .env.example .env
# edit .env with your DISCORD_TOKEN (and optional Mongo)
python bot.py
```

## Deploy on Render (free) + UptimeRobot

### 1. Render — Web Service

1. Go to [Render](https://render.com) → **New** → **Web Service**
2. Connect the GitHub repo `joanafmaia/sudoku`
3. Settings:
   - **Runtime:** Python
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `python bot.py`
4. **Environment** (Environment Variables) — add the same keys as `.env`:
   - `DISCORD_TOKEN` (required)
   - `MONGODB_URI` (recommended)
   - `MONGODB_DB` (e.g. `sudoku`)
5. Deploy and copy your public URL, e.g. `https://sudoku-xxxx.onrender.com`

The bot serves `GET /` and `GET /health` (returns `ok`) so the free tier can be kept awake.

### 2. UptimeRobot

1. Go to [UptimeRobot](https://uptimerobot.com) → Add New Monitor
2. **Monitor Type:** HTTP(s)
3. **URL:** `https://YOUR-SERVICE.onrender.com/health`
4. **Interval:** every 5 minutes
5. Save

UptimeRobot will ping the health URL so Render is less likely to sleep.

> Free hosting can still restart or briefly go offline. For rock-solid 24/7, use a small VPS (e.g. Oracle Always Free).

## Environment

| Variable | Required | Description |
|---|---|---|
| `DISCORD_TOKEN` | yes | Bot token |
| `MONGODB_URI` | no | Atlas/local URI (in-memory fallback if unset) |
| `MONGODB_DB` | no | Database name (default `sudoku`) |
| `PORT` | no | HTTP port (Render sets this automatically) |

Never commit `.env` — it is gitignored.

## Commands

`/help` · `/play` · `/daily` · `/challenge` · `/shop` · `/quit` · `/leaderboard` · `/stats` · `/dailyboard`
