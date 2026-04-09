# ACreatorsLab — Project Knowledge

## What This Is
Aggregator/hub website showcasing all adult-market tools and products built by the same owner. Acts as a discovery and cross-sell layer — customers landing on one product see the full suite.

**Brand**: ACreatorsLab — domain: `acreatorslab.com`

## Why It Exists
- All products serve the same buyer (OF creator / adult agency)
- One SEO target instead of six fragmented domains
- Cross-sell is automatic when everything is in one place
- Authority signal: looks like a software company, not scattered side projects

## Critical Rule — No Checkout Here
All payment links point OUT to individual product domains (`acaption.com`, `gifperfect.com`, etc.).
The aggregator never hosts a checkout. This protects Stripe on GIF Perfect — if checkout lived here on an explicitly adult-branded site, Stripe would flag it.

## Stack
- Pure HTML/CSS/JS — single `index.html`, no framework, no build step
- Fonts: **Playfair Display** (serif headlines, italic accents) + **DM Mono** (monospace body) — same as Acaption
- Hosted on Netlify (static deploy)
- Dev server: `node server.js` → `http://localhost:3000`

## Design Language
Light colour scheme (switched from dark, April 2026):
```css
--bg:          #f5f2ee;
--surface:     #eeeae6;
--surface2:    #e8e4e0;
--border:      #d8d2ca;
--border2:     #cec8c0;
--accent:      #c8102e;   /* same red */
--text:        #0e0e0e;
--muted:       #6b6259;
--muted2:      #a09888;
```
Noise texture overlay, fade-up animation on load.
Tool cards: red top-border reveal on hover, dimmed opacity for coming-soon items.
**Silhouette background**: `flyer-bg.jpg` (blurred nude woman, 41KB compressed) applied as fixed background overlay on both `acreatorslab.com` and `acaption.com` — CSS class `.silhouette-bg`, `mix-blend-mode: darken`, `opacity: 0.55`, `z-index: 50`.

## Products Featured

| Product | Status | Domain | Price | Notes |
|---------|--------|--------|-------|-------|
| Acaption | ✅ Live | acaption.com | €59–€199/mo | Includes @AcaptionBot on Telegram (200 ⭐ / 3 free demos) |
| GIF Perfect | ✅ Live | gifperfect.com | $29–$199 one-time | Stripe live |
| Slo-Mo Perfect | ✅ Live | slomoperfect.com | $29–$179 one-time | Desktop slo-mo tool, Python/tkinter/PyInstaller, niche-agnostic |
| ImageTagger + Viewer | ✅ Live | atagger.com | $79 one-time | Desktop app, AI image tagging |
| VideoTagger + Viewer | 🔜 Coming Soon | TBD | Bundled | Same catalogue/Viewer as ImageTagger |
| AutoXPoster | 🔜 Coming Soon | autoxposter.com | Subscription TBD | Engine running in prod, needs SaaS wrapper |

## Sections in index.html
1. **Nav** — fixed, blurs in on scroll
2. **Hero** — "ACreatorsLab" logo, tagline, two CTAs
3. **Tools Grid** — 2-col, live tools full opacity / coming-soon dimmed. Live: Acaption (with AcaptionBot block), GIF Perfect, Slo-Mo Perfect, ImageTagger. Coming soon: VideoTagger, AutoXPoster
4. **Automation Section** — 5 streams: OF Posting, Creator Promo Automation, Auto Mass Messaging, AutoXPoster, Telegram Channel
5. **Agency Section** — API access, B2B invoicing, volume plans, EU VAT
6. **Community Resources** — KinkyBeatricePro script packs (EXTERNAL, no affiliation, hard disclaimer)
7. **Newsletter/Waitlist** — email capture, goes to `acreatorslab@translatea.com`
8. **Footer** — links to acaption.com, gifperfect.com, atagger.com

## Platform Tags
All tool cards have platform tags: `Any Platform` / `OnlyFans` / `X` / `Telegram`

## Community Resources Disclaimer
KinkyBeatricePro.com script packs are featured as "Community Resources" — third-party, external links.
**No affiliation, ownership, or association must ever be implied.** The disclaimer box in the HTML must remain and must reference ACreatorsLab (not Xproject).

## Flyer
`flyer.html` + `flyer-bg.jpg` in project root. Rendered to `flyer.png` (1080×1350px) via Puppeteer:
```bash
node -e "const puppeteer = require('puppeteer'); (async () => { const browser = await puppeteer.launch({ executablePath: '/Applications/Google Chrome.app/Contents/MacOS/Google Chrome', args: ['--no-sandbox'] }); const page = await browser.newPage(); await page.setViewport({ width: 1080, height: 1350 }); await page.goto('file:///Users/mac/Desktop/Xproject/flyer.html', { waitUntil: 'networkidle0' }); await page.screenshot({ path: '/Users/mac/Desktop/Xproject/flyer.png' }); await browser.close(); })();"
```
Flyer design: silhouette bg, product names left side (right-aligned), automation names right side (left-aligned), absolute positioned to hug body curve.

## Dev Server
```bash
node server.js   # serves on http://localhost:3000
```
`.claude/launch.json` is configured with `"Xproject Dev"` → `node server.js` on port 3000.

## Deploy
Netlify static deploy. **Must use `--dir` flag** — the `.netlify/state.json` siteId was previously pointing to a wrong orphan site and has been corrected.
```bash
netlify deploy --prod --dir=/Users/mac/Desktop/Xproject
```
Netlify site: `acreatorslab` (ID: `d8d8d8a1-426b-49f1-abbf-f3b4dfb7f638`) → `acreatorslab.com`

The same silhouette background (`flyer-bg.jpg`) is also deployed to `acaption.com`. Deploy Acaption from:
```bash
cd /Users/mac/Desktop/Acaption && netlify deploy --prod --dir=deploy
```
Acaption Netlify site: `rad-brioche-c5331b` (ID: `e5798797-f50c-47d2-bfc0-7211cc17fd03`) → `acaption.com`

## Pending Decisions
- [x] Final brand name / domain → `acreatorslab.com`
- [x] Email collection backend → Netlify Forms (AJAX, `name="waitlist"`)
- [x] Replace `noindex` → real SEO + OG tags added
- [x] Wire newsletter form → done
- [x] Set up Netlify site, link `acreatorslab.com` domain → live
- [x] Contact email → `acreatorslab@translatea.com`
- [ ] Add Open Graph image (`og:image`) — placeholder still missing
- [ ] VideoTagger domain TBD
- [ ] AutoXPoster SaaS wrapper

## Related Projects (Other Sessions)
| Project | Folder | Notes |
|---------|--------|-------|
| Acaption | `~/Desktop/Acaption` | Adult AI caption SaaS — source of design language |
| AcaptionAPI | `~/Desktop/AcaptionAPI` | Hetzner VPS backend for Acaption + GIF Perfect + Slo-Mo Perfect |
| GIF Perfect | `~/Desktop/GifPerfect` | Desktop GIF tool, Stripe live |
| Ucaption | `~/Desktop/Ucaption` | Universal (non-adult) sister product |
| XAutoPosting | `~/Desktop/XAutoPosting` | X autoposter — future autoxposter.com |
| ImageTagger | `~/Desktop/ImageTagger` | AI image tagging CLI — live at atagger.com |
| VideoTagger | `~/Desktop/VideoTagger` | AI video tagging CLI |
| Viewer | `~/Desktop/Viewer` | Media browser for tagger output |
| SlowMo | `~/Desktop/SlowMo` | Slo-Mo Perfect — desktop slo-mo tool, slomoperfect.com |
