# H2H Music Improv Dataset — Site Progress

## What the site is
A static GitHub Pages site (`index.html`) that shows annotated free-improvisation clips. Each clip card has an embedded YouTube video with an SVG annotation timeline directly below it. Clicking the timeline seeks the video.

## Data files
| File | Purpose |
|------|---------|
| `data/master.csv` | One row per annotation event. Columns: `date_recorded`, `clip_num`, `id`, `annotation_providing_instrument`, `action`, `action_taking_instrument`, `start`, `clip_duration`, `performer_id` |
| `data/performers.csv` | Maps `date_recorded` + `annotation_providing_instrument` → `performer_id`. Used to auto-derive clip pairings and anonymized instrument labels (e.g. `guitar1`, `trombone2`). |

## Clip registry (`CLIP_META` in index.html)
Add a `youtubeId` string to show a clip. Leave `null` to hide it entirely.

| id | youtubeId |
|----|-----------|
| 11-19-2025-Clip1 | null |
| 11-19-2025-Clip2 | null |
| 11-19-2025-Clip3 | `Tg3z55bgLSk` ✓ |
| 03-24-2026-Clip4 | null |
| 03-24-2026-Clip5 | null |
| 03-24-2026-Clip6 | null |

## Anonymization mapping
Real names are not present anywhere in the site or CSVs. Instrument+ID labels are derived automatically from `performers.csv`.

| Performer | Label |
|-----------|-------|
| Jeff | trombone2 |
| Anthony | guitar1 |
| James | saxophone3 |
| Alexandria | trumpet5 |
| Danny | trombone4 |

## Completed
- **Layout**: annotation timeline stacked directly below video (was side-by-side)
- **Bar alignment**: video and annotation bar share the same horizontal padding via `.clip-body`, guaranteeing identical pixel width
- **Duration sync**: SVG timeline rebuilds from `p.getDuration()` once the YouTube player reports its real duration, so bar scale, click-to-seek, and playhead are all consistent
- **performers.csv linked**: pairings (e.g. `trombone2 × guitar1`) auto-derived from performers.csv by matching date prefix of clip ID; track labels show `instrument+id` (e.g. `guitar1`, `trombone2`)
- **Anonymization**: all real names removed from site
- **Show only linked clips**: only clips with a `youtubeId` are rendered
- **Visualization**: no-ending mode (last phase extends to clip end using synthetic `initiation` endpoint); unknown phases excluded; legend trimmed to negotiation / proposal / stability only
- **Click-to-seek fix (deployed)**: switched video pane from pre-built `<iframe>` to `<div>` placeholder so `YT.Player` owns the iframe; added `origin: window.location.origin` to `playerVars`
- **Race condition fix**: `initPlayers()` now gates on both `ytApiReady` and `clipsRendered` flags, so YouTube players are never created before the DOM exists
- **`href="#"` fix**: Zenodo and Paper placeholder buttons changed to `javascript:void(0)` so clicking them no longer appends `#` to the URL (which broke YouTube postMessage validation)
- **Click-to-seek UX**: plain-language instruction ("Click the bar to jump to that moment in the video"), gold hover glow on the bar, cursor changed to pointer, seek feedback says "jumped to X:XX"

## ⚠️ Not yet committed to GitHub
All changes above are local only. Need to `git add` + `git commit` + `git push` before they appear on the live site.

## Pending / next session
- **Commit & push**: push all local changes to GitHub Pages
- **Per-stem audio**: embed native `<audio>` elements (one per performer stem) below the annotation bar
- **Seek-target selector**: pill/toggle above the annotation to choose which component (video or a stem) click-to-seek controls
- **Remaining YouTube IDs**: keep uploading and adding IDs for the rest of the 37 clips in `data/master.csv` (5 are live now)
- **Datasheet**: `datasheet.html` is currently empty
- **Paper link**: `link-paper` in the header is still hidden — arXiv submission pending. Once live, set its `href` and remove `style="display:none"` ([index.html](index.html))
- **Cite section BibTeX**: placeholder entry added at the bottom of the page — fill in exact paper title, pages, and arXiv ID once finalized

## Author / citation info (added)
- Header now shows author byline (no affiliations, per decision) and an ISMIR 2026 publication statement
- Zenodo button wired to https://zenodo.org/records/21726560 (DOI 10.5281/zenodo.21726560)
- Cite section added near the footer with an `@inproceedings` BibTeX placeholder

## How to preview locally
```
cd "/Volumes/mtsandra-t9/H2H Data/h2himprov.github.io"
python3 -m http.server 8080
```
Then open http://localhost:8080. (VS Code Live Server also works and auto-reloads.)

## How to deploy
```
git add -A
git commit -m "your message"
git push
```
GitHub Pages auto-deploys from the `main` branch within ~1 minute.
