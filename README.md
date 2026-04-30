# ai-agents

Project-level [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) agents and skills, with the tronscan MCP server pre-wired for TRON on-chain analysis.

## What's inside

- **`tron-expert`** — TRON on-chain behavior analyst. Give it an address, transaction hash, or contract; it produces a structured report (snapshot / behavior / counterparties / risk signals / confidence and gaps).
- **`tronscan` MCP server** — registered in `.mcp.json` against `https://mcp.tronscan.org/mcp`, exposing read-only analytics (account detail, transfers, related accounts, stablecoin events, contract stats).
- **Skill / commands / docs scaffolding** — empty folders ready for your own additions.

## Prerequisites

- Claude Code installed and authenticated.
- Git.
- Network access to `https://mcp.tronscan.org/mcp`.

## Install

```bash
git clone git@github.com:davidapp/ai-agents.git
cd ai-agents
claude
```

On the first `claude` launch in this directory, Claude Code will:

1. Detect `.mcp.json` and prompt whether to trust the `tronscan` server. Approve.
2. Load `tron-expert` from `.claude/agents/`.

No build or dependency install — agent files and `.mcp.json` are read at session start.

## Verify

Inside Claude Code, run:

- `/mcp` — `tronscan` should appear with status `connected`.
- `/agents` — `tron-expert` should appear in the list.

If either is missing, restart the session. Agents and MCP servers are loaded at session start and do not hot-reload.

## First use

Type into Claude Code:

```
分析 TRON 地址 TQq8GStVyztZzNBtDeaUCGvjgyLuGxFMhH
```

Claude routes to `tron-expert` via the agent's `description` trigger and runs its six-step analysis flow.

## Layout

```
.claude/
  agents/
    tron-expert.md          # TRON behavior analyst
  skills/
    sample-skill/           # skill scaffold
  commands/                 # custom /slash commands
  settings.json             # project-shared settings
.mcp.json                   # tronscan server registered here
.mcp.example.json           # stdio / http config template
docs/
  agents/  skills/  mcp/    # debug notes per component
```

## Optional: trongrid fallback

`tron-expert` will use `mcp__trongrid__*` for raw chain state (block-exact balances, JSON-RPC, event logs) when tronscan numbers look stale. That server is **not** shipped here — register it yourself in `~/.claude.json` (user-level, shared across projects) or extend this repo's `.mcp.json`.

## Adding your own agent

```bash
$EDITOR .claude/agents/<name>.md
```

Minimal frontmatter:

```markdown
---
name: <kebab-case>
description: <when to invoke — this is the routing signal, be specific>
tools: <optional, comma-separated tool whitelist>
---

<system prompt body>
```

Restart the session after creating.

## License

See [LICENSE](LICENSE).
