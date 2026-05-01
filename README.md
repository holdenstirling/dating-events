# Come Mingle, Bring Your Singles

Landing page for a friends-of-friends mingle night at Schoolyard Beer Hall, Denver — Friday June 19, 5–9 PM. 21+, no cover, 100 spots, Fresh Barbershop on-site.

## What's in here

```
byos-website-repo/
├── index.html              ← the entire site, CSS + JS inline
├── favicon.svg             ← browser tab icon (wristband stripes)
├── apple-touch-icon.svg    ← iOS home-screen icon (180px, with "BYS · DENVER" tag)
├── site.webmanifest        ← Android / Chrome PWA-ish manifest
├── og.svg                  ← source for the social-share preview image (export to og.png)
├── robots.txt              ← lets crawlers index the site
├── sitemap.xml             ← single-page sitemap for Google
├── README.md               ← this file
├── DEPLOY-STEPS.md         ← GitHub + Vercel deploy commands
└── .gitignore
```

No build step, no package manager, no dependencies. Open `index.html` directly in a browser to preview, or deploy as-is.

## Deploy

Hosted on Vercel as a static site. Pushes to `main` auto-deploy via Vercel's GitHub integration. See `DEPLOY-STEPS.md` for the first-time setup.

## Editing the site

Everything lives in `index.html`. Common edits:

- **RSVP link** — search for `partiful.com/e/` (appears in the QR markup, the RSVP button, the JSON-LD `offers.url`, and the QR-generation script — four spots) and swap the URL.
- **Date/time** — search for `June 19` (and the ISO timestamps in the countdown script and JSON-LD: `2026-06-19T17:00:00-06:00` / `2026-06-19T21:00:00-06:00`).
- **Venue** — search for `Schoolyard Beer Hall`.
- **Social handles** — search for `bringyoursingles` and `fresh_barbershop_denver`.
- **Domain** — search for `bringyoursingles.com` (canonical, og:url, JSON-LD URLs all reference it). Update everywhere if you deploy to a different domain.

## QR code

The RSVP section renders a QR pointing at the Partiful URL. It loads `qrcode@1.5.3` from jsDelivr at runtime and writes an inline SVG into `#qr-svg`. If the script ever fails to load, a styled "Tap to open RSVP" fallback link stays visible inside the same card.

## Favicon and home-screen icon

- `favicon.svg` is the wristband-stripes mark (black background, four colored stripes — green/pink/yellow/blue, matching the topbar). Modern browsers (Chrome, Firefox, Safari, Edge) all use this directly.
- `apple-touch-icon.svg` is the larger version with the "BYS · DENVER" tag, used when someone "Add to Home Screen" on iOS.
- `site.webmanifest` declares both icons + theme color so Chrome/Android show a proper install prompt.

If you want a more traditional `favicon.ico`, drop one in alongside the SVG — the SVG is preferred by modern browsers but a `.ico` is a good extra fallback. You can generate one from `favicon.svg` at https://realfavicongenerator.net or any SVG-to-ICO tool.

## Social-share preview image (Open Graph)

`og.svg` is the 1200×630 design that previews on iMessage, Slack, Discord, Twitter, Facebook, etc.

**You need to convert it to `og.png` once** so older platforms (iMessage, Facebook) render it cleanly. The `<meta property="og:image">` tag in `index.html` is already pointing at `/og.png`, so the moment that file exists, link previews will work.

Easiest conversion on macOS:
1. Right-click `og.svg` → Open With → Preview.
2. File → Export → Format: PNG → resolution: 144 dpi (gives you 1200×630 px) → Save as `og.png`.

Or use any free SVG→PNG converter (cloudconvert.com, svgtopng.com).

## SEO / discoverability

The page includes:
- A `<title>` and `<meta description>` optimized for the event.
- Open Graph + Twitter Card tags for clean link previews.
- A canonical URL.
- JSON-LD `Event` structured data — Google can show this as a rich event result with date, venue, price, and the Partiful link as the official RSVP.
- `robots.txt` and `sitemap.xml` at the root.

Once the site is live at `bringyoursingles.com`, submit the sitemap in Google Search Console (https://search.google.com/search-console) for faster indexing.

## Accessibility

- Skip-to-content link (visible on keyboard focus).
- `<main>` landmark wrapping the content sections.
- Brand-colored focus rings on every interactive element.
- `prefers-reduced-motion` respected (no smooth-scroll for users who've opted out).
- ARIA labels on the QR card.

## Mobile

Three breakpoints (760px / 480px / 360px). Hero typography scales down to fit iPhone SE; safe-area insets keep content clear of the iPhone notch and home-bar.
