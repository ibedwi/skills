# Skills

My agent skills for Claude Code, Codex, and other agents.

## Installation

Pick one route per agent: installing both gives you every skill twice.

<details>
<summary><strong>Claude Code</strong></summary>

```bash
claude plugin marketplace add ibedwi/skills
claude plugin install ibedwi-skills@ibedwi
```

</details>

<details>
<summary><strong>Codex</strong></summary>

```bash
codex plugin marketplace add ibedwi/skills
codex plugin add ibedwi-skills@ibedwi
```

</details>

<details>
<summary><strong>Any other agent, or editable files</strong> (manual updates)</summary>

```bash
npx skills@latest add ibedwi/skills -a <agent>
```

To update, run `npx skills@latest update`, and re-run `add` to pick up new skills.

</details>

## Reference

These split on one axis: who can invoke them. **User-invoked** skills are reachable only when you type them (e.g. `/my-skill`). **Model-invoked** skills can be invoked by you _or_ reached for automatically by the agent when the task fits.

### Engineering

Skills I use daily for code work.

**User-invoked**

- **[fan-out](./skills/engineering/fan-out/SKILL.md)**: Start one herdr worktree lane per ready GitHub ticket, each running Claude on `/implement`, so tickets build in parallel.
- **[fan-in](./skills/engineering/fan-in/SKILL.md)**: Collect the lanes `/fan-out` started: open PRs for finished tickets, surface blocked agents, and clean up merged lanes.

**Model-invoked**

_None yet._

### Productivity

General workflow tools, not code-specific.

**User-invoked**

- **[setup-obsidian](./skills/productivity/setup-obsidian/SKILL.md)**: Set up the `obsidian` skill: pick a vault, confirm daily note and folder locations, and write its TOML config. Run once per machine.

**Model-invoked**

- **[obsidian](./skills/productivity/obsidian/SKILL.md)**: Log work to your daily note, save thoughts, and save conversation summaries to an Obsidian vault, configured by one TOML file.
