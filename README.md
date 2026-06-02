# Recovery — 3-Week Plan

A single-file, offline-first web app for following a structured **3-week burnout
recovery plan**. It guides you day by day through rest and a gentle return to
activity, and includes a separate daily **morning recalibration** practice
(cognitive-bias / predictive-processing exercises). Everything is self-contained
in one HTML file, runs entirely in the browser, and stores your data locally.

## Overview

The plan is organized into three week-long phases:

| Week | Title | Days | Focus |
|------|-------|------|-------|
| 1 | Deep Rest | 1–7 | Wake naturally, move gently, let the emergency mode fade |
| 2 | Gentle Reintroduction | 8–14 | A little more movement; one small task a day if it feels manageable |
| 3 | Rebuild Rhythm | 15–21 | A roughly consistent shape to the day; light, contained future-thinking |

Each day is built from four routine sections — **Morning, Midday, Afternoon,
Evening** — plus weekly tasks, reminders, and "markers to notice."

## Features

### Daily tracking (Day view)
- Per-day checklist generated from the plan, grouped by section.
- Optional free-text note on every checklist item (e.g. why something was skipped).
- A daily progress bar (items done / total).
- A check-in with tracked questions: sleep quality, energy 1–10, any skips, synchronicity logger, plus a
  week-specific question.
- A free-text notes field per day.
- Day-to-day navigation with a "jump to today" shortcut. "Today" is computed from
  your chosen start date.

### Week view
- A 7-day grid for each week with per-day completion bars.
- Tap any day to open it.
- A per-week written assessment.

### Overview
- Aggregate stats: days touched, overall percent complete, items checked, and
  per-week averages.
- A whole-plan reflection field.
- Reference lists of what to avoid and what to allow across all three weeks.
- Export and reset controls.

### Morning Recalibration (Morning view)
A separate ~15-minute daily practice, stored independently and anchored to the
first day you use it (its own "practice day" counter). Four exercises plus an
evening close:

1. **Prediction Log** — write three specific, testable predictions for the day;
   mark the outcome (Confirmed / Partial / Wrong) and what actually happened in
   the evening.
2. **Ambiguous Scenarios** — preset reframing scenarios for practice weeks 1 and
   2; from practice day 15 onward you supply your own (trigger → threatening
   interpretation → benign resolution).
3. **Evidence Audit** — a structured six-question walk-through of one
   anxiety-generating situation.
4. **Singing** — a brief vagal-toning reset.
5. **Evening Close** — review your predictions and complete a closing sentence.

Includes its own date navigation, progress bar, and reference notes on why the
exercises work.

### About view
A read-only summary of the plan's arc, available at any time.

## Data & storage

- **Local-first.** All data lives in your browser via `localStorage` (two stores:
  one for the recovery tracker, one for the morning log). No server, no account.
- **Optional file sync.** In browsers that support the File System Access API
  (Chrome and Edge), you can link a JSON file on disk and the app auto-saves every
  change to it. The file handle is persisted across sessions via IndexedDB.
  - The recovery tracker and the morning log each sync to their own separate JSON
    file.
  - If a saved file loses permission (e.g. after a browser restart), a
    **reconnect banner** appears at the top of the page prompting you to grant
    access again.
- **Export / import.** Each store can be exported as a formatted JSON file
  (`burnout_tracker.json` / `morning.json`), and a previously saved file can be
  loaded back in.
- **Reset.** A reset control erases all tracked data and returns to the start
  screen.

## Usage

The app is a single static file with no build step and no dependencies (it loads
a web font from Google Fonts when online, but works without it).

1. Open `index.html` in a browser — directly, or serve it locally:
   ```sh
   python3 -m http.server
   # then visit http://localhost:8000/index.html
   ```
2. On first run, choose **Start Day 1 today**, pick an earlier start date, or load
   an existing saved JSON file.
3. Follow the daily list. Your progress saves automatically.

For auto-sync to a file on disk, use Chrome or Edge; other browsers fall back to
in-browser storage plus manual JSON export.

## Technical notes

- Pure HTML/CSS/vanilla JavaScript in one file — no frameworks, no bundler.
- Plan content (routines, weekly tasks, questions, scenarios, reference text) is
  hardcoded as data structures at the top of the script.
- Stored item IDs are accompanied by a regenerated "item index" legend so saved
  files remain self-describing.
- Includes light data migration/normalization on load (e.g. legacy "skips" are
  folded into per-item notes).
- Serif typography (Spectral) on a warm paper palette, designed to feel calm and
  low-pressure.
