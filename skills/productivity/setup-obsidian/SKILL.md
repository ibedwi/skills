---
name: setup-obsidian
description: Set up the obsidian skill: pick a vault, confirm daily note and folder locations, and write its TOML config.
disable-model-invocation: true
---

Write the config the `obsidian` skill reads, by interviewing the user. Ask one question at a time, each with a recommended answer drawn from what you found on disk, so the user can just accept it. Do not write anything inside the vault.

## 1. Check for an existing config

Look for `$OBSIDIAN_SKILL_CONFIG`, else `~/.config/obsidian-skill/config.toml`. If one exists, show its current values and ask whether to update it or stop. When updating, edit the file in place and keep the user's comments and any keys you did not ask about.

## 2. Vault

Read the vault list from `~/Library/Application Support/obsidian/obsidian.json` (macOS), `~/.config/obsidian/obsidian.json` (Linux), or `%APPDATA%\obsidian\obsidian.json` (Windows): the `vaults` object, each with a `path`, and `open: true` on the one open now. Offer those paths, the open one first. Accept any other path the user gives, and confirm it exists before going on.

## 3. Daily notes

Read `<vault>/.obsidian/daily-notes.json` (keys `folder`, `format`, `template`). If found, show the values and recommend leaving `[daily]` unset so the skill keeps following Obsidian's own settings. If not found, ask for the folder and filename format (recommend the vault root and `YYYY-MM-DD`) and write them to `[daily]`.

## 4. Work log

Ask where log entries go: under a heading in the daily note (`mode = "daily"`, recommended) or in a separate log file (`mode = "file"`, then ask for `folder` and `filename`). Then ask for the heading, recommending `## Work log`. If the daily note template already has a log-like heading, recommend that one instead.

## 5. Thoughts and summaries folders

List the vault's top-level folders (skip dot-folders). Ask for the thoughts folder and then the conversation summaries folder. Recommend an existing match when there is one (an inbox such as `Inbox` or `0 Inbox` for thoughts; something like `AI`, `Resources`, or `Logs` for summaries), else the defaults `Inbox` and `AI conversations`. Say that missing folders are created on first write.

## 6. Write the config

Fill [config.example.toml](./config.example.toml) with the answers and write it to `~/.config/obsidian-skill/config.toml` (or the `$OBSIDIAN_SKILL_CONFIG` path), creating the directory. Keep keys the user did not change at their example values.

## 7. Per-project override (optional)

If the current directory is inside a git repo, ask whether this repo should log under its own settings. If yes, ask for the project name (recommend the git root basename) and any extra tags, then write `.obsidian-skill.toml` at the git root with only those keys:

```toml
project = "<name>"
tags = ["<tag>"]
```

Ask whether to commit it or add it to `.gitignore`.

## 8. Confirm

Print the resolved targets for today, as absolute paths: the daily note (and whether it exists yet), the work log heading, a sample thought path, and a sample summary path. Tell the user the setup is done and that `/obsidian log`, `/obsidian thought`, and `/obsidian summary` (or just asking in plain words) now work.
