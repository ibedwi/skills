## What it does

`setup-obsidian` writes the config file the [obsidian](./obsidian.md) skill reads, `~/.config/obsidian-skill/config.toml`, by asking you a handful of questions. Each question comes with a recommended answer based on what it found on disk, so most of the time you just accept.

It asks, one at a time:

1. Which vault, from the list Obsidian itself keeps (the open vault first).
2. Whether to follow your Daily notes plugin settings (recommended) or set the daily note folder and format yourself.
3. Where work log entries go: under a heading in the daily note, or in a separate log file.
4. Which folders hold thoughts and conversation summaries, suggesting matching folders that already exist in the vault.
5. Inside a git repo: whether this repo gets its own `.obsidian-skill.toml` with a project name and tags.

It never writes inside the vault. It finishes by printing the exact paths today's log, a thought and a summary would go to.

## When to reach for it

Type `/setup-obsidian` once per machine, before your first `/obsidian`. Run it again to change vaults or folders: it shows the current config and edits it in place, keeping your comments. The agent never runs it on its own; `obsidian` tells you to run it when no config exists.

## Common questions

**Can I skip it and write the config by hand?**
Yes. Only `vault` is required. `skills/productivity/obsidian/CONFIG.md` has every key and its default.

**I have more than one vault.**
Pick one here. To use another for a while, point `$OBSIDIAN_SKILL_CONFIG` at a second config file.

## It's working if

- The printed daily note path is the note Obsidian opens when you click "Open today's daily note".
- The thoughts and summaries folders are ones you actually browse.
- `/obsidian log` works straight after, without asking where to write.
