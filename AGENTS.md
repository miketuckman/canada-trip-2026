# Agent instructions — Canada Trip 2026

Static HTML trip-presentation pages. Full context is in `README.md`; this file is the short operational contract for AI agents.

## Critical workflow

- **Hosting:** Cloudflare Workers (static assets) at `canada.tuckman.us`. **Do NOT re-enable GitHub Pages** — it was intentionally deleted on 2026-07-02 after repeated deploy failures (details in `README.md`).
- **Deploy = `git push` to `main`.** Cloudflare's connected Git build auto-runs `npx wrangler deploy`. No dashboard/manual step.
- **Main page source is `trip-presentation.html`.** After editing it, run `cp trip-presentation.html index.html` before committing — `/` serves `index.html`.
- Standalone pages: `full.html` → `/full`, `stjohns.html` → `/stjohns`.
- `itinerary-research.md` is the source of truth for plans/costs/sources — keep it in sync with the HTML.
- `.md` files and repo internals are excluded from the deployed site via `.assetsignore`; don't rely on them being publicly served.

## Style rules (Mike's preferences)

- **Direct, not travelogue** — dates, logistics, availability, cost. No "things to see" filler on the main page.
- **Costs are islands-only, per couple** — count only the Newfoundland / Saint-Pierre / Madeleine legs; exclude the Florida⇄Halifax flights, the Halifax rental car, and Halifax/PEI lodging.
- Match the existing aesthetic: **Fraunces + Inter** fonts, palette vars `--hfx / --stj / --spm / --pei / --idm / --gold`.

## Verify before claiming done

- Load pages headlessly (serve locally, e.g. `python3 -m http.server`) and check the switcher/map/ribbon render and the console is clean.
- After deploy, confirm live (public resolver or `--resolve` to a Cloudflare IP; the local LAN may lag on DNS — see README).
