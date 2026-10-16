# Config schema

TOML. Global file: `$OBSIDIAN_SKILL_CONFIG`, else `~/.config/obsidian-skill/config.toml`. Optional per-project override: `.obsidian-skill.toml` at the git root, merged over the global file table by table. `/setup-obsidian` writes the global file.

## Top level

| Key | Default | Meaning |
| --- | --- | --- |
| `vault` | (required) | Absolute path to the vault folder. `~` is expanded. |
| `project` | git root basename | Value of `{project}`. Usually set only in a per-project file. |
| `date_format` | `"YYYY-MM-DD"` | Moment.js tokens for `{date}`. |
| `time_format` | `"HH:mm"` | Moment.js tokens for `{time}`. |
| `tags` | `[]` | Tags added to every new note, merged with each table's own `tags`. |

## `[daily]`

Where the daily note lives. Used by `log` in `daily` mode.

| Key | Default | Meaning |
| --- | --- | --- |
| `folder` | from vault | Daily notes folder. |
| `format` | from vault | Moment.js filename format, e.g. `"YYYY-MM-DD"` or `"YYYY/MM/YYYY-MM-DD"`. |
| `template` | from vault | Vault-relative path of the note used when creating a missing daily note. |

"From vault": when a key is unset, read `<vault>/.obsidian/daily-notes.json` (the Daily notes core plugin's settings, keys `folder`, `format`, `template`) so the skill writes to the same note Obsidian opens. If that file is missing too, use the vault root and `YYYY-MM-DD`.

## `[work_log]`

| Key | Default | Meaning |
| --- | --- | --- |
| `mode` | `"daily"` | `"daily"`: append to today's daily note. `"file"`: append to one file named by `filename` in `folder`, creating it if missing. |
| `heading` | `"## Work log"` | Section the entries go under. |
| `entry_template` | `"- {time} **{project}**: {text}"` | One line per entry. |
| `folder` | `"Work log"` | `file` mode only. |
| `filename` | `"{date}"` | `file` mode only. Use e.g. `"Work log"` for one ever-growing file. |

## `[thought]`

| Key | Default | Meaning |
| --- | --- | --- |
| `folder` | `"Inbox"` | |
| `filename` | `"{date} {title}"` | |
| `tags` | `["thought"]` | |
| `template` | none | Vault-relative path of a template note. Placeholders are filled; `{text}` is the thought. |

## `[summary]`

| Key | Default | Meaning |
| --- | --- | --- |
| `folder` | `"AI conversations"` | |
| `filename` | `"{date} {title}"` | |
| `tags` | `["ai-conversation"]` | |
| `sections` | `["Summary", "Decisions", "Open questions", "Next steps", "Links"]` | `##` sections written in order; empty ones are skipped. |
| `template` | none | As for `[thought]`; `{text}` is the rendered sections. |
| `link_daily` | `true` | Also append `- {time} [[<note name>]]` under `work_log.heading` in today's daily note. |

## `[frontmatter]`

Written at the top of every new note. Each value is a template string; empty results are dropped. `tags` is always written from the merged tag lists.

```toml
[frontmatter]
created = "{datetime}"
project = "{project}"
source = "{agent}"
```

Default: exactly the three keys above. Set `[frontmatter]` to an empty table to write only `tags`.
