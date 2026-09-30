# Crew Dispatch — Task Board

A paper-ledger style task board for a 10-person team. Assign tasks with
deadlines, watch the clocks tick, punch in work with a single button, and get
alerted before anything slips.

Built as a single self-contained HTML file — no build step, no server, no
dependencies beyond a font and icon CDN. Open it in a browser and it runs.

---

## The Idea

Every task has two independent systems running in parallel:

1. **The deadline clock** — real time counts down from the moment a task
   starts. Time pressure is automatic.
2. **The work log** — progress only moves when someone actually does the
   work. One press of the **WORK** button = one day of work done.

A task is only *complete* when the work is finished — the clock just decides
whether that finish was **on time** or **late**. A 5-day task needs 5 presses.
A 10-day task needs 10. Effort and time are separate, which is the whole point.

---

## How It Works

### Assigning a task
- Click **ASSIGN TASK** on any employee row.
- Type the task name and pick a length: **1 / 3 / 5 / 10 days**.
- Hit **START CLOCK** — the countdown begins immediately.

### Doing the work
- Each press of **WORK** punches one tally stroke and advances the progress
  bar by `1 ÷ days` (a 5-day task moves 20% per press).
- The button shows how many presses remain.
- Tally marks render as classic grouped tallies (four verticals + a slash per
  five strokes) — like a real work log on paper.
- The **final press closes the task**, stamped **COMPLETE** — or **LATE** if
  the deadline already passed.

### Deadlines & alerts
- The countdown runs live on every active row (`D-3 LEFT`, `D-1 LEFT`, …).
- At **D-1** ("the day before the last day"), a notification fires:
  toast popup, bell badge in the header, entry in the alert log, chime sound.
- If the deadline passes unfinished, the row flips to **OVERDUE** with its own
  alert. Work can still be finished — it's just recorded as late, with the
  overrun shown on the row.

### At a glance
- The **crew strip** at the top shows all 10 employees as cells — hatched
  (active), green (done), red (overdue), dashed (idle). Click a cell to jump
  to that row.
- Header counters track **ACTIVE / DONE / OVERDUE** across the whole board.

---

## Demo Time Scale

> **1 real minute = 1 working day.**

So a "5-day" task plays out in 5:00 minutes — fast enough to exercise every
state (assign → work → alert → complete / overdue) in a single sitting.
Change the `DAY` constant in the script (in milliseconds) to rescale.

A sample task starts automatically on load:
**Employee 01 — "Quarterly report — first draft", 5 days.**

---

## Features

- 10-employee roster with inline task assignment
- Live per-task countdown clocks and D-1 / overdue alerts
- Press-to-work progress: strokes = days, bar + tally update instantly
- Alert log panel with unread badge
- Toast notifications for alerts, completions, and overdue tasks
- Subtle synthesized sounds (Web Audio API — mutable via the speaker toggle)
- Two-step reset (no browser `confirm()` popups)
- Fully responsive — rows reflow to a stacked layout on mobile
- Everything runs locally; session-only, nothing leaves the page

---

## Running It

No install, no build:

```bash
# just open the file
open index.html        # macOS
# or double-click it on any OS
```

Requires an internet connection on first load for the
[Archivo](https://fonts.google.com/specimen/Archivo) /
[IBM Plex Mono](https://fonts.google.com/specimen/IBM+Plex+Mono) fonts and
[Lucide](https://lucide.dev) icons (both via CDN). Everything else is
self-contained.

---

## Tech Notes

- **Single file** — HTML, CSS, and vanilla JS in one document
- **No frameworks** — state is a simple `Map`, rendering is template strings
- **Web Audio API** — all ticks, chimes, and completion jingles are
  synthesized, no audio files
- **Inline SVG** — tally marks are generated per-row with a seeded PRNG, so
  each employee's strokes look hand-drawn but stay consistent across renders

---

## Possible Next Steps

- Persist state to `localStorage` so the board survives a refresh
- Per-employee keyboard shortcut for the WORK button
- Configurable deadline scale (days → real days for production use)
- Editable crew roster instead of the fixed 10 names
