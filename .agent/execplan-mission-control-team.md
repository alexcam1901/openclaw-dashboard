# ExecPlan: Mission Control — Team Screen (Org Chart + Mission Statement)

## Goal
Add a Team page to the OpenClaw Mission Control Dashboard showing an org chart of all
configured agents (from `~/.openclaw/openclaw.json`) with their identity cards, plus an
editable Mission Statement.

## Data Sources
- `~/.openclaw/openclaw.json` → `agents.list[]` for agent roster (id, name, model, workspace)
- `{agent.workspace}/IDENTITY.md` → Name, Creature, Vibe, Emoji fields
- `{agent.workspace}/SOUL.md` → First non-heading paragraph as soul excerpt
- `{WORKSPACE_DIR}/data/mission.json` → `{ statement, updatedAt }` (created on first save)

## API Endpoints

### GET /api/team
Returns:
```json
{
  "agents": [
    {
      "id": "main",
      "name": "Main",
      "model": "global.anthropic.claude-sonnet-4-6",
      "identity": { "name": "Main", "creature": "AI Assistant", "vibe": "Balanced, helpful, direct", "emoji": "⚙️" },
      "soulExcerpt": "Be genuinely helpful. Just help..."
    }
  ],
  "mission": { "statement": "...", "updatedAt": "..." }
}
```

### PUT /api/team/mission
Body: `{ "statement": "..." }` (max 2000 chars)
Returns: `{ "ok": true, "mission": { "statement": "...", "updatedAt": "..." } }`

## Milestones

### M1: API endpoints in server.js
- Add `getTeamData()` helper function that reads openclaw.json + IDENTITY/SOUL files
- Add `GET /api/team` endpoint
- Add `PUT /api/team/mission` endpoint (writes `{WORKSPACE_DIR}/data/mission.json`)

### M2: Team nav item [index.html]
- Add `<div class="nav-item" data-page="team">` (icon `👥`) after calendar nav item
- Wire `fetchTeam()` in the nav click handler (`if (page === 'team')`)

### M3: Team page CSS [index.html]
- `.team-mission`, `.team-mission-header`, `.team-mission-label`, `.team-mission-text`
- `.team-mission-edit` (textarea), `.mission-btn`, `.mission-btn-save`, `.mission-actions`
- `.team-grid` (CSS grid), `.agent-card`, `.agent-emoji`, `.agent-name`
- `.agent-model` (badge), `.agent-creature`, `.agent-vibe`, `.agent-soul`

### M4: Team page HTML + JS [index.html]
HTML (after calendar `</div>`):
- `<div class="page" id="team">` with mission section and agent grid

JS functions:
- `fetchTeam()` — fetches `/api/team`, stores in `teamData`, calls `renderTeam()`
- `renderTeam()` — calls sub-renderers
- `renderTeamMission(mission)` — displays/updates the mission statement
- `renderTeamGrid(agents)` — renders agent cards
- `teamMissionEdit()` — switch to edit mode (show textarea)
- `teamMissionCancel()` — cancel edit, restore display
- `teamMissionSave()` — PUT to `/api/team/mission`, update UI

## Progress
- [x] M1: API endpoints in server.js
- [x] M2: Nav item + click handler
- [x] M3: CSS
- [x] M4: Page HTML + JS
