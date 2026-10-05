# Smart Escape — AI DevFest Mock Test

**Participant:** YOUR FULL NAME
**Registration:** YOUR-REGISTRATION-NUMBER
**Live site:** https://YOUR-USERNAME.github.io/devfest-YOUR-REGISTRATION-NUMBER/
**Repository:** https://github.com/YOUR-USERNAME/devfest-YOUR-REGISTRATION-NUMBER

---

## What it is

Smart Escape is a browser-only tool that visualises a building map and computes the
lowest-cost escape route from a selected start (room or junction) to any open exit.
Hazards (blocked nodes, blocked corridors, closed exits) can be toggled live, and the
route recalculates instantly using **Dijkstra's algorithm** with the exact tie-break
rules specified in the problem statement.

## Running locally

1. Clone the repository.
2. Serve the folder over HTTP (fetch needs a real origin):

   ```bash
   python3 -m http.server 8080
   # or
   npx serve .
   ```

3. Open <http://localhost:8080/> in Chrome.

No build step, no dependencies, no backend.

## How to use

| Action | Result |
|---|---|
| **Click** a room/junction | Set it as the start |
| **Click** the start again | Clear the start |
| **Click** any other node | Toggle it blocked/unblocked |
| **Click** a corridor | Toggle it blocked/unblocked |
| **Right-click** an exit | Close/reopen it |
| **Right-click** a non-exit node | Toggle blocked |
| **Reset** button | Restore the file's `initial_state` |
| **Clear Start** button | Remove the current start |
| **Sample** button | Re-load `building.json` from the repo |
| **English / বাংলা** | Switch UI language |

## Implemented features

- [x] Local `building.json` import (file picker) + auto-load of the sample file.
- [x] Full input validation with clear error messages (schema, IDs, types, ranges, categories).
- [x] SVG map render at dataset coordinates with distinct node types, visible labels, and edge costs.
- [x] Dijkstra shortest path with cost = sum of edge weights.
- [x] Exclusions: blocked nodes and their incident edges, blocked edges, closed exits (including as intermediates).
- [x] Tie-break: minimum cost → lexicographically smallest exit ID → lexicographically smallest node-ID sequence.
- [x] Instant recalculation on every start or hazard change (no reimport).
- [x] "No route available" and "Starting location blocked" states.
- [x] Reset restores the original `initial_state`.
- [x] Subtle animations: route pulse, start pulse, hover transitions.
- [x] Bangla + English UI for all labels, buttons, statuses, errors, and instructions.
- [x] Distinct visuals: room / junction / exit / start / blocked node / closed exit / blocked corridor / route.

## Optional / bonus

- [ ] Alternative routes panel
- [ ] High-contrast / accessibility mode
- [ ] PNG export
- [ ] Save progress (localStorage)
- [ ] Route walkthrough animation

## Known issues

- The **Sample** button requires the page to be served over HTTP (not `file://`), because browsers block `fetch` on `file://`. Use the file picker if serving locally isn't an option.
- Graph rendering does not auto-zoom for very large datasets; the SVG scales to fit the container.

## AI tools and best prompt

- **AI tool used:** Claude (Anthropic) via the official AI DevFest workflow.
- **Most useful prompt:**
  > "Build a single-file HTML/CSS/JS frontend for a weighted-graph escape-route tool. It must load a `building.json`, validate it thoroughly, render an SVG map at the given coordinates, run Dijkstra with the tie-break rules (min cost, then lexicographically smallest exit ID, then lexicographically smallest node-ID sequence), support live hazard toggling without reimport, show 'No route available' / 'Starting location blocked', provide English + Bangla UI, and animate route updates. No backend, no build step, offline-capable."

## License

MIT — see [LICENSE](./LICENSE).