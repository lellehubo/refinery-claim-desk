# The Claim Desk

A one-page tool, built live with Claude Code, that shows how a drone strike on Russian oil infrastructure is described by different sources: who said it first, who repeated whom, and which words they chose.

**Live page:** `index.html` (built on stage) · **Reference version:** `fallback/index.html`

## What is in here
- `CLAUDE.md`: the newsroom rules the AI must follow. **This is the part worth copying.** Change the rules and you change what the tool calls corroborated.
- `data/`: pre-built, public data.
  - `facilities.json`: 61 sites (refineries, terminals, depots, pumping stations) with coordinates and capacity.
  - `events.json`: strikes 2022 to October 2026. Detail from 21 Apr 2026; older ones are background only.
  - `claims.json`: about 250 public posts from Telegram, X and news sites, with time, sender type and short quotes.
  - `firms.json`: NASA FIRMS satellite heat anomalies (VIIRS, 375 m), if present.
- `fallback/`: the finished reference version.

## How the status works
The tool counts **independent origins, not posts**. Thirty channels repeating one governor are one origin. Status is one of:
Both sides · Corroborated · Single origin · Tracker only · Target unclear. The exact rules are in `CLAUDE.md`.

Every row is a claim, not a fact.

## Limits you should know
- Posts come from a Meltwater sample (1 Jun to 21 Oct 2026, max 100 documents per query). It is a selection, not everything.
- Russian governors' own channels and the Max messenger are not indexed. Their words appear only as relayed by others.
- Matching posts to strikes is mechanical for events without a full trail. Featured events (●) were checked by hand.
- Translations were made by Claude and are labelled. Quotes are shortened to about 15 words.
- A satellite heat anomaly is consistent with fire. It is not proof of a strike, and no anomaly does not mean nothing happened.

## Fork it
Replace `data/` with your own beat and rewrite `CLAUDE.md` for your newsroom. Run it with any static server, for example `python -m http.server`, because browsers block `fetch` from `file://`.

## Before you do: source protection
Decide what must **never** enter a cloud AI service. Public statements by officials and public channels are one thing.
Witnesses, sources, anyone inside occupied territory or inside Russia, unpublished material and anything that could identify a person are another.
Keep that material out of prompts, uploads and connected tools, and write the rule into your own `CLAUDE.md`.
