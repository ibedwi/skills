# Engineering

Skills I use daily for code work.

## User-invoked

Reachable only when you type them (Claude Code: `disable-model-invocation: true`; Codex: `policy.allow_implicit_invocation: false` in `agents/openai.yaml`).

- **[fan-out](./fan-out/SKILL.md)**: Start one herdr worktree lane per ready GitHub ticket, each running Claude on `/implement`, so tickets build in parallel.
- **[fan-in](./fan-in/SKILL.md)**: Collect the lanes `/fan-out` started: open PRs for finished tickets, surface blocked agents, and clean up merged lanes.

## Model-invoked

Model- or user-reachable (rich trigger phrasing so the model can reach for them).

_None yet._
