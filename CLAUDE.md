Skills are organized into bucket folders under `skills/`:

- `engineering/`: daily code work
- `productivity/`: daily non-code workflow tools
- `misc/`: kept around but rarely used, not promoted

Every skill in `engineering/` or `productivity/` (the **promoted** buckets) must have a reference in the top-level `README.md` and an entry in `.claude-plugin/plugin.json`'s `skills` array (the Claude Code plugin ships exactly the promoted set). Skills in `misc/` must not appear in either.

`.claude-plugin/marketplace.json` is the repo's own marketplace. Run `claude plugin validate . --strict` after touching either manifest.

Each skill entry in the top-level `README.md` must link the skill name to its `SKILL.md`.

Each bucket folder has a `README.md` that lists every skill in the bucket with a one-line description, with the skill name linked to its `SKILL.md`. The promoted buckets' `README.md`s and the top-level `README.md` group entries into **User-invoked** and **Model-invoked**; `misc/README.md` uses a flat list.

Skills in `engineering/` and `productivity/` also have a human-facing docs page at `docs/<bucket>/<skill-name>.md` (the docs tree mirrors those two bucket folders). A page carries four sections: **What it does**, **When to reach for it**, **Common questions**, and **It's working if**. Skills in `misc/` get no docs page.

Every `SKILL.md` is either user-invoked (`disable-model-invocation: true` plus `policy.allow_implicit_invocation: false` in `agents/openai.yaml`, reachable only by the human) or model-invoked (model- or user-reachable). See [.agents/invocation.md](./.agents/invocation.md). Steps for adding a skill are in [.agents/new-skill.md](./.agents/new-skill.md).

Versioning uses changesets: add one with `npx changeset` for every skill change. The Release workflow runs `npm run version`, which also copies the version from `package.json` into `.claude-plugin/plugin.json`.

To (re)link every skill outside `misc/` into the local harness skill directories (`~/.claude/skills`, `~/.agents/skills`), run `scripts/link-skills.sh`. Re-run it after adding, removing, or renaming a skill.

No em-dashes anywhere in this repo's prose. Rewrite the sentence with a comma, colon, period, parentheses, or a conjunction instead.
