# ACreatorsLab — Project Knowledge

## ⛔ Stop on ambiguity (master rule)

If there is even a slight area of uncertainty — anything unclear or ambiguous about the task, scope, file targets, parameter values, or expected outcomes — **do not make assumptions and proceed.** Stop, double-check the relevant state (read files, query systems, inspect state), and/or ask the user before taking action. Applies to every operation with non-trivial blast radius: file writes, deployments, daemon restarts, config edits, deletes, schema interpretation, anything that changes shared state.


## ⛔ One watcher per workflow trigger

When a background watcher (bash poller, log-tail loop, file-system watcher, post-condition handler, or any long-running process whose purpose is to detect a single state transition) already exists for a given workflow event — **do not spawn a second watcher polling the same condition**. Modify the existing watcher to handle the additional action(s) instead. Multiple watchers waiting on the same trigger waste PIDs, multiply log-file reads, and create race conditions between near-simultaneous firings. Examples: if a watcher is already polling for "Batch done" to un-pause file A, and you also need to un-pause file B on the same event, extend the existing watcher rather than spawning a new one.
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

## PerfectStudio Pitch Assets (live on acreatorslab.com)
The PerfectStudio tool card in `index.html` carries a "For Adult Creators" pitch block in addition to the public `Get PerfectStudio →` CTA that points to `perfectstudio.app`. The two adult-creator assets are hosted **here** on acreatorslab.com — not on perfectstudio.app — because perfectstudio.app is the non-adult public site (per the global Contact Email Policy) and must not mix adult-brand attribution.

| Asset | Path | URL |
|---|---|---|
| 15-second portrait promo | `perfectstudio-promo.mp4` | https://acreatorslab.com/perfectstudio-promo.mp4 |
| Long-form sales pitch (HTML) | `perfectstudio-pitch.html` | https://acreatorslab.com/perfectstudio-pitch.html |

Both are linked from the PerfectStudio tool card via the `.tool-pitch-block` element. The pitch HTML carries the same ACreatorsLab brand tokens (cream/crimson/Playfair/DM Mono) as `index.html` and embeds the promo video at the top. Source builder for the promo lives at `/Users/mac/Desktop/PerfectStudio/scripts/build_promo.py` (writes to `/Users/mac/Desktop/perfectstudio-promo.mp4` — copy the result over into this directory after each rebuild).

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

## Automation Streams (live in `index.html`)

The `#automation` section is a 6-card grid. Each card is a `.automation-stream` block — HTML-only, no JS. To add/remove/reorder: edit `index.html` directly, keep the `Stream NN` numbers contiguous (both the HTML comment and the `.stream-number` span), and renumber everything below the insertion point.

| # | Name | Platform | Notes |
|---|---|---|---|
| 01 | Automated Posting | OnlyFans | Library rotation + Acaption captions |
| 02 | Creator Promo Automation | OnlyFans | Paid placements with tracking links |
| 03 | Share for Share (SFS) | OnlyFans | Unpaid cross-creator reach exchange — added 2026-05-28 |
| 04 | Auto Mass Messaging | OnlyFans | Free + PPV broadcasts |
| 05 | AutoXPoster | X / Twitter + More | Card links to `autoxposter.com` |
| 06 | Telegram Channel | Telegram | PPV via Telegram Stars |

## Flyer Generation
```bash
node -e "const puppeteer = require('puppeteer'); (async () => { const browser = await puppeteer.launch({ executablePath: '/Applications/Google Chrome.app/Contents/MacOS/Google Chrome', args: ['--no-sandbox'] }); const page = await browser.newPage(); await page.setViewport({ width: 1080, height: 1350 }); await page.goto('file:///Users/mac/Desktop/Xproject/flyer.html', { waitUntil: 'networkidle0' }); await page.screenshot({ path: '/Users/mac/Desktop/Xproject/flyer.png' }); await browser.close(); })();"
```

## File Overwrite Policy

- **⛔ Never silently overwrite a local file.** Before any operation that would replace an existing file on the local machine (image/format conversion, codegen, downloads, copies, moves to an occupied path, save-as targets, batch processing), check whether a same-named file already exists at the destination. If it does, **stop and ask the user**: overwrite, or pick a new name? Do not decide on your own based on file size, content similarity, timestamps, single-frame-vs-animated checks, or any other heuristic. The user has to choose. This applies even when the existing file looks redundant, auto-generated, or "obviously" derived from the same source. **Burned 2026-05-12**: silently overwrote 33 single-frame `.gif` files in `/Volumes/All/Gifs/1-25 MB/` during a batch JPG→GIF conversion after self-deciding it was safe.

## Netlify build diagnosis

- **⛔ Don't act on `Build script returned non-zero exit code: 2 / 4` without reading the actual deploy log + getting explicit user approval.** That surface error wraps multiple unrelated failure modes (secret-scanner false positives, file-count timeouts, plugin install errors, npm install failures, function bundling, build-env limits). The real error lives ONLY in the Netlify web UI at `https://app.netlify.com/projects/$SITE/deploys/$DEPLOY_ID` — the public REST API does NOT expose log content. Before disabling auto-builds, switching deploy mechanisms, adding env vars, overriding plugins, or any other architectural change: pause, ask the user to paste the actual log section, get approval, then act. Burned 2026-05-16 on AutomationFlows — misdiagnosed a 3-day outage as a Netlify plugin issue and shipped 3 architectural commits before the real cause (secret-scanner false positives, fixed with `SECRETS_SCAN_ENABLED=false`) was confirmed. See `~/.claude/CLAUDE.md` "Workflow Rules" for the full version.
