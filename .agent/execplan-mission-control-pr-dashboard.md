# ExecPlan: Mission Control — Pull Requests Dashboard

## Goal
Add a Pull Requests page to the OpenClaw Mission Control Dashboard showing open and recently merged PRs across all projects from `~/.openclaw/projects.json`.

## Data Sources
- `~/.openclaw/projects.json` — project registry with `ghRepo` fields
- `gh pr list -R <repo> --state open --json ...` — open PRs per project
- `gh pr list -R <repo> --state merged --json ...` — recently merged PRs

## Milestones

### M1: Backend `/api/prs` endpoint [server.js]
- Add `GET /api/prs` before the Task Board API block (~line 3012)
- For each project with a `ghRepo`, run parallel `gh pr list` for open + recently merged
- Open PR fields: `number, title, author, headRefName, createdAt, updatedAt, labels, reviewDecision, isDraft`
- Merged PR fields: `number, title, author, headRefName, mergedAt`
- Return: `{ projects: [{ name, ghRepo, open, merged }], totalOpen }`

### M2: Frontend nav item + page HTML/CSS [index.html]
- Add nav item `data-page="prs"` with icon `🔀` and label "Pull Requests" after `docs` nav item
- Add `<div class="page" id="prs">` before `</main>` (after team page)
- CSS: `.pr-summary-bar`, `.pr-project-section`, `.pr-table`, `.pr-row`, `.pr-status-badge`
- Wire `fetchPRs()` in nav click handler (after the `if (page === 'docs')` block)

### M3: Frontend JS [index.html]
- `fetchPRs()` — calls `/api/prs`, renders the page
- `renderPRs(data)` — builds summary bar + per-project PR sections
- `buildPRRow(pr)` — renders one PR row with badge, title, author, age, labels
- `prFilterState` — tracks active filter (all/needs-review/draft/approved/merged)
- Filter buttons: All / Needs Review / Draft / Changes Requested

## Progress
- [x] M1: Backend /api/prs endpoint
- [x] M2: Nav item + page HTML/CSS
- [x] M3: Frontend JS
