# Canada Trip 2026 — Atlantic Canada Itinerary

Interactive, single-page trip presentations for a **July 2026 Atlantic Canada trip** (Halifax · PEI · Saint-Pierre 🇫🇷 · Newfoundland · Îles de la Madeleine). Built to be shared with family to review and pick a plan.

## Live site

Hosted on **Cloudflare** at **https://canada.tuckman.us**, all served by one Worker. The front door is a **landing page** of full-width plan boxes; each links to a standalone plan page:

| URL | What it is |
|-----|------------|
| **https://canada.tuckman.us/** | **Landing page** — 5 full-width plan boxes (A–E), pick one |
| **/stjohns** | **Plan A** — St. John's by air (simplest, all bookable) |
| **/saint-pierre-townhouse** | **Plan B** — Saint-Pierre, booked townhouse |
| **/saint-pierre-sunday** | **Plan C** — Saint-Pierre, Sunday window (more France) |
| **/madeleines** | **Plan D** — Îles de la Madeleine (simplest travel) |
| **/big-loop** | **Plan E** — France → Newfoundland → PEI (most ambitious) |
| **/full** | Archived all-in-one switcher (old A–D labels, kept as backup) |

## Hosting & deployment

- **Cloudflare Workers (static assets).** Custom domain `canada.tuckman.us` lives on the `tuckman.us` Cloudflare zone.
- **Deploy = `git push` to `main`.** Cloudflare's connected Git build automatically runs `npx wrangler deploy`. There is **no manual/dashboard step** for updates.
- Config lives in the repo:
  - `wrangler.jsonc` — assets-only Worker (no `main`), `assets.directory: "."`, route `canada.tuckman.us` with `custom_domain: true`.
  - `.assetsignore` — keeps repo internals and all `.md` files out of what gets served publicly.

## Files

| File | Role |
|------|------|
| `index.html` | **Landing page** (served at `/`) — the 5 plan boxes. Edit directly (not generated from anything) |
| `stjohns.html` | Plan A — `/stjohns` |
| `saint-pierre-townhouse.html` | Plan B — `/saint-pierre-townhouse` |
| `saint-pierre-sunday.html` | Plan C — `/saint-pierre-sunday` |
| `madeleines.html` | Plan D — `/madeleines` |
| `big-loop.html` | Plan E — `/big-loop` |
| `full.html` | Archived all-in-one switcher (`/full`) |
| `trip-presentation.html` | Legacy switcher source (old A–D); no longer copied to `index.html` |
| `itinerary-research.md` | Research notes & source of truth: plans, costs, flight/ferry/lodging data, sources (not served) |
| `wrangler.jsonc`, `.assetsignore` | Cloudflare config |

Each plan page is self-contained (hero, sticky nav, Leaflet map, split-timeline day ribbon, chapters, islands-only cost). They share a visual language but no shared CSS/JS file — edit each independently.

## Updating the site

1. Edit the relevant file — `index.html` for the landing boxes, or the specific plan page (e.g. `big-loop.html`).
2. `git add … && git commit && git push` → Cloudflare deploys in ~1 minute.
3. Verify at the URL (first load on the local LAN may lag — see **DNS note** below).

## Why not GitHub Pages? (the migration story)

The site originally lived on GitHub Pages (`miketuckman.github.io/canada-trip-2026`). On **2026-07-02** the legacy Pages builder failed repeatedly for hours: the **build step passed but the deploy step kept throwing `Deployment failed, try again later`** — a GitHub-side infrastructure degradation, not a content problem. Pushes sat undeployed.

We migrated to Cloudflare and **deleted the GitHub Pages site**. If anyone reconsiders Pages later:

- The failures were on GitHub's deploy step, not the build (our content was fine).
- Trying an Actions-based Pages workflow was blocked by a missing `workflow` OAuth scope on the CLI token.
- Cloudflare uses the identical `git push` workflow and has been reliable.

## DNS note (local network)

`tuckman.us` is a Cloudflare zone, but the local LAN resolves through **two Technitium DNS servers**. After any DNS record change (e.g., adding the `canada` subdomain), **flush the Technitium cache** (Cache tab → Flush) on both servers, or a **30-minute negative-cache TTL** will keep the LAN from resolving the new record. Browsers and the OS cache separately too (Chrome: `chrome://net-internals/#dns` → Clear host cache). Public resolvers (`1.1.1.1`, `8.8.8.8`) are the quickest way to confirm a record is actually live on the internet.
