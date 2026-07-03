# Canada Trip 2026 — Atlantic Canada Itinerary

Interactive, single-page trip presentations for a **July 2026 Atlantic Canada trip** (Halifax · PEI · Saint-Pierre 🇫🇷 · Newfoundland · Îles de la Madeleine). Built to be shared with family to review and pick a plan.

## Live site

Hosted on **Cloudflare** at **https://canada.tuckman.us** — three versions, all served by one Worker:

| URL | What it is |
|-----|------------|
| **https://canada.tuckman.us/** | Main 4-plan presentation (A–D), **direct** style |
| **https://canada.tuckman.us/full** | Illustrated / travelogue version (kept as a backup) |
| **https://canada.tuckman.us/stjohns** | Standalone **St. John's-by-air** alternative itinerary |

## Hosting & deployment

- **Cloudflare Workers (static assets).** Custom domain `canada.tuckman.us` lives on the `tuckman.us` Cloudflare zone.
- **Deploy = `git push` to `main`.** Cloudflare's connected Git build automatically runs `npx wrangler deploy`. There is **no manual/dashboard step** for updates.
- Config lives in the repo:
  - `wrangler.jsonc` — assets-only Worker (no `main`), `assets.directory: "."`, route `canada.tuckman.us` with `custom_domain: true`.
  - `.assetsignore` — keeps repo internals and all `.md` files out of what gets served publicly.

## Files

| File | Role |
|------|------|
| `trip-presentation.html` | **Source** of the main presentation — edit this |
| `index.html` | Deployed copy of the main page (served at `/`). Regenerate with `cp trip-presentation.html index.html` before committing |
| `full.html` | Illustrated/travelogue version (`/full`) |
| `stjohns.html` | St. John's-by-air alternative (`/stjohns`) |
| `itinerary-research.md` | Research notes & source of truth: plans, costs, flight/ferry/lodging data, sources (not served) |
| `wrangler.jsonc`, `.assetsignore` | Cloudflare config |

## Updating the site

1. Edit `trip-presentation.html` (or `full.html` / `stjohns.html`).
2. If you changed the main page: `cp trip-presentation.html index.html`.
3. `git add … && git commit && git push` → Cloudflare deploys in ~1 minute.
4. Verify at the URL (first load on the local LAN may lag — see **DNS note** below).

## Why not GitHub Pages? (the migration story)

The site originally lived on GitHub Pages (`miketuckman.github.io/canada-trip-2026`). On **2026-07-02** the legacy Pages builder failed repeatedly for hours: the **build step passed but the deploy step kept throwing `Deployment failed, try again later`** — a GitHub-side infrastructure degradation, not a content problem. Pushes sat undeployed.

We migrated to Cloudflare and **deleted the GitHub Pages site**. If anyone reconsiders Pages later:

- The failures were on GitHub's deploy step, not the build (our content was fine).
- Trying an Actions-based Pages workflow was blocked by a missing `workflow` OAuth scope on the CLI token.
- Cloudflare uses the identical `git push` workflow and has been reliable.

## DNS note (local network)

`tuckman.us` is a Cloudflare zone, but the local LAN resolves through **two Technitium DNS servers**. After any DNS record change (e.g., adding the `canada` subdomain), **flush the Technitium cache** (Cache tab → Flush) on both servers, or a **30-minute negative-cache TTL** will keep the LAN from resolving the new record. Browsers and the OS cache separately too (Chrome: `chrome://net-internals/#dns` → Clear host cache). Public resolvers (`1.1.1.1`, `8.8.8.8`) are the quickest way to confirm a record is actually live on the internet.
