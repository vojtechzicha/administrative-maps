# Správní mapy ČR

A single-file static site presenting administrative maps of the Czech Republic — all 14 regions (*kraje*) and 92 municipalities with extended competence (*obce s rozšířenou působností*, ORP) organised by district (*okres*).

**[→ Open the site](./index.html)** · no build step, no dependencies, open `index.html` in any browser.

## Features

- **Homepage** — editorial masthead with a grid of all 14 regions.
- **Command-palette search** — press <kbd>/</kbd> (or <kbd>Ctrl</kbd>/<kbd>⌘</kbd>+<kbd>K</kbd>) from anywhere to search across regions, districts, and ORPs. Diacritic-insensitive fuzzy match. Arrow keys to navigate, <kbd>↵</kbd> to open.
- **Region detail** — overview map of the region (where available) plus ORPs grouped by district.
- **Map detail** — full-resolution image with breadcrumb trail, previous / next ORP navigation, and click-to-zoom lightbox.
- **Hash routing** — every view has a shareable URL (`#/`, `#/r/<region>`, `#/m/<map>`).

## Design

Editorial atlas aesthetic: cream paper ground with subtle grain & grid textures, deep ink navy, cartographic crimson accent. Typography pairs **Fraunces** (variable serif, used across display and body) with **JetBrains Mono** for metadata and labels. Staggered card reveals on load, smooth view transitions.

## Running locally

The site is entirely static. Either:

```bash
# open the file directly
open index.html        # macOS
start index.html       # Windows
xdg-open index.html    # Linux
```

or serve it via any static HTTP server — handy for sharing across a network:

```bash
python -m http.server 8000
# then open http://localhost:8000
```

## Project structure

```
administrative-maps/
├── index.html        # the entire application (HTML + CSS + JS inline)
├── maps/             # PNG/JPG map images, 100 files total
├── CLAUDE.md         # guidance for Claude Code sessions on this repo
├── LICENSE           # MIT (applies to code, not to the map images)
└── README.md
```

Map files follow a consistent naming convention parsed at runtime:

- `<Region>.png` — standalone region overview map
- `<Region> - okres <District> - ORP <ORP>.png` — ORP map inside a district

Adding a new map is just a matter of dropping a correctly-named file into `maps/` and appending its filename to the `FILES` array in `index.html`.

## Map attribution

> **Map images © [Český statistický úřad (ČSÚ)](https://www.czso.cz/) — Czech Statistical Office.**
>
> The administrative boundary maps included in `maps/` are the copyrighted property of ČSÚ. They are reproduced here for overview and educational purposes only. All rights to the underlying cartographic material remain with ČSÚ; please refer to ČSÚ's [terms of use](https://www.czso.cz/csu/czso/podminky_zverejnovani_a_uziti_udaju) for any reuse of the images beyond personal browsing.
>
> If you are a rightsholder and would like an image removed or the attribution adjusted, please open an issue.

## License

The **site code** (`index.html`, styling, documentation) is released under the [MIT License](./LICENSE).

The **map images** under `maps/` are **not** covered by the MIT license — they remain the property of ČSÚ. See *Map attribution* above.

## Acknowledgements

- Maps by [Český statistický úřad](https://www.czso.cz/).
- Typography via Google Fonts: [Fraunces](https://fonts.google.com/specimen/Fraunces), [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono).
- Design & implementation by [Vojtěch Zicha](https://github.com/vojtechzicha) with [Claude Code](https://claude.com/claude-code).
