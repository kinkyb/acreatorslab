# ACreatorsLab — Project Knowledge

## Workflow Rules
- **Verify deploy target before deploying**: Before running any deploy command, confirm which Netlify site ID / project it will deploy to. Deploying to the wrong site is a silent failure — the correct site gets nothing.
- **Update CLAUDE.md after every push**: After every git push, update this file. Commit and push the CLAUDE.md update immediately after.
- **⛔ Before EVERY new build, assume a bug exists and find it — whether or not one has been reported.** This is a default-on rule, not a conditional. Re-scan the entire pipeline end-to-end — front-end (UI / IPC / renderer) AND back-end (main process, business logic, pipeline, bundling) — actively hunting for the bug you've assumed is there. If you find one, fix it and re-scan. If after a thorough double-check you're 100% sure no bug exists, build a standalone repro of the **full production chain** (every post-processing step — watermarks, composites, encoders, format conversions, IPC envelope) and prove the path produces the expected output. Only at that point do you tag / commit / trigger the build. Burned 2026-05-12 on PerfectStudio v1.2.2: standalone test omitted the trailing watermark composite, missing that sharp's `.composite()` overwrites prior overlays when chained — v1.2.3 was the real fix. Cost of skipping: 2 wasted ~20-min CI builds + user-visible repeat failure + credibility hit.
- **⛔ Windows Electron installers: always hide the in-window menu bar before shipping.** Electron defaults to showing a `File / Edit / View / Help` strip across the top of every BrowserWindow on Windows (and Linux), which looks unprofessional for a focused desktop app. Required fix: call `win.setMenuBarVisibility(false)` once per window right after `win.loadFile(...)`, with NO platform guard. macOS treats it as a no-op (menu lives in the system menu bar at the top); Windows hides the strip. The menu can stay registered via `Menu.setApplicationMenu(...)` — visibility and registration are independent, so keyboard accelerators still fire and Alt brings the bar back when needed. Always re-verify on a fresh Windows install before shipping a new desktop-app version. Burned 2026-05-12 on PerfectStudio v1.2.3: `main.js` had `if (process.platform === 'darwin') win.setMenuBarVisibility(false)` — macOS-only guard, so Windows users saw the strip; fixed in v1.2.4 by removing the guard.


## Contact Email
All contact links, footer emails, and form targets use `acreatorslab@translatea.com` — never `acreatorstore@translatea.com`.

## Critical Rule — No Checkout Here
All payment links point OUT to individual product domains (`acaption.com`, `gifperfect.com`, etc.). The aggregator never hosts a checkout. This protects Stripe on GIF Perfect — if checkout lived on an explicitly adult-branded site, Stripe would flag it.

## Community Resources Disclaimer
KinkyBeatricePro.com script packs are featured as "Community Resources" — third-party, external links. **No affiliation, ownership, or association must ever be implied.** The disclaimer box in the HTML must remain and must reference ACreatorsLab (not Xproject).

## Dev Server
```bash
node server.js   # serves on http://localhost:3000
```

## Deploy
```bash
netlify deploy --prod --dir=/Users/mac/Desktop/Xproject
```
Netlify site: `acreatorslab` (ID: `d8d8d8a1-426b-49f1-abbf-f3b4dfb7f638`) → `acreatorslab.com`

**Must use `--dir` flag** — `.netlify/state.json` siteId was previously pointing to a wrong orphan site and has been corrected.

## Flyer Generation
```bash
node -e "const puppeteer = require('puppeteer'); (async () => { const browser = await puppeteer.launch({ executablePath: '/Applications/Google Chrome.app/Contents/MacOS/Google Chrome', args: ['--no-sandbox'] }); const page = await browser.newPage(); await page.setViewport({ width: 1080, height: 1350 }); await page.goto('file:///Users/mac/Desktop/Xproject/flyer.html', { waitUntil: 'networkidle0' }); await page.screenshot({ path: '/Users/mac/Desktop/Xproject/flyer.png' }); await browser.close(); })();"
```

## File Overwrite Policy

- **⛔ Never silently overwrite a local file.** Before any operation that would replace an existing file on the local machine (image/format conversion, codegen, downloads, copies, moves to an occupied path, save-as targets, batch processing), check whether a same-named file already exists at the destination. If it does, **stop and ask the user**: overwrite, or pick a new name? Do not decide on your own based on file size, content similarity, timestamps, single-frame-vs-animated checks, or any other heuristic. The user has to choose. This applies even when the existing file looks redundant, auto-generated, or "obviously" derived from the same source. **Burned 2026-05-12**: silently overwrote 33 single-frame `.gif` files in `/Volumes/All/Gifs/1-25 MB/` during a batch JPG→GIF conversion after self-deciding it was safe.
