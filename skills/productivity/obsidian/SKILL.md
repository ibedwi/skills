---
name: obsidian
description: Write to the user's Obsidian vault. Use when the user wants to log work ("log this", "add to my work log", "what did I do today, write it down"), save a thought, idea, or note ("save this thought", "jot this down", "note to self"), or save a summary of the current conversation ("summarize this to Obsidian", "save this session"). Also use when the user mentions their vault, daily note, or Obsidian.
argument-hint: "log | thought | summary, plus optional text"
---

Writes three kinds of notes into an Obsidian vault, all driven by one TOML config. The vault is a plain folder of Markdown files: write them directly with file tools. The Obsidian app does not need to be running.

## 1. Load the config

Read the first file that exists:

1. `$OBSIDIAN_SKILL_CONFIG`
2. `~/.config/obsidian-skill/config.toml`

Then, if the current project has `.obsidian-skill.toml` at its git root, read it too and let its keys override the global ones, table by table. This lets one repo log to a different folder or carry its own tags.

[CONFIG.md](./CONFIG.md) is the full schema and its defaults. Every key except `vault` is optional.

**No config found:** stop and tell the user to run `/setup-obsidian`, which writes the config. Do not guess a vault.

Expand `~` in paths. If `vault` does not exist on disk, stop and tell the user.

## 2. Pick the action

From the user's words or the first argument:

| Action | Triggers | Goes to |
| --- | --- | --- |
| `log` | log, work log, what I did, done today | appended to the daily note (or a log file) |
| `thought` | thought, idea, note, jot, remember this | a new note per thought |
| `summary` | summarize, save this conversation/session | a new note per conversation |

If it is still ambiguous, ask with one short question.

## 3. Build the content

Placeholders available in every template and filename: `{date}`, `{time}`, `{datetime}`, `{title}`, `{project}`, `{agent}`, `{text}`, `{tags}`.

- `{date}`/`{time}`: now, rendered with `date_format`/`time_format` (Moment.js tokens, the same ones Obsidian's Daily notes plugin uses). Get the time from the shell `date` command, not from memory.
- `{datetime}`: now as ISO 8601 with offset (`date +%Y-%m-%dT%H:%M:%S%z`), for frontmatter.
- `{project}`: `project` from config, else the basename of the current git root, else the current directory name.
- `{title}`: a short title you write (3 to 7 words), stripped of characters illegal in filenames (`\ / : * ? " < > |`, `#`, `^`, `[`, `]`).
- `{agent}`: the harness you are running in (e.g. `claude-code`, `codex`).
- `{text}`: the content built below (one log line, the thought, or the rendered summary sections).
- `{tags}`: the merged tag list as Obsidian inline tags (`#thought #ai-conversation`).

**log:** use the user's text if given. If not, write one to three terse bullet lines of what was actually done in this session (changes made, things shipped or decided), not what was discussed. Render each line with `work_log.entry_template`.

**thought:** keep the user's words. Light cleanup only (typos, line breaks). Do not expand or reinterpret it. Add a `## Context` line only if the thought is meaningless without it.

**summary:** write the sections listed in `summary.sections`, in order, skipping any that would be empty. Be specific: name files, commands, decisions, and links (paths, PR/issue URLs) rather than describing them. Reference long artifacts by path instead of copying them.

For every action: redact secrets (API keys, tokens, passwords) and anything the user marked private.

## 4. Write it

Paths are relative to `vault`. Create missing folders.

- **New note** (`thought`, `summary`): path is `{folder}/{filename}.md`. Never overwrite: if it exists, append ` 2`, ` 3`, ... to the name. Start the note with YAML frontmatter from `frontmatter` (see CONFIG.md), then the body. If `template` is set, read that vault note and fill its placeholders instead of using the default body layout.
- **Append to daily note** (`log` with `work_log.mode = "daily"`, the default): open the daily note for today. If it is missing, create it (from `daily.template` if set, else empty). Find the line matching `work_log.heading`. If it exists, insert the entries at the end of that section (before the next heading of the same or higher level). If not, append the heading and entries at the end of the note. Never touch other content in the note.
- **Append to log file** (`log` with `work_log.mode = "file"`): same as the daily note case, but the file is `work_log.folder`/`work_log.filename`.md, created with frontmatter if missing.

## 5. Report

Reply with the vault-relative path written and, for an append, the lines added. One or two lines, no recap of the content.
