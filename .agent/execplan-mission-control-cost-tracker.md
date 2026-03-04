# ExecPlan: Mission Control — Cost Tracker (Per-Agent Breakdown)

## Goal
Enhance the existing Costs page with a per-agent spending breakdown, showing how much each
OpenClaw agent has cost across all time.

## Data Sources
- Session JSONL files: `~/.openclaw/agents/*/sessions/*.jsonl`
- Agent IDs derived from directory path: `path.basename(path.dirname(sessDir))`
- Model pricing: `data/model_pricing_usd_per_million.json` (already loaded)
- Existing `estimateMsgCost()` and `normalizeModel()` functions in server.js

## Milestones

### M1: Backend — Add `perAgent` to `getCostData()` [server.js]
- In the inner loop of `getCostData()`, extract agentId from session dir path
- Aggregate `perAgent[agentId] += cost` alongside the existing `perModel`, `perDay` buckets
- Include `perAgent` in the returned object
- No new endpoint needed — piggybacked on existing `/api/costs`

### M2: Frontend HTML — Add "Cost by Agent" card [index.html]
- Add a new full-width card below the existing `grid-2` (Cost by Model + Top Sessions)
- Card id `costByAgent`, title "Cost by Agent"
- CSS-only horizontal bar chart reusing existing patterns

### M3: Frontend JS — Render agent breakdown in `updateCosts()` [index.html]
- Read `costs.perAgent`, sort descending by cost
- Render horizontal bar rows with percentage bars
- Agent name in monospace, cost right-aligned
- Colors reuse `--accent`, `--green`, `--purple`, `--yellow`, `--cyan` cycling

## Progress
- [x] M1: Backend perAgent aggregation
- [x] M2: HTML card for Cost by Agent
- [x] M3: JS rendering in updateCosts()
