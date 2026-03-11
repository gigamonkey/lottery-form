# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Clasp Commands

clasp is installed locally. Use `./node_modules/.bin/clasp` (or `npx clasp`):

```bash
./node_modules/.bin/clasp pull      # pull latest from Apps Script
./node_modules/.bin/clasp push      # push local changes to Apps Script
./node_modules/.bin/clasp open      # open project in Apps Script editor
./node_modules/.bin/clasp deploy    # create a new deployment
```

There is no build step or test suite — changes are pushed directly to Google Apps Script and tested live.

## Architecture

This is a Google Apps Script web app — a single-page form for BHS students to rank Learning Communities (LCs) for the Rising 10th Grade Lottery.

**`Code.js`** — Server-side (Apps Script runtime):
- `doGet()` — serves the HTML page
- `saveRankings(data)` — appends a row to sheet 0 of the bound spreadsheet; uses `LockService` to prevent concurrent writes
- `getRankings(studentEmail)` — reads the most recent row for a given email from the sheet
- `whoami()` — returns the active user's email

**`index.html`** — Client-side (all JS is inline at the bottom):
- Calls server functions via `google.script.run.withSuccessHandler(...).fnName(args)`
- Two modes based on the logged-in user's email domain:
  - `@students.berkeley.net` — student mode: load own rankings, submit own form
  - `@berkeley.net` — staff/adult mode: shows extra fields to submit on behalf of a student
- LCs are drag-and-drop tiles. AC and BIHS are **mandatory** (must be ranked); AHA, AMPS, CAS are optional small schools
- `FORM_CLOSES` constant controls when the form locks (currently set to Feb 19, 2026 at 4pm PT)

**Spreadsheet schema** (sheet 0, one row per submission):
`timestamp | submitter_email | student_email | rank1 | rank2 | rank3 | rank4 | rank5 | student_id | reason`
