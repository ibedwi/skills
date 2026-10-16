# Adding a skill

1. Pick a bucket under `skills/` (see `CLAUDE.md`).
2. Create `skills/<bucket>/<name>/SKILL.md`:

   ```markdown
   ---
   name: <name>
   description: <model-invoked: rich "Use when..." triggers; user-invoked: one-line human summary>
   # disable-model-invocation: true   # user-invoked only
   ---

   <instructions>
   ```

3. Create `skills/<bucket>/<name>/agents/openai.yaml`:

   ```yaml
   interface:
     display_name: "<Display Name>"
     short_description: "<short summary>"
   # policy:                          # user-invoked only
   #   allow_implicit_invocation: false
   ```

4. Supporting reference files sit beside `SKILL.md` in the same folder.
5. List it in the bucket `README.md`.
6. If the bucket is promoted (`engineering/`, `productivity/`): add it to the top-level `README.md`, to `.claude-plugin/plugin.json`'s `skills` array, and write `docs/<bucket>/<name>.md`.
7. Run `npx changeset` to record the change, and `scripts/link-skills.sh` to install locally.
