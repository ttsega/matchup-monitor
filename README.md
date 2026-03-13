# Matchup Monitor — NCAA Tournament Signal Intelligence

Internal risk analyst tool for monitoring 2026 NCAA Men's Tournament matchups, secondary market ticket pricing, and prediction market signals in real time.

## What It Does

- Monitors all **67 games** across the 2026 NCAA Men's Tournament (First Four through Championship)
- Auto-populates the bracket from ESPN after **Selection Sunday (March 15)** and refreshes every 30 minutes on game days
- Pulls **live secondary market ticket pricing** from SeatGeek — get-in price, average listing, listing count, and face value
- Tracks **prediction market probabilities** from Kalshi for win-probability signals
- Generates a **composite aggression signal** per game (GET AGGRESSIVE / LEAN IN / HOLD / WATCH / BACK OFF) based on ticket price velocity, market probability deltas, and game context
- Applies **round-aware confidence thresholds** — higher standards for Sweet 16+ games
- Real-time updates via **WebSocket** — no manual refresh needed
- Full **activity log** with timestamped scan history and signal narratives

## Access

**URL:** https://ttsega.github.io/matchup-monitor/

**Password:** Distributed internally — contact the ops team.

## How To Use

1. Open the URL and enter the access code
2. Games auto-populate from ESPN after Selection Sunday — no manual setup needed
3. Use **POPULATE BRACKET** to force an immediate refresh from ESPN
4. Use **MANUAL SCAN** anytime for an on-demand data pull
5. Game cards update in real time via WebSocket as new scans complete
6. Signal badges show current aggression tier per game — color coded green → red
7. Tabs organize games by round (First Round, Sweet 16, Elite Eight, etc.)
8. Click **LOG** (top right) to view full activity history and signal narratives

## Signal Tiers

| Signal | Meaning |
|---|---|
| 🟢 GET AGGRESSIVE | Strong composite signal — act now |
| 🟩 LEAN IN | Positive signal — favorable conditions |
| ⬜ HOLD | Neutral — monitor but no action |
| 🟡 WATCH | Early signal — conditions developing |
| 🔴 BACK OFF | Negative signal — unfavorable conditions |

## Architecture

| Component | Service | Purpose |
|---|---|---|
| Frontend | GitHub Pages | Hosts the HTML dashboard |
| Backend API | Railway (Node.js + Express) | Signal engine, REST API, WebSocket |
| Database | Railway (PostgreSQL) | Game snapshots, signals, action log |
| Cache | Railway (Redis) | Rate limiting, queue |
| Ticket Data | SeatGeek API | Secondary market pricing by venue/date |
| Prediction Markets | Kalshi API | Win probability signals |
| Game Data | ESPN API | Bracket, matchups, live scores, TV info |
| AI Resolution | Anthropic Claude | Game identity matching, signal narration |

## Tournament Schedule

| Round | Dates | Venues |
|---|---|---|
| First Four | March 17–18 | Dayton, OH (UD Arena) |
| First Round | March 19–20 | Buffalo, Greenville, OKC, Portland, Tampa, Philadelphia, San Diego, St. Louis |
| Second Round | March 21–22 | Same as First Round |
| Sweet 16 | March 26–27 | Houston, San Jose, Chicago, Washington D.C. |
| Elite Eight | March 28–29 | Same as Sweet 16 |
| Final Four | April 4 & 6 | Indianapolis (Lucas Oil Stadium) |

## Signal Weights

```
Ticket velocity (SeatGeek):   45%
Prediction market (Kalshi):   35%
Game context (ESPN):          20%
```

## Round-Aware Confidence Thresholds

| Round | Min Confidence to Fire |
|---|---|
| First Four / First Round | 0.55 |
| Second Round | 0.60 |
| Sweet 16 / Elite Eight | 0.65 |
| Final Four / Championship | 0.70 |

## Security Notes

- All API keys stored as Railway environment variables — never in source code
- Dashboard protected by password gate
- Rotate Anthropic API key at **console.anthropic.com** after tournament ends
- Railway free tier: upgrade before March 17 if needed

## Updating the Dashboard

1. Edit `index.html` locally in this repo
2. `git add index.html && git commit -m "update" && git push origin main`
3. GitHub Pages redeploys automatically in ~60 seconds
