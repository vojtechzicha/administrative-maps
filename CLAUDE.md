# CLAUDE.md

Guidance for future Claude Code sessions working in this repository.

## What this repo is

A **single-file static site** presenting administrative maps of the Czech Republic (14 regions, ~92 ORPs, all from ČSÚ). No framework, no build step — everything lives in `index.html`.

## Architecture

- `index.html` — contains all HTML, CSS, and JavaScript inline. Keep it that way: the site is designed to be trivially portable (open-the-file-and-it-works).
- `maps/` — image assets (PNG + one JPG). Filenames carry structural information that is parsed at runtime by the JS; see "Naming convention" below.
- No bundler, no package manager, no server-side rendering.

## Naming convention (load-bearing)

The JS parses filenames with two regexes:

```
^(.+?) - okres (.+?) - ORP (.+?)\.(png|jpg|jpeg)$   → ORP inside a district
^(.+?)\.(png|jpg|jpeg)$                             → standalone region map
```

**If you add or rename files, preserve this convention** — otherwise they silently disappear from the site (or are mis-categorized). Also keep the filename appended to the `FILES` array in `index.html`; the file list is explicit, not discovered by a directory listing (so the site works from `file://` too).

## Routing

Pure hash routing:
- `#/` — home
- `#/r/<region-slug>` — region detail
- `#/m/<map-slug>` — individual map view

Slugs are generated from names via `String.normalize("NFD")` + ASCII transliteration. Do not hardcode slugs.

## Design system

- **Fonts**: Fraunces (variable, display + body) and JetBrains Mono (labels, metadata). Loaded from Google Fonts.
- **Colors**: defined as CSS custom properties in `:root`. Cream paper (`--bg`), deep ink (`--ink`), crimson accent (`--accent`).
- **Aesthetic direction**: editorial atlas / cartographic. When extending the UI, stay in that register — avoid generic "tech dashboard" patterns.

## Things to be careful about

- **Filename diacritics.** Images are referenced via `encodeURIComponent`-ed paths. Do not strip diacritics from the on-disk filenames; do not manually pre-encode the `FILES` array entries.
- **Two known filename typos are preserved intentionally** because they match actual files on disk: `ORP Skolov.png` (should be Sokolov) and `ORP Mladá Bolesllav.png` (extra "l"). If you fix the files, fix both the filename and the `FILES` entry.
- **Map image copyright.** Images are © ČSÚ. They are reproduced for overview purposes. Keep the ČSÚ attribution visible both on the site (footer + each map caption) and in `README.md`.

## Testing a change

```bash
python -m http.server 8000
# open http://localhost:8000
```

Walk the full flow: home → pick a region → pick an ORP → view map → back. Try the `/` search with diacritic-free input (e.g. "ricany" should match "Říčany").

## Do NOT

- Do not introduce a build step, a framework, or dependencies without a strong reason.
- Do not break the single-file portability (`file://` open should keep working).
- Do not commit the ČSÚ attribution away.
