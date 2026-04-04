# Xproject — Project Knowledge

## What This Is
Aggregator/hub website showcasing all adult-market tools and products built by the same owner. Acts as a discovery and cross-sell layer — customers landing on one product see the full suite.

**Placeholder name**: "Xproject" — final domain/brand TBD.

## Why It Exists
- All products serve the same buyer (OF creator / adult agency)
- One SEO target instead of six fragmented domains
- Cross-sell is automatic when everything is in one place
- Authority signal: looks like a software company, not scattered side projects

## Critical Rule — No Checkout Here
All payment links point OUT to individual product domains (`acaption.com`, `gifperfect.com`).
The aggregator never hosts a checkout. This protects Stripe on GIF Perfect — if checkout lived here on an explicitly adult-branded site, Stripe would flag it.

## Stack
- Pure HTML/CSS/JS — single `index.html`, no framework, no build step
- Fonts: **Playfair Display** (serif headlines, italic accents) + **DM Mono** (monospace body) — same as Acaption
- Hosted on Netlify (static deploy, same pattern as Acaption/Ucaption)
- Dev server: `node server.js` → `http://localhost:3000`

## Design Language
Derived from `acaption.com` but slightly lighter:
```css
--bg:      #0e0e0e;   /* Acaption is #080808 — this is warmer */
--surface: #161616;
--border:  #252525;
--accent:  #c8102e;   /* same red */
--text:    #e8e0d8;   /* same warm off-white */
--muted:   #6b6259;
```
Noise texture overlay, radial red glow behind hero, fade-up animation on load.
Tool cards: red top-border reveal on hover, dimmed opacity for coming-soon items.

## Products Featured

| Product | Status | Domain | Price | Notes |
|---------|--------|--------|-------|-------|
| Acaption | ✅ Live | acaption.com | €59–€199/mo | Payments pending CCBill approval |
| GIF Perfect | ✅ Live | gifperfect.com | $29–$199 one-time | Stripe live |
| ImageTagger + Viewer | 🔜 Coming Soon | TBD | $79 one-time | Internal tool, needs desktop GUI |
| VideoTagger + Viewer | 🔜 Coming Soon | TBD | Bundled | Internal tool, needs desktop GUI |
| AutoXPoster | 🔜 Coming Soon | autoxposter.com | Subscription TBD | Engine running in prod, needs SaaS wrapper |

## Sections in index.html
1. **Nav** — fixed, blurs in on scroll
2. **Hero** — "Xproject" logo, tagline, two CTAs
3. **Tools Grid** — 2-col, live tools full opacity / coming-soon dimmed
4. **Creator Toolkit Bundle** — ImageTagger + VideoTagger + Viewer, €79 one-time, coming soon
5. **Agency Section** — API access, B2B invoicing, volume plans, EU VAT
6. **Community Resources** — KinkyBeatricePro script packs (EXTERNAL, no affiliation, hard disclaimer)
7. **Newsletter/Waitlist** — email capture for coming-soon products
8. **Footer**

## Community Resources Disclaimer
KinkyBeatricePro.com script packs are featured as "Community Resources" — third-party, external links.
**No affiliation, ownership, or association must ever be implied.** The disclaimer box in the HTML must remain.

## Dev Server
```bash
node server.js   # serves on http://localhost:3000
```
`.claude/launch.json` is configured with `"Xproject Dev"` → `node server.js` on port 3000.
`preview_start` will work when this directory is the Claude Code project root.

## Deploy
Same pattern as Acaption/Ucaption — Netlify static deploy.
No Netlify functions needed (pure static, all links point out to other domains).
```bash
netlify deploy --prod
```
Site ID and domain TBD once final brand name is chosen.

## Pending Decisions
- [ ] Final brand name / domain (placeholder: "Xproject")
- [ ] Email collection backend for waitlist (Netlify Forms or a function)
- [ ] Replace `noindex` meta tag with real SEO tags once domain is live
- [ ] Wire newsletter form to actual endpoint
- [ ] Add real Open Graph image once brand name is confirmed

## Related Projects (Other Sessions)
| Project | Folder | Notes |
|---------|--------|-------|
| Acaption | `~/Desktop/Acaption` | Adult AI caption SaaS — source of design language |
| AcaptionAPI | `~/Desktop/AcaptionAPI` | Hetzner VPS backend for Acaption + GIF Perfect |
| GIF Perfect | `~/Desktop/GifPerfect` | Desktop GIF tool, Stripe live |
| Ucaption | `~/Desktop/Ucaption` | Universal (non-adult) sister product |
| XAutoPosting | `~/Desktop/XAutoPosting` | X autoposter — future autoxposter.com |
| ImageTagger | `~/Desktop/ImageTagger` | AI image tagging CLI |
| VideoTagger | `~/Desktop/VideoTagger` | AI video tagging CLI |
| Viewer | `~/Desktop/Viewer` | Media browser for tagger output |
