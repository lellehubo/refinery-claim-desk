# The Claim Desk: newsroom rules

This page shows how one drone strike is described by different sources: who said it, when, and in what words.
**The AI sorts and labels. It never decides what is true.**

## Editorial rules (do not break these)
1. **Every row is a claim, not a fact.** Never write "confirmed", "verified" or "destroyed" in the interface.
2. **Count origins, not posts.** Ten channels repeating one governor are one source: the governor.
   A post's *origin* is `claim.cites` if set, otherwise `claim.sender_type`.
   If `sender_type` is `ru_official` and `cites` is empty, the origin is `ru_channel` (a Russian channel speaking for itself).
3. **Independent origin groups:** `ua_official`, `russia` (= `ru_official` + `ru_channel` together), `monitoring`, `independent_media`, `industry`.
   Ignore claims with `uncertain_match: true` unless they are the only claims for the event.
4. **Status, in this order:**
   - facility `type` is `unidentified` → **Target unclear**
   - origins include both `ua_official` and `ru_official` → **Both sides**
   - two or more origin groups → **Corroborated**
   - one origin group → **Single origin**
   - no claims at all (only `secondary_sources`) → **Tracker only**
5. **Labels that always show:** facility type other than `refinery` or `unidentified` → "Not a refinery". `coord_quality` is `region` → "Location approximate". `indistinguishable_from` set → "Cannot tell from neighbour".
   `date_doubtful` → "Date doubtful". An `industry` claim → "Outage reported".
6. **Timing:** only posts with `time_reliable: true` (Telegram, X) set the start time. Show news-site times, but mark them "time approx.".
7. **Translations** in `quote_en` were made by Claude. Always label them "Translation". If `quote_en` is null the quote is already English.
8. **Satellite heat** (`data/firms.json`) is written as "Satellite heat anomaly consistent with fire". It is never proof and never changes the status.
9. Never invent a source, a time, a quote or a number. If a field is missing, show nothing.

## Data (read-only, do not edit)
- `data/facilities.json` → `facilities[]`: `id, name, type, region, lat, lon, coord_quality, capacity_mt_yr, control_group, indistinguishable_from`
- `data/events.json` → `events[]`: `id, date, facility_id, featured, period ("recent" | "earlier"), date_doubtful, secondary_sources[{name,url,partisan,damage}], notes[]`
- `data/claims.json` → `claims[]`: `id, event_id, time_utc, time_reliable, platform, sender, sender_type, cites, url, quote, quote_en, lens_terms[]`
- `data/firms.json` → `anomalies[]`: `facility_id, date, flag, max_frp_mw, z, days, note` (may be missing)

`sender_type` values and display names: `ua_official` Ukrainian official · `ru_official` Russian official (relayed) · `ru_channel` Russian channel ·
`monitoring` Monitoring channel · `independent_media` Independent media · `industry` Reuters / industry sources.

## Design
- One self-contained `index.html` (inline CSS and JS). Load JSON with `fetch('data/...')`. English only.
- Dark first, readable on a projector: background `#0b1220`, panels `#121a2b`, text `#e8edf5`, muted `#9aa7bd`. Font: Jost from Google Fonts. Base size 17px.
- No red and no grey-red. Status colours: Both sides `#2dd4bf` · Corroborated `#60a5fa` · Single origin `#fbbf24` · Tracker only `#a78bfa` · Target unclear `#f472b6`.
- Map: Leaflet 1.9.4 from unpkg, CARTO dark tiles. **The list must still work if the map fails to load.**
- "recent" events are bright circles sized by number of claims. "earlier" events are tiny faint dots in the background, hidden from the list, toggled by "Show earlier strikes".
- Header: "The Claim Desk", subtitle "Strikes on Russian oil infrastructure, 21 Apr to 21 Oct 2026 · Built live with Claude Code · Public sources only".
- Footer: "Every row is a claim, not a fact. Status reflects independent origins, not volume."
- No em dashes anywhere.

## Working style during the demo
- Do only the step asked for. Keep everything in `index.html`. Do not touch `data/` or `fallback/`.
- Do not start servers or open browsers. Save the file and say in two lines what changed.
- If the data is ambiguous, say so in one line.
