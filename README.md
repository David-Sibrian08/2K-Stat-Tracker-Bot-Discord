# GTD 2K Tracker (Discord Bot)

A private Discord bot for tracking NBA 2K games, stats, and leaderboards for our group.

The bot supports:
- Creating and managing games (DRAFT → FINAL)
- Manual stat entry
- Game summaries with receipts
- Player summaries, teammate / opponent splits
- Leaderboards ranked by Fantasy Points per Game (FP/G)
- Screenshot uploads via OCR (**work in progress**)

---

## Core Concepts

### Game Lifecycle
- **DRAFT**
  - Editable
  - Stats can be added or replaced
  - Score can be updated
- **FINAL**
  - Locked
  - Used for leaderboard calculations
  - Stats cannot be modified unless unfinalized

---

## Commands

### General
- `/ping` — check if the bot is alive

---

### Player Commands
- `/player add gamertag:<tag>`
- `/player summary gamertag:<tag>`
- `/player with gamertag:<tag> teammate:<tag>`
- `/player vs gamertag:<tag> opponent:<tag>`

---

### Game Commands
- `/game create`  
  Creates a new **DRAFT** game (manual entry flow)

- `/game ocr image:<attachment>`  
  Creates a **DRAFT** game, saves the screenshot as a receipt, and attempts OCR  
  ⚠️ OCR accuracy is **not guaranteed** (see OCR section below)

- `/game score game_id:<id> team_a:<int> team_b:<int>`

- `/game summary game_id:<id>`

- `/game list [status:DRAFT|FINAL] [limit:1–25]`

- `/game find gamertag:<tag> [limit:1–25]`

- `/game finalize game_id:<id>`

- `/game unfinalize game_id:<id>`

---

### Stat Commands
- `/stat add game_id:<id> gamertag:<tag> team:<A|B>`
  - `pts`, `reb`, `ast`, `stl`, `blk`, `fouls`, `to`
  - `fgm`, `fga`, `tpm`, `tpa`, `ftm`, `fta`

Re-adding stats for the same player and game **replaces** the existing line.

---

## Setup

### Requirements
- Java 17+ recommended
- Discord Bot Token
- SQLite (handled automatically)

---

### Environment Variables

Set these before running the bot:

- `DISCORD_TOKEN` — Discord bot token
- `DISCORD_GUILD_ID` — Guild (server) ID where commands are registered
- `ADMIN_USER_ID` — Discord user ID allowed to run write commands
- `TRACKER_CHANNEL_ID` — Channel where write commands are allowed

> If `ADMIN_USER_ID` or `TRACKER_CHANNEL_ID` are not set, write commands are allowed everywhere (not recommended).

---

## Data Storage

- SQLite database:
  data/playnow.db
- Saved images / receipts:
  data/images/

  ---

## OCR Status (Work in Progress)

The OCR system is **experimental** and not production-reliable yet.

Current limitations:
- Shooting splits like `1/11`, `4/5`, `0/1` can be misread
- Small 2K icons near gamertags introduce noise
- An `@` symbol may appear before gamertags — this is a **2K overlay artifact**, not part of the real tag
- OCR may detect only some players in a game

### Recommended Workflow
1. Use `/game ocr` to save the screenshot as a **receipt**
2. Review results with `/game summary`
3. Manually fix or add stats using `/stat add`
4. Finalize once verified

---

## Leaderboard Rules

- Only **FINAL** games are included
- Minimum games required per player (currently hardcoded)
- Ranked by **Fantasy Points per Game (FP/G)**
- W/L is calculated from recorded team scores

---

## Notes & Known Behavior

- Game summaries may feel slow if a receipt image is attached (Discord upload latency)
- If a summary times out, retry on a stable connection
- `/game list` intentionally omits player names to avoid clutter

---

## Roadmap Ideas

- Improve OCR accuracy and stat pairing
- Add receipt attachment support for manually created games
- Admin audit tools for W/L mismatches
- OCR confidence reporting per player row

---

## Disclaimer

This bot is built for **personal/private use** and assumes trusted users.
No validation is performed beyond basic sanity checks.

---
