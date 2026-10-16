# Productivity

General workflow tools, not code-specific.

## User-invoked

Reachable only when you type them (Claude Code: `disable-model-invocation: true`; Codex: `policy.allow_implicit_invocation: false` in `agents/openai.yaml`).

- **[setup-obsidian](./setup-obsidian/SKILL.md)**: Set up the `obsidian` skill: pick a vault, confirm daily note and folder locations, and write its TOML config. Run once per machine.

## Model-invoked

Model- or user-reachable (rich trigger phrasing so the model can reach for them).

- **[obsidian](./obsidian/SKILL.md)**: Log work to your daily note, save thoughts, and save conversation summaries to an Obsidian vault, configured by one TOML file.
