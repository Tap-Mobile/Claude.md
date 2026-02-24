# Claude.md

Opinionated, battle-tested CLAUDE.md playbook for [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

Eliminates ping-pong, enforces verification, and keeps context lean.

## What's in it

- **Staff engineer identity** — ownership mentality, autonomous bug fixing
- **Structured workflow** — intake → plan → execute → verify → handoff
- **Visual feedback loop** — self-verify via Chrome extension, zero user roundtrips
- **Self-improvement** — persistent `tasks/lessons.md` that compounds across sessions
- **Subagent strategy** — model policy for Task tool (Sonnet 4.6+) and Codex MCP (5.3+, larger context window)
- **Safety rules** — production defaults, GPU awareness, destructive action gates
- **Frontend standards** — anti-slop design principles baked in

## Usage

Copy to your global config (applies to all projects):
```bash
mkdir -p ~/.claude
cp CLAUDE.md ~/.claude/CLAUDE.md
```

Or to a specific project root:
```bash
cp CLAUDE.md /path/to/your/project/CLAUDE.md
```

## Tasks directory

The playbook uses a `tasks/` directory for persistent state:
```
tasks/
├── todo.md      # Current session plan & progress
└── lessons.md   # Accumulated learnings across sessions
```

Create it in your project or home directory:
```bash
mkdir -p tasks
```

## License

MIT — use it, fork it, make it yours.
