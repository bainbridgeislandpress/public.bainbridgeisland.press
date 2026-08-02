# public.bainbridgeisland.press

Public art site for [Bainbridge Island Press](https://bainbridgeisland.press): **Scotch Broom** poetry broadsides and **POETICS** zines.

## Site structure

| Path | Purpose |
|------|---------|
| `/` | Home — latest Scotch Broom broadside + latest POETICS zine |
| `/scotch-broom/` | Project page + full broadside gallery (assets live here) |
| `/poetics-zine/` | Project page + full zine gallery (assets live here) |

Each project directory holds its own `index.html` plus the poster/zine source files.

## Adding a new issue

Drop files into the right project folder with the naming convention below, optionally add metadata, then either:

```bash
python3 scripts/build_galleries.py
```

…or push to `main` and the **Update galleries** GitHub Action will rebuild the HTML automatically.

### Scotch Broom (`scotch-broom/`)

| File | Role |
|------|------|
| `SBPoster-{MMM}{YY}.jpg` | Gallery thumbnail (recommended) |
| `SBPoster-{MMM}{YY}.png` | Full-size image (linked from the card) |
| `SBPoster-{MMM}{YY}-v{N}.jpg` / `.png` | Versioned revision (highest `vN` wins) |
| `ScotchBroom-{MMM}{YY}.pdf` | Optional PDF download |

**Month codes:** `JAN` `FEB` `MAR` `APR` `MAY` `JUN` `JUL` `AUG` `SEP` `OCT` `NOV` `DEC`  
**Year:** two digits (`26` → 2026)

Examples:

```
scotch-broom/SBPoster-SEP26.jpg
scotch-broom/SBPoster-SEP26.png
scotch-broom/ScotchBroom-SEP26.pdf
```

Optional metadata in `data/scotch-broom.meta.json` (key = `MMMYY`):

```json
{
  "SEP26": {
    "artist": "Poet Name",
    "title": "Poem Title",
    "label": "Poet Name"
  }
}
```

- `label` — subtitle under the month (defaults to `artist` if omitted)
- `title` / `artist` — used in image `alt` text

### POETICS zines (`poetics-zine/`)

| File | Role |
|------|------|
| `POETICS-Zine-{MM}{YY}.jpg` | Cover image |
| `POETICS-Zine-{MM}{YY}.pdf` | Downloadable issue (linked from the card) |

**Month:** `01`–`12` · **Year:** two digits

Examples:

```
poetics-zine/POETICS-Zine-0926.jpg
poetics-zine/POETICS-Zine-0926.pdf
```

Optional metadata in `data/poetics.meta.json` (key = `MMYY`):

```json
{
  "0926": {
    "label": "Issue 02",
    "issue": 2,
    "title": "POETICS Zine"
  }
}
```

## Build script

`scripts/build_galleries.py` scans `scotch-broom/` and `poetics-zine/`, merges optional metadata, and rewrites only the marked regions in:

- `index.html` — latest issue of each project  
- `scotch-broom/index.html` — full Scotch Broom gallery  
- `poetics-zine/index.html` — full POETICS gallery  

Markers look like:

```html
<!-- LATEST:SCOTCH-BROOM:START -->
…
<!-- LATEST:SCOTCH-BROOM:END -->
```

Hand-edit project descriptions and shared layout freely; keep those comment markers in place so the script can update galleries.

## GitHub Action

`.github/workflows/update-galleries.yml` runs on pushes to `main` that touch `scotch-broom/`, `poetics-zine/`, `data/`, or the build script. It runs the script and commits any HTML changes.
