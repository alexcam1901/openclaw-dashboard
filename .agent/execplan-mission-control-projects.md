# ExecPlan: Mission Control — Projects Screen

## Goal
Add a Projects page to the OpenClaw Mission Control Dashboard showing all projects from `~/.openclaw/projects.json` with live GitHub stats, git activity, and Obsidian docs.

## Data Sources
- `~/.openclaw/projects.json` — project registry
- `~/Documents/ObsidianVault/projects/*/context.md` and `status.md` — project docs
- `git -C <repo> log` — last commit
- `gh pr list` / `gh issue list` — open PRs and issues

## Milestones

### M1: Backend helpers + `/api/projects` [server.js]
- Add `execPromise(cmd, opts)` helper wrapping `exec` as a Promise
- Add `readProjectsConfig()` helper reading `~/.openclaw/projects.json`
- Add `GET /api/projects` endpoint returning array of project objects with:
  - `name`, `repo`, `worktrees`, `obsidian`, `defaultBranch`, `ghRepo`
  - `lastCommit`: `{ hash, subject, age, author }`
  - `openPRs`, `openIssues` (from `gh` CLI)
  - `contextMd`, `statusMd` (raw markdown strings)
  - `worktreeCount`

### M2: Frontend nav item [index.html]
- Add `<script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js">` to `<head>`
- Add Projects nav item (`data-page="projects"`, icon `🗂️`) after Tasks in sidebar
- Wire `fetchProjects()` in the nav click handler

### M3: Projects page HTML + CSS [index.html]
- Add `<div class="page" id="projects">` page div before `</main>`
- CSS: `.projects-grid`, `.project-card`, `.project-stats`, `.project-commit`, `.project-status-snippet`
- CSS: `.project-modal-overlay`, `.project-modal`, `.project-modal-tab`, `.md-content`

### M4: Frontend JS [index.html]
- `fetchProjects()` — calls `/api/projects`, stores in `projectsData[]`
- `renderProjectsGrid()` — builds project cards grid
- `buildProjectCard(p)` — renders a single card with stats + commit + status snippet
- `openProjectModal(idx)` — opens detail modal with status + context tabs (rendered via `marked`)
- `closeProjectModal()` / `switchProjectTab(tab)` — modal controls
- Add `<div id="projectModal" class="project-modal-overlay">` modal HTML before `</body>`

## Progress
- [x] M1: Backend helpers + /api/projects
- [x] M2: Nav item + marked CDN
- [x] M3: Page HTML + CSS
- [x] M4: JS implementation
