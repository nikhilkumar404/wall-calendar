# Wall Calendar

An interactive wall calendar with date range selection, month navigation, and per-range notes. Built as a single self-contained HTML file — no installs, no build step, no dependencies.

**Live demo:** https://nikhilkumar404.github.io/wall-calendar/

---

## Running locally

Clone the repo and open `index.html` directly in your browser:

```bash
git clone https://github.com/nikhilkumar404/wall-calendar.git
cd wall-calendar
# open index.html in your browser
```

No server needed. Keep `index.html` and the hero image (`calendar_hero_1775709739543.png`) in the same folder.

---

## Features

- Monthly calendar grid, Monday-first
- Click a date to start a range, click again to end it
- Hover preview shows the range before you confirm
- Today highlighted separately from the selection
- Weekends styled differently from weekdays
- Quick selectors: This week / Next week / This month
- Notes scoped to the selected range or the current month
- All state (range + notes) persists across reloads via `localStorage`
- Responsive — stacks on mobile, fits the viewport on desktop without scrolling

---

## Tech

Plain HTML, CSS, and JavaScript. No frameworks, no build tooling.

---

## Design decisions

**Why no framework?**  
The state is simple — a single JS object with `curYear`, `curMonth`, `rangeStart`, `rangeEnd`. A plain `render()` that rebuilds the DOM on state change is easy to follow and debug. Adding React would mean a build step and a `node_modules` folder for something that doesn't need it.

**Viewport-fit layout**  
The card uses CSS flexbox with `flex: 1` on the body and `grid-template-rows: repeat(6, 1fr)` on the calendar grid, so rows expand to fill whatever height is available. This keeps the whole calendar visible without scrolling on most screens.

**Range hover without flicker**  
While picking the end date, hovering previews the range. An earlier approach rebuilt the entire grid DOM on every `mousemove` — this caused a flicker loop because replacing DOM triggered `mouseleave`, which re-rendered, and so on. The fix is to stamp each day cell with a `data-date` attribute at initial render, then the hover handler just walks existing elements and swaps CSS classes. No DOM rebuilding during hover.

**Notes keyed by range**  
Each note is stored in `localStorage` under a key derived from the selected date range (or current month). Switching ranges or tabs loads the right note automatically without any server.

**Responsive**  
On screens ≤ 768px, the two-column layout (calendar + notes) stacks vertically, padding is reduced, and the hero image is shorter. The calendar grid still uses `grid-template-rows: repeat(6, 1fr)` but within a `height: auto` container so it doesn't overflow.
