# Crew Dispatch — Task Board

A paper-ledger style task board for managing a team's work. Assign tasks with
deadlines, watch the clocks tick, punch in work with a single button, and get
alerted before anything slips. The roster itself is live — add or remove crew
members at any time.

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
whether that finish was **on time** or **late**. The presses needed always
match the assigned length:

| Task length | Presses needed | Progress per press |
|-------------|----------------|--------------------|
| 1 day       | 1 press        | 100%               |
| 3 days      | 3 presses      | ~33%               |
| 5 days      | 5 presses      | 20%                |
| 10 days     | 10 presses     | 10%                |

Effort and time are separate systems — that's the whole point.

---

## The Roster Is Editable

- **Add employees** — the dashed **ADD EMPLOYEE** button below the roster
  opens an inline form (name + role; role is optional and defaults to
  *Crew*). New members join at the bottom of the sheet, ready for assignment.
- **Remove employees** — hover any row and a trash icon appears next to the
  name. Deleting an idle member is one click; deleting someone with an
  active or finished task requires a two-step confirm (the icon turns into a
  pulsing red ✓ for ~2.5 seconds), so you can't lose work in progress by
  accident.
- **Automatic renumbering** — the sheet and crew strip re-flow after every
  change, always reading 01, 02, 03… with no gaps. Internal IDs stay stable,
  so running tasks keep their state even if someone above them is removed.
- **Works with any crew size** — including an empty board, which shows a
  proper empty state.

---

## How It Works

### Assigning a task

- Click **ASSIGN TASK** on any employee row.
- Type the task name and pick a length: **1 / 3 / 5 / 10 days**.
- Hit **START CLOCK** — the countdown begins immediately.

### Doing the work

- Each press of **WORK** punches one tally stroke and advances the progress
  bar (`1 ÷ days` per press).
- The button shows how many presses remain.
- Tally marks render as classic grouped tallies — four verticals + a diagonal
  slash per five strokes — like a real work log on paper.
- The **final press closes the task**, stamped **COMPLETE** — or **LATE** if
  the deadline already passed. The stamp sits rotated across the finished
  tally sheet, like a supervisor's approval on paper.

### Deadlines & alerts

- The countdown runs live on every active row (`D-3 LEFT`, `D-1 LEFT`, …).
- At **D-1** ("the day before the last day"), a notification fires:
  toast popup, bell badge in the header, entry in the alert log, chime sound.
- If the deadline passes unfinished, the row flips to **OVERDUE** with its
  own alert. Work can still be finished — it's just recorded as late, with
  the overrun shown on the row.

### At a glance

- The **crew strip** at the top shows every employee as a cell — hatched
  (active), green (done), red (overdue), dashed (idle). Click a cell to jump
  to that row.
- Header counters track **ACTIVE / DONE / OVERDUE** across the whole board,
  and the kicker shows the current crew size.

---

## Demo Time Scale

> **1 real minute = 1 working day.**

So a "5-day" task plays out in 5:00 minutes — fast enough to exercise every
state (assign → work → alert → complete / overdue) in a single sitting.
Change the `DAY` constant in the script (in milliseconds) to rescale.

A sample task starts automatically on load:
**Employee 01 — "Quarterly report — first draft", 5 days = 5 presses.**

---

## Features

- Editable roster — add/remove employees with safe deletion and renumbering
- Inline task assignment with selectable lengths (1/3/5/10 days)
- Press-to-work progress: strokes = days, bar + tally update instantly
- Live per-task countdown clocks and D-1 / overdue alerts
- Alert log panel with unread badge
- Toast notifications for alerts, completions, overdue tasks, and roster
  changes
- Subtle synthesized sounds (Web Audio API — mutable via the speaker toggle)
- Two-step confirmations for destructive actions (reset, delete with work)
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
- **No frameworks** — state is a simple `Map` keyed by stable employee IDs;
  rendering is template strings
- **Web Audio API** — all ticks, chimes, and completion jingles are
  synthesized, no audio files
- **Inline SVG** — tally marks are generated per-row with a seeded PRNG, so
  each employee's strokes look hand-drawn but stay consistent across renders

---

## Possible Next Steps

- Persist state to `localStorage` so people *and* tasks survive a refresh
- Edit existing crew members (rename / change role), not just add/remove
- Drag-and-drop reordering of the roster
- Configurable deadline scale (days → real days for production use)
- Per-employee keyboard shortcut for the WORK button
````

**To use it:** copy everything between the outer ` ```` ` fences into a file named `README.md` (or just copy starting from `# Crew Dispatch` — the fences were only there to show it as one block). GitHub, GitLab, and VS Code preview will render all of it: the headings, the press-count table, the bash code block, the blockquote, and the hyperlinked font/icon names.
