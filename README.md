# Claude.md — God Mode

Built from the Claude Code source code. Maximum autonomy, maximum capability, minimum friction.

## Why this exists

We analyzed the Claude Code source and found behaviors that silently cripple your agent:

| Problem | What actually happens | What we fix |
|---|---|---|
| **Silent model downgrade** | After a few 429/529 errors, CC switches from Opus to Sonnet without telling you | Block all downgrades without explicit user consent |
| **Blind subagents** | Agents spawned with `subagent_type` get zero conversation context | Playbook specifies when to fork (shared cache) vs fresh (independent) |
| **Lossy compaction** | Auto-compaction at ~167K tokens keeps only ~5 message pairs, everything else summarized | Proactive compaction, file-based state persistence — you control what survives |
| **Truncated search** | Grep caps at 250 results; tool results over 50K chars truncated to 2KB preview | Exhaustive search with `head_limit: 0`, detect and recover from truncation |
| **Cache-killing model switches** | Switching models mid-session invalidates the entire prompt cache | Stay on chosen model, fork to subagent for different capabilities |

## What's in it

- **Scope ladder** — smallest fix → structural change → bold refactor in isolated worktree
- **Agent swarms** — no concurrency limit, fan out many agents for research, synthesize, execute
- **Self-review gate** — Opus code-review subagent verifies work before reporting done
- **Context mastery** — file system as infinite memory, proactive compaction, never partial data
- **Error-first debugging** — grep the error message before guessing
- **Visual feedback loop** — self-verify via Chrome extension, zero user roundtrips
- **Bug autopsy & failure recovery** — explain *why* bugs happen, escalate after two failed attempts
- **Self-improvement loop** — persistent `tasks/lessons.md` compounds learnings across sessions
- **Frontend standards** — anti-slop design principles when working on UI
- **Safety** — move fast without breaking prod, rollback-first strategy

## Install

Copy to your global config (applies to all projects):
```bash
mkdir -p ~/.claude
curl -o ~/.claude/CLAUDE.md https://raw.githubusercontent.com/Tap-Mobile/Claude.md/main/CLAUDE.md
```

Or to a specific project root:
```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/Tap-Mobile/Claude.md/main/CLAUDE.md
```

## Tasks directory

The playbook uses a `tasks/` directory for persistent state:
```
tasks/
├── todo.md      # Current session plan & progress
├── lessons.md   # Accumulated learnings across sessions
└── context.md   # Working state persisted across compaction
```

Create it in your project or home directory:
```bash
mkdir -p tasks
```

## License

Proprietary. All rights reserved.
