# Smart Escape

**Participant:** Mohammad Adib Abtahi
**Registration:** aif_452928d646c443ada0ed
**Live site:** https://adib18804.github.io/devfest-aif_452928d646c443ada0ed/
**Repository:** https://github.com/Adib18804/devfest-aif_452928d646c443ada0ed



## What it is

Smart Escape is a **frontend-only** browser tool that visualises a building map and
computes the **lowest-cost route** from a chosen start location (room or junction)
to any open exit. Hazards — blocked nodes, blocked corridors, and closed exits —
can be toggled live, and the route recalculates instantly using **Dijkstra's
shortest-path algorithm** with the exact tie-break rules specified in the problem
statement.

This is an educational simulation, **not** a certified real-world evacuation tool.

## Live demo

Open **https://adib18804.github.io/devfest-aif_452928d646c443ada0ed/** in the
latest Chrome. No login, no installation.

## Running locally

```bash
git clone https://github.com/Adib18804/devfest-aif_452928d646c443ada0ed.git
cd devfest-aif_452928d646c443ada0ed
python -m http.server 8080      # or: npx serve .
```

Then open <http://localhost:8080>.

No build step, no dependencies, no backend — pure HTML + CSS + JavaScript.

## How to use

| Action | Result |
|---|---|
| **Click** a room/junction | Set it as the start |
| **Click** the start again | Clear the start |
| **Click** any other node | Toggle it blocked / unblocked |
| **Click** a corridor | Toggle it blocked / unblocked |
| **Right-click** an exit | Close / reopen it |
| **Right-click** a non-exit node | Toggle blocked |
| **Reset** button | Restore the file's `initial_state` |
| **Clear Start** button | Remove the current start |
| **Sample** button | Reload `building.json` from the repo |
| **English / বাংলা** | Switch UI language |
| **🌗** | Toggle light / dark theme |
| **◐** | Toggle high-contrast accessibility mode |
| **🖼️ PNG / ⬇️ SVG** | Export the current map |
| `R` | Reset |
| `Esc` | Clear start |
| `E` / `B` | Switch to English / Bangla |

## Implemented features

### Core (mandatory)
- [x] Local `building.json` import via file picker + auto-load of the sample file
- [x] Full input validation with clear error messages (schema, IDs, types, ranges, category checks)
- [x] SVG map render at dataset coordinates, distinct node types, readable labels, visible edge costs
- [x] Dijkstra shortest path, cost = sum of edge weights (coordinates and hop count are ignored)
- [x] Excludes blocked nodes + their incident edges, blocked edges, and closed exits (including as intermediates)
- [x] Tie-break: minimum cost → lexicographically smallest exit ID → lexicographically smallest node-ID sequence
- [x] Instant recalculation on every start or hazard change (no reimport needed)
- [x] "No route available" and "Starting location blocked" states
- [x] Reset restores the original `initial_state`
- [x] Subtle animations: route pulse, start pulse, hover transitions
- [x] Bangla + English UI for every label, button, status, error, and instruction
- [x] Distinct visuals: room / junction / exit / start / blocked node / closed exit / blocked corridor / route

### Bonus (Section 4.2 optional)
- [x] **Alternative routes panel** — every reachable open exit ranked by cost then exit ID
- [x] **PNG export** — rasterised map (2× resolution)
- [x] **SVG export** — vector map
- [x] **LocalStorage persistence** — start location and active hazards survive reload
- [x] **High-contrast mode** — accessibility toggle
- [x] **Light / dark theme**
- [x] **Keyboard shortcuts** — `R` reset, `Esc` clear start, `E`/`B` language
- [x] **Copy route to clipboard**
- [x] **Live stats bar** — node / edge / hazard counts
- [x] **Algorithm runtime badge**
- [x] **First-visit onboarding hint**

## Screenshots

| Baseline | Blocked C2 (reroute) |
|---|---|
| ![Baseline](screenshots/01-baseline.png) | ![Blocked C2](screenshots/02-blocked-c2.png) |

| No route available | Bangla UI |
|---|---|
| ![No route](screenshots/03-no-route.png) | ![Bangla](screenshots/04-bangla.png) |

## Sample checks (Section 4.1)

| Scenario | Action | Result |
|---|---|---|
| Baseline | Select **R1** | `R1 → C1 → C2 → E1`, cost **7** ✅ |
| Blocked junction | Select **R1**, block **C2** | `R1 → C1 → C3 → C4 → E2`, cost **11** ✅ |
| Exits closed | Select **R1**, close **E1** and **E2** | *No route available* ✅ |
| Different start | Select **R2** | `R2 → C3 → C4 → E2`, cost **7** ✅ |
| Blocked start | Select **R1**, then block **R1** | *Starting location blocked* ✅ |

## Tech

- **Language:** vanilla HTML + CSS + JavaScript (no framework, no bundler)
- **Render:** inline SVG
- **Algorithm:** Dijkstra with explicit lexicographic tie-breaking
- **Persistence:** `localStorage`
- **Hosting:** GitHub Pages
- **No backend, no remote storage, no external APIs**

## Known issues

- The **Sample** button needs the page served over HTTP (not `file://`) because browsers block `fetch` on `file://`. Use the file picker as a fallback.
- Very large graphs (close to the 60-node limit) render at a fixed viewBox; use browser zoom if needed.

## AI tools and best prompt

- **AI tool used:** Claude (Anthropic) — official AI DevFest workflow
- **Most useful prompt:**

  > "Build a single-file HTML/CSS/JS frontend for a weighted-graph escape-route tool. It must load a `building.json`, validate it thoroughly, render an SVG map at the given coordinates, run Dijkstra with the tie-break rules (min cost → lexicographically smallest exit ID → lexicographically smallest node-ID sequence), support live hazard toggling without reimport, show 'No route available' / 'Starting location blocked', provide English + Bangla UI, and animate route updates. Then extend it with an alternative-routes panel, PNG/SVG export, localStorage save, high-contrast mode, keyboard shortcuts, and a first-visit onboarding hint. No backend, no build step, offline-capable."

## License

MIT — see [LICENSE](./LICENSE).
