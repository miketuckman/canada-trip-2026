# Agent instructions — Canada Trip 2026

Static HTML trip-presentation pages. Full context is in `README.md`; this file is the short operational contract for AI agents.

## Critical workflow

- **Hosting:** Cloudflare Workers (static assets) at `canada.tuckman.us`. **Do NOT re-enable GitHub Pages** — it was intentionally deleted on 2026-07-02 after repeated deploy failures (details in `README.md`).
- **Deploy = `git push` to `main`.** Cloudflare's connected Git build auto-runs `npx wrangler deploy`. No dashboard/manual step.
- **`/` = `index.html` = the landing page** — a directory of 5 full-width plan boxes; each links to a standalone plan page. Edit `index.html` directly (it is NO LONGER a copy of `trip-presentation.html`).
- **One standalone page per plan** (each self-contained: hero, sticky nav, Leaflet map, split-timeline ribbon, chapters, islands-only cost). Plans are lettered A–E by landing order:
  - `stjohns.html` → `/stjohns` = **Plan A** (St. John's by air)
  - `saint-pierre-townhouse.html` → `/saint-pierre-townhouse` = **Plan B** (booked townhouse)
  - `saint-pierre-sunday.html` → `/saint-pierre-sunday` = **Plan C** (Sunday window)
  - `madeleines.html` → `/madeleines` = **Plan D** (Îles de la Madeleine)
  - `big-loop.html` → `/big-loop` = **Plan E** (France → Newfoundland → PEI)
- **Sticky-nav pattern** (all plan pages): a `.nav-sentinel` + IntersectionObserver toggles `nav.stuck`, which reveals the `.nav-title` (plan name) only when pinned; a `.backpill` (tinted to the plan `--accent`) links back to `/`; chip-styled links + a "Jump to:" hint. Keep this consistent when adding pages.
- **Split-timeline travel days:** in each page's `days[]`, a `from:` key marks a travel day → the ribbon cell renders a diagonal `linear-gradient` split (morning place → night place).
- `full.html` → `/full` = archived all-in-one switcher (old A–D labels; kept as backup). `trip-presentation.html` is the legacy switcher source behind `/full`-style content — **not** copied to `index.html` anymore.
- `itinerary-research.md` is the source of truth for plans/costs/sources (still uses the old A–D letters — new landing order is A–E; St.John's=A, townhouse=B, Sunday=C, Madeleines=D, BigLoop=E). Keep it in sync with the HTML.
- `.md` files and repo internals are excluded from the deployed site via `.assetsignore`; don't rely on them being publicly served.

## Style rules (Mike's preferences)

- **Direct, not travelogue** — dates, logistics, availability, cost. No "things to see" filler on the main page.
- **Costs are islands-only, per couple** — count only the Newfoundland / Saint-Pierre / Madeleine legs; exclude the Florida⇄Halifax flights, the Halifax rental car, and Halifax/PEI lodging.
- Match the existing aesthetic: **Fraunces + Inter** fonts, palette vars `--hfx / --stj / --spm / --pei / --idm / --gold`.

## Verify before claiming done

- Load pages headlessly (serve locally, e.g. `python3 -m http.server`) and check the switcher/map/ribbon render and the console is clean.
- After deploy, confirm live (public resolver or `--resolve` to a Cloudflare IP; the local LAN may lag on DNS — see README).
