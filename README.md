# 888MECH · Mobile Mechanics

> On-demand mobile mechanics for cars, trucks, and motorcycles.
> **Scranton · NEPA** · We come to you.

Live site: **[888mech.com](https://888mech.com/)**
Call/Text: **(570) 614-8445**
Cash App: **[$Triple888mech](https://cash.app/$Triple888mech)**

---

## What this is

A Progressive Web App (PWA) built for 888MECH — on-demand mobile mechanic
services in Scranton and Northeast Pennsylvania. Installable on iOS and Android
home screens. Works offline after first visit. iOS liquid-glass UI layered over
a Harley-Davidson industrial aesthetic (chrome, orange, matte black).

## Tech

- Pure HTML/CSS/JS — no build step, no framework
- PWA manifest + service worker for home-screen install and offline
- JSON-LD structured data (`AutoRepair` + `MotorcycleRepair` + `FAQPage`) for
  Google rich results
- Open Graph + Twitter Card meta for link previews on iMessage, Facebook, etc.
- SVG logo scales to any resolution, icon.svg maskable variant for PWA install
- Hosted on GitHub Pages via `actions/deploy-pages`

## File structure

```
.
├── index.html               # main app (PWA shell)
├── manifest.webmanifest     # PWA manifest
├── sw.js                    # service worker (offline cache)
├── robots.txt               # crawler directives + AI scraper blocks
├── sitemap.xml              # sitemap for Google Search Console
├── browserconfig.xml        # Windows tile config
├── humans.txt               # team info
├── CNAME                    # custom domain binding (888mech.com)
├── .nojekyll                # disables Jekyll processing on GitHub Pages
├── icon.svg                 # source logo (scalable)
├── icon-180.png             # apple-touch-icon
├── icon-192.png             # PWA standard
├── icon-512.png             # PWA large
├── icon-512-maskable.png    # PWA maskable (Android)
├── favicon.ico              # multi-res favicon (16/32/48)
├── favicon-16.png
├── favicon-32.png
├── favicon-48.png
├── favicon-64.png
├── og-image.png             # 1200x630 social share card
└── .github/
    └── workflows/
        └── deploy.yml       # GitHub Actions → GitHub Pages
```

## Local preview

Just open `index.html` in a browser, or serve with any static HTTP server:

```sh
# Python 3
python3 -m http.server 8000

# Node
npx serve .
```

Then visit `http://localhost:8000`.

## Deployment to GitHub Pages

### First-time setup

1. Create a new repository on GitHub (e.g., `888MECH/888mech`).
2. Push this folder's contents to the `main` branch.
3. In the repo: **Settings → Pages → Source → "GitHub Actions"**.
4. Push will trigger `.github/workflows/deploy.yml`, which publishes the site.

### Custom domain (888mech.com)

The `CNAME` file binds the site to `888mech.com`. You also need DNS:

**Apex (`888mech.com`) — A records pointing to GitHub's IPs:**
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

**`www.888mech.com` — CNAME record:**
```
www   CNAME   <your-github-username>.github.io
```

Then in the GitHub repo: **Settings → Pages → Custom domain → `888mech.com`**
and tick **"Enforce HTTPS"** once the certificate is provisioned.

### Updating content

Edit `index.html` (prices, text, etc.), commit, and push to `main`. Deploy
takes ~1 minute.

## Pricing auto-adjustment

Prices auto-compound **10% every January 1**, keyed off the base year set in
`index.html`. The baseline is 2026 — the prices in the HTML reflect what's
charged in 2026. On Jan 1, 2027 the app will display every price × 1.10;
on Jan 1, 2028 it'll display base × 1.21, and so on.

### What updates automatically

- Every `$XX` in the menu (`.menu-price`)
- Every `$XX` in notes (e.g., "Diagnosis — $55 up front · deducted from job of $220 or more")
- The "Effective YYYY" tag in each menu header
- The JSON-LD schema prices and description amounts (keeps Google rich results accurate)
- The `dateModified` field in the page schema (signals a crawl refresh)

### How to override / freeze prices

Find this block near the top of the `<script>` in `index.html`:

```js
const BASE_YEAR = 2026;
const ANNUAL_INCREASE = 0.10;
```

Change `BASE_YEAR` to the year you want the *current* hardcoded prices to
represent. Set `ANNUAL_INCREASE = 0` to freeze prices entirely until you
edit them manually.

### How to reset the baseline

If at some point you want to re-anchor prices (say, after a market correction),
edit the hardcoded dollar amounts in the HTML to the new baseline values, then
update `BASE_YEAR` to the current year.

### Debugging

Open the browser console and type `__888MECH_PRICING` — you'll see:

```js
{
  baseYear: 2026,
  currentYear: 2026,
  yearsElapsed: 0,
  multiplier: 1,
  nextIncrease: "Fri Jan 01 2027"
}
```

---

## SEO checklist

- [x] Title tag with primary keyword + location
- [x] Meta description under 160 characters
- [x] Canonical URL
- [x] Open Graph tags (Facebook, iMessage, LinkedIn)
- [x] Twitter Card tags
- [x] 1200×630 `og-image.png` branded share card
- [x] Favicon (multi-res `.ico` + SVG + PNG)
- [x] PWA manifest with shortcuts for Pay/Call/Text
- [x] Apple touch icon (180×180)
- [x] Geo meta tags (region, placename, ICBM coords)
- [x] `robots.txt` + `sitemap.xml`
- [x] JSON-LD `AutoRepair` + `MotorcycleRepair` schema with full service
      catalog and prices (eligible for Google rich results)
- [x] JSON-LD `FAQPage` schema (eligible for FAQ rich snippet)
- [x] JSON-LD `WebSite` + `WebPage` + `LocalBusiness` graph

## After going live — do these

1. **Google Search Console** — add property, verify via HTML tag or DNS,
   submit `sitemap.xml`.
2. **Bing Webmaster Tools** — add site, submit sitemap.
3. **Google Business Profile** — claim/create your listing; it's the single
   biggest lever for ranking locally in Scranton. Link it to `888mech.com`.
4. **Rich Results Test** — run https://search.google.com/test/rich-results on
   `https://888mech.com/` to confirm the schema is valid.
5. **PageSpeed / Lighthouse** — aim for 95+ on Performance, Accessibility,
   Best Practices, SEO, and PWA.
6. **Share testing**:
   - https://www.opengraph.xyz/ — preview OG card
   - https://cards-dev.twitter.com/validator — Twitter card preview
   - Send yourself an iMessage link to see the rich preview.

## License

© 2026 888MECH — All rights reserved.
