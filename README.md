# Recovery — 3-Week Plan

A single-file, offline-first web app for following a structured **3-week burnout
recovery plan**. It guides you day by day through rest and a gentle return to
activity, and includes a separate daily **morning recalibration** practice
(cognitive-bias / predictive-processing exercises). Everything is self-contained
in one HTML file, runs entirely in the browser, and stores your data locally —
with optional private cloud sync so your progress follows you across devices.

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
first day you use it (its own "practice day" counter). The exercises, plus an
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
5. **Seven Sentences** — *(practice week 2 onward)* a fixed set of seven
   reframing sentences to sit with morning and evening, with a separate checkbox
   for each sitting. Counts toward the day's completion once both are ticked.
6. **Evening Close** — review your predictions and complete a closing sentence.

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
- **Cloud sync (optional).** Sign in with **GitHub** to sync both stores
  privately across devices (e.g. laptop and phone) — useful since the File System
  Access API is desktop-only. Sync is backed by [Supabase](https://supabase.com):
  - Each dataset is stored as a private per-user row in an `app_data` table,
    protected by row-level security, so your entries are **never** part of the
    public site or repository.
  - On sign-in the app pulls your remote data (seeding the cloud from the current
    device if the cloud is empty); thereafter every change is auto-saved
    (debounced). Conflicts resolve last-write-wins per dataset.
  - A status badge in the top bar shows sync state (`☁✓` / `☁⋯` / `☁⚠`). Sign-in
    is available from both the **About** panel and the start screen.
  - The Supabase project URL and public anon key ship in the client by design;
    data access is gated by login + row-level security, not by key secrecy.
- **Export / import.** Each store can be exported as a formatted JSON file
  (`burnout_tracker.json` / `morning.json`). An **Import JSON** button next to
  each Export button loads a saved file back in using a standard file picker that
  works on **any browser, including mobile** (importing replaces that browser's
  data and, when signed in, pushes the result to the cloud).
- **Reset.** A reset control erases all tracked data and returns to the start
  screen.

## Usage

The app is a single static file with no build step. It works fully offline; when
online it loads a web font from Google Fonts and, for optional cloud sync, the
Supabase JS client from a CDN — neither is required for local use.

1. Open `index.html` in a browser — directly, or serve it locally:
   ```sh
   python3 -m http.server
   # then visit http://localhost:8000/index.html
   ```
2. On first run, choose **Start Day 1 today**, pick an earlier start date, load an
   existing saved JSON file, or **sign in with GitHub** to restore data synced
   from another device.
3. Follow the daily list. Your progress saves automatically.

For auto-sync to a file on disk, use Chrome or Edge; other browsers fall back to
in-browser storage plus JSON export/import. To carry data between devices (or onto
mobile), use **Cloud sync**, or move the exported JSON to the other device and use
**Import JSON**.

## Technical notes

- Pure HTML/CSS/vanilla JavaScript in one file — no frameworks, no bundler. The
  only runtime dependency is the Supabase JS client (loaded from a CDN), used
  solely for optional cloud sync.
- Plan content (routines, weekly tasks, questions, scenarios, reference text) is
  hardcoded as data structures at the top of the script.
- Stored item IDs are accompanied by a regenerated "item index" legend so saved
  files remain self-describing.
- Includes light data migration/normalization on load (e.g. legacy "skips" are
  folded into per-item notes).
- Serif typography (Spectral) on a warm paper palette, designed to feel calm and
  low-pressure.
