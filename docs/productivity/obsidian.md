## What it does

`obsidian` writes three kinds of notes into your Obsidian vault:

- **Work log**: one-line entries appended under a `## Work log` heading in today's daily note. Give it text, or let it write what this session actually did.
- **Thoughts**: a new note per thought, kept in your own words, in an inbox folder.
- **Conversation summaries**: a new note with Summary, Decisions, Open questions, Next steps and Links sections, plus a link from today's daily note.

It writes Markdown files straight into the vault folder, so Obsidian does not need to be open.

## Setup

Run `/setup-obsidian` once. It finds the vaults Obsidian knows about, asks which one to use, checks your daily note settings and folders, and writes `~/.config/obsidian-skill/config.toml`. Until that file exists, `obsidian` tells you to run it.

You can also edit the file by hand: only `vault` is required, and `skills/productivity/obsidian/CONFIG.md` has the full schema. Point `$OBSIDIAN_SKILL_CONFIG` at another file to swap configs.

For a per-repo tweak (a different project name, extra tags, another folder), add `.obsidian-skill.toml` at the repo's git root. Its tables override the global ones. `/setup-obsidian` offers to write it when run inside a repo.

## When to reach for it

Type `/obsidian log`, `/obsidian thought`, or `/obsidian summary`, or just say it: "log this", "save this thought", "summarize this conversation to Obsidian". The agent can also reach for it on its own when you mention your vault or daily note.

## Common questions

**Which daily note does it write to?**
The one Obsidian opens. Unless you set `[daily]` in the config, it reads the vault's Daily notes plugin settings (`.obsidian/daily-notes.json`) for the folder, filename format and template.

**Will it overwrite anything?**
No. New notes get a ` 2`, ` 3` suffix on a name clash, and appends touch only the work log section.

**Can I change the note layout?**
Yes: `entry_template`, `filename`, `sections`, `[frontmatter]`, or a `template` pointing at a note in your vault.

## It's working if

- Log entries land in today's daily note under the work log heading, with time and project.
- A saved thought reads like you wrote it, not like the agent rewrote it.
- A summary names real files, decisions and links, and you can act on it cold.
- Nothing in the vault contains a key, a token or a password.
