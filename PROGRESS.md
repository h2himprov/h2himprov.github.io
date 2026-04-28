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

## Completed this session
- **Layout**: annotation timeline stacked directly below video (was side-by-side)
- **Bar alignment**: video and annotation bar share the same horizontal padding via `.clip-body`, guaranteeing identical pixel width
- **Duration sync**: SVG timeline rebuilds from `p.getDuration()` once the YouTube player reports its real duration, so bar scale, click-to-seek, and playhead are all consistent
- **performers.csv linked**: pairings (e.g. `trombone2 × guitar1`) auto-derived from performers.csv by matching date prefix of clip ID; track labels show `instrument+id` (e.g. `guitar1`, `trombone2`)
- **Anonymization**: all real names removed from site
- **Show only linked clips**: only clips with a `youtubeId` are rendered
- **Visualization**: ending and unknown phases excluded; only negotiation / proposal / stability shown

## Pending / next session
- **Per-stem audio**: embed native `<audio>` elements (one per performer stem) below the annotation bar
- **Seek-target selector**: pill/toggle above the annotation to choose which component (video or a stem) click-to-seek controls
- **Remaining YouTube IDs**: upload and add IDs for Clips 1, 2, 4, 5, 6
- **Datasheet**: `datasheet.html` is currently empty

## How to preview locally
```
cd "/Volumes/mtsandra-t9/H2H Data/h2himprov.github.io"
python3 -m http.server 8080
```
Then open http://localhost:8080. (VS Code Live Server also works and auto-reloads.)
