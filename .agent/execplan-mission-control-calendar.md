# ExecPlan: Mission Control — Calendar Screen (Cron Visualization)

## Goal
Add a Calendar page to the OpenClaw Mission Control Dashboard showing all cron jobs
in a weekly timeline heatmap plus a detailed job table with toggle/run actions.

## Data Sources
- `/api/crons` endpoint — reads `~/.openclaw/cron/jobs.json`
- Existing `/api/cron/:id/toggle` and `/api/cron/:id/run` POST endpoints

## Schedule Formats in Data
- `{ kind: "every", everyMs: N, anchorMs: N }` — interval-based (e.g. every 1h, 6h)
- `{ kind: "cron", expr: "0 9 * * *", tz: "America/Detroit" }` — standard cron expr

## Milestones

### M1: Enhance `getCronJobs()` in server.js
- Fix schedule label for `kind:"every"` jobs (was returning empty string)
- Add `scheduleRaw: { kind, everyMs, expr, tz, anchorMs }` field to each job
- This raw data is needed by the frontend to place jobs in the weekly grid

### M2: Calendar nav item [index.html]
- Add `<div class="nav-item" data-page="calendar">` (icon `📅`) after Projects nav item
- Wire `fetchCalendar()` in the nav click handler (`if (page === 'calendar')`)

### M3: Calendar page CSS [index.html]
- `.cal-stats`, `.cal-stat-card`, `.cal-stat-value`, `.cal-stat-label`
- `.cal-legend`, `.cal-legend-item`, `.cal-legend-dot`
- `.cal-week-wrap`, `.cal-week-header`, `.cal-week-day-hdr`, `.cal-week-body`
- `.cal-week-row`, `.cal-hour-lbl`, `.cal-cell`, `.cal-dot`
- `.cron-tbl`, `.cbadge` variants, `.cron-btn`

### M4: Calendar page HTML + JS [index.html]
HTML (after projects `</div>`):
- `<div class="page" id="calendar">` with stats bar, legend, weekly grid, cron table

JS functions:
- `fetchCalendar()` — fetches `/api/crons`, stores in `calCrons[]`, calls `renderCalendar()`
- `renderCalendar()` — calls all three sub-renderers
- `renderCalStats()` — total / enabled / disabled / ran-24h / errors stats bar
- `expandCronField(field, min, max)` — parses cron field syntax to array of ints
- `calJobHoursForDay(job, dow)` — returns hours a job fires on day-of-week 0-6
- `renderCalWeekGrid()` — 7-day × 24-hour grid with colored dots per job
- `renderCalCronTable()` — table with name, schedule, badges, last/next run, toggle/run
- `calToggleCronJob(id, btn)` — POST toggle, re-render calendar
- `calRunCronJob(id, btn)` — POST run-now with loading state

## Progress
- [x] M1: getCronJobs() enhanced with scheduleRaw + fixed interval labels
- [x] M2: Nav item + click handler
- [x] M3: CSS
- [x] M4: Page HTML + JS
