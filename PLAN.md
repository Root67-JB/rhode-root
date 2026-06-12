# Bud's Dashboard — Plan

**Owner:** rhode
**End user:** Bud (rhode's dad), Palm Beach Ferrari salesperson
**Existing iOS app:** voice-first Ginny assistant in TestFlight Beta

## Why this exists

This site started as a Twilio A2P / sole-proprietor compliance landing page (see `/about.html` — content preserved from the original `index.html`). With session-106's reframe, it now ALSO serves as Bud's read-mostly browser dashboard — a companion to the voice app for when he's at his desk, planning tomorrow at home, or wants a visual scan of inventory.

The compliance content stays at `/about.html` so Twilio reviewers still find the URL they recorded during Brand registration.

## v1 — this commit (static preview with mock data)

Goal: ship a layout dad can react to BEFORE we invest in backend wiring + auth. Catches design problems early.

- **One page**, vertical scroll, no nav menu
- Five sections: header + Bebe, today's snapshot (4 big numbers), inventory grid, reminders, customer search box, recent interactions
- **All data is mock** — every number, photo placeholder, customer name, and reminder is hardcoded for preview. Clearly labeled
- Mobile-first responsive (single column phone, 2-3 column desktop)
- Big readable typography (18px body, 56-72px numbers)
- Existing Ferrari-red accent (`#c1272d`)
- No JavaScript yet (besides a no-op search-box input)

## v2 — next session (real data + auth)

- **Backend:** three new HTTP endpoints on the brain at Fly:
  - `GET /web/snapshot` — counts + recent-activity rollup (mirrors the daily email's aggregate())
  - `GET /web/customers/search?q=...` — proxy to `find_customer_by_name`
  - `GET /web/interactions/recent` — last N voice-logged items
  - All gated by the same Apple JWT verification + `APPLE_AUTHORIZED_SUB` allowlist as the iOS minter
- **Frontend:** swap mock data for real `fetch()` calls
- **Auth on web:**
  - Apple Sign-In for Web (Service ID + return URL configured in Apple Developer)
  - 30-day signed cookie after first sign-in
  - Reuses the brain's existing Apple sub allowlist — same lock, second front door
- **Hosting:**
  - Static files stay on GitHub Pages (zero ops) OR move to Fly for unified domain
  - Backend endpoints live on the existing `sales-assistant-minter` Fly app

## v3 — later (if dad actually uses it)

- Inventory card detail overlay (price history, photos, VDP link)
- Tap-to-call top customers
- Daily-email archive (yesterday's, last week's)
- Calendar overview integrating Google Calendar
- Tap a reminder to mark done (write tool, needs confirmation gate)

## What this is NOT

- Not a marketing site for outside customers
- Not a write-heavy tool (voice app stays the primary input path)
- Not a Salesforce competitor — read-mostly, dashboard-shaped
- Not a place to ship the Apple TestFlight invite flow

## Non-goals for v1

- No auth (everything is mock data)
- No real photos (gray gradient placeholders)
- No interactivity beyond visual layout
- No write actions

## Bebe

The yellow-lab photo Bud asked for in session-106 lives in the dashboard header. v1 uses a placeholder; v2 drops in dad's actual photo of Bebe.

**To add the real photo later:** save the file as `bebe.jpg` (or `.png`) in this repo root, then update the `<img>` `src` in `index.html`. GitHub Pages auto-redeploys on push.
