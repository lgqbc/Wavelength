# wavelength.wiki — project overview

## What it is
A free, no-account, browser-based suite of tools for music producers. Three pages:
- `/` — Musical Calculator: BPM ↔ ms ↔ Hz ↔ note ↔ sample conversion, tap tempo, click track, dotted/triplet divisions, wavelength/temperature calc, plus scratchpad notes + basic calculator.
- `/session-notes/` — timestamped note-taking synced to audio playback or live mic recording, WAV export.
- `/about/` — blurb, formulas reference (tempo→ms, ms→hz, ms→samples, note→hz), contact (SimpleLogin email alias).

## Stack
- Vanilla JS, no framework, no build step, no dependencies — single-file-per-page HTML/CSS/JS.
- PWA: manifest.json, service worker (`sw.js`) with network-first navigation caching, cache-first static assets, stale-while-revalidate for cross-origin (Google Fonts).
- Persistence: `localStorage` for calculator/session state, IndexedDB (single-slot) for the last-opened audio file's bytes.
- No `eval()` — custom recursive-descent parser handles inline math expressions.

## Hosting / infra
- GitHub Pages, custom domain `wavelength.wiki` via CNAME, DNS on GoDaddy (not proxied through Cloudflare — no custom headers possible currently).
- View counter: separate Cloudflare Worker + KV, pixel-beacon on all 3 pages, on the account's `wavelengthapp.workers.dev` subdomain (renamed from a subdomain that leaked a real name).

## Current state (as of this review)
- Live, picking up traction (~500 views/24h at time of writing) via Reddit promotion in music-production subreddits.
- SEO basics in place: per-page canonical/OG/Twitter meta, sitemap submitted to Google Search Console, robots.txt correct, JSON-LD `WebApplication` structured data added to `/` and `/session-notes/`.
- Explainer/FAQ text was tried on `/` and `/session-notes/` to add indexable content, then **reverted** — the site is meant to stay minimal, and hidden/invisible SEO text isn't a legitimate technique anyway (Google's spam policy explicitly covers hidden text). Decision: take the SEO hit, keep the UI clean; any extra copy will go on `/about/` later instead, as visible content someone would actually choose to read.
- Dead `window.storage` branches (leftover from artifact prototyping) removed from `index.html` and `session-notes/index.html` — always fell through to `localStorage` anyway, no behavior change.

## Known, deliberately not fixed yet
- **Pages are still thin on indexable text** (~60–200 words) — accepted trade-off for keeping the tool minimal. Any future content addition should be real, visible copy (e.g. on `/about/`), not hidden text.
- **No CSP header/meta tag.** Low risk today (no user-controlled HTML sinks found — note text is properly escaped before rendering), but flagged for later, especially once/if the site is proxied through Cloudflare and adding headers becomes cheap.
- **No security headers at all** — GitHub Pages can't set them; would need Cloudflare (or another CDN) in front of the domain.
- **Session Notes localStorage keys accumulate** per opened file/recording, never cleaned up on delete. Low practical impact at current scale.
- **OG/Twitter share image is the square 512×512 app icon**, not a proper 1200×630 banner — affects how link previews look when shared (Reddit/Discord/Twitter), separate from SEO.
- Sitemap has no `<lastmod>` dates.

## Promotion so far
Reddit (music production subreddits), Google Search Console sitemap submission. Next steps (Show HN, Product Hunt, `awesome-audio` list PRs, niche producer-tool blogs, Cloudflare Web Analytics for referrer tracking) — parked for later.
