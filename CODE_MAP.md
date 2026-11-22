# Code map — Investor view

**Files:** `index.html` (page text and layout) · `style.css` (looks) · `data.js` (the numbers) · `main.js` (all the D3 code, 369 lines) · `writeup.js` (text for the write-up sections; templates only).

## The idea in one paragraph
Everything on the page is drawn from **one list of 140 months** (`P`, built from `data.js`). Three variables decide what is shown:
**`win`** = the selected window (set by the brush or the period buttons), **`head`** = the last month drawn (moved by playback or jumping), **`focus`** = the month under the pointer or playhead (or `null`).
Whenever one of them changes, `render()` redraws the chart and `updatePanels()` refreshes the dial, notes, tape, readouts and overview corner, so nothing can go stale.

## main.js sections
| # | Lines | What it does |
|---|---|---|
| 1 | 2–37 | Builds `P` (one object per month), 3-month smoothing (`smooth`, line 16), the five market phases |
| 2 | 39–58 | State variables and the scales (rate→x, sales→y, month→x on the overview, ratio→line width) |
| 3 | 60–131 | The connected scatterplot: axes, textbook arrow, `render()` (line 101) draws segments, dots, year labels |
| 4 | 133–186 | The Governor's desk: dial (`moveDial`, line 152), 100 notes (`drawNotes`, 168), ticker tape (`drawTape`, 176) |
| 5 | 188–236 | Overview: 100% stacked area, phase ribbon, brush (line 209), `setWindow` (223), draggable playhead (231) |
| 6 | 238–267 | `updatePanels()` (line 243): the one function that keeps every panel in sync |
| 7 | 269–343 | Hover and click-to-jump (line 272), `scrub` (316), playback `tick`/`play` (325/332), buttons |
| 8 | 345–355 | Legend |
| 9 | 357–369 | Start-up and the background check against the live CSV (line 365) |

## Likely demo questions → where to look
- **What is plotted?** One dot per month: x = policy rate, y = sales (3-month mean). `render()`.
- **What does line width mean?** Mortgage registrations ÷ sales (`p.q`), mapped by `widthOf` in section 2.
- **How does the brush work?** `d3.brushX` sets `win`; `render()` only draws months inside it.
- **How does playback work?** A timer calls `tick()` which adds one month to `head`; `scrub()` sets `head` directly (click a point or drag the ▼).
- **Why can't the corner value go stale?** Everything goes through `updatePanels()`; a new window clears `focus`.
- **Where do rates come from / how joined?** `D.r` in `data.js`, one rate per month, joined to the sales counts by year-month (see the page's Data section).
- **Why is the overview not raw counts?** It shows the mortgage ratio so it does not look like Part 2's monthly sales line.

## Removed on purpose
The guess-and-reveal game and the velocity colouring were removed to keep the code short.
