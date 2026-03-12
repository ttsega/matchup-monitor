# Matchup Monitor — Tournament Ops

Internal risk analyst tool for monitoring college basketball tournament matchups and secondary market ticket pricing in real time.

## What It Does

- Monitors **Big Ten Men's, ACC, Big 12, and SEC** tournament brackets (March 13–15, 2026)
- Automatically detects when **TBD matchups are confirmed** and fires instant alerts
- Pulls **live secondary market ticket pricing** from StubHub and SeatGeek including get-in price, average listing, listings count, and face value
- Tracks **price movement** vs last scan and vs session open baseline
- Generates an **aggression signal** per game (GET AGGRESSIVE / LEAN IN / HOLD / WATCH / BACK OFF) based on price momentum and inventory levels
- Flags **price spikes** above a configurable threshold (default 8%)
- Allows analysts to manually **flag high-risk games**
- Full **activity log** with timestamped scan history

## Access

**URL:** https://ttsega.github.io/matchup-monitor/

**Password:** Distributed internally — contact the ops team.

## How To Use

1. Open the URL and enter the access code
2. Click **START MONITOR** to begin automatic polling
3. Set your preferred scan interval (2 / 5 / 10 / 15 min) and spike alert threshold
4. Use **MANUAL SCAN** anytime for an on-demand pull
5. When a TBD resolves, a green **NEW MATCHUP** alert fires at the top
6. When a price moves beyond the spike threshold, an amber or red alert fires
7. Use the **⚑ flag** button on any game card to mark it as high risk
8. Click **LOG** (top right) to view full activity history

## Architecture

| Component | Service | Purpose |
|---|---|---|
| Frontend | GitHub Pages | Hosts the HTML dashboard |
| API Proxy | Cloudflare Workers | Proxies requests to Anthropic API, handles CORS |
| AI / Search | Anthropic Claude + Web Search | Fetches live matchup and ticket pricing data |

## Updating the Tool

1. Edit `matchup-monitor.html` locally
2. Rename to `index.html`
3. Upload to this repo via **Add file → Upload files**
4. Commit to `main` — GitHub Pages redeploys automatically in ~60 seconds

## Security Notes

- API key is stored as an encrypted secret in Cloudflare Workers — never in the source code
- Access to the dashboard is protected by a password gate
- Rotate the Anthropic API key at **console.anthropic.com** after each tournament weekend

## Tournaments Tracked

| Tournament | Venue | Dates |
|---|---|---|
| Big Ten Men's | Gainbridge Fieldhouse, Indianapolis | Fri 3/13 – Sun 3/15 |
| ACC Men's | Barclays Center, Brooklyn | Fri 3/13 – Sun 3/15 |
| Big 12 Men's | T-Mobile Center, Kansas City | Fri 3/13 – Sun 3/15 |
| SEC Men's | Bridgestone Arena, Nashville | Fri 3/13 – Sun 3/15 |
