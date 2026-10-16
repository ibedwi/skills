## What it does

`fan-out` turns a batch of GitHub tickets into parallel work. For each ticket it creates a **lane**: a herdr worktree on a fresh `ticket/<n>-<slug>` branch cut from `origin/main` (or the base you name), with a Claude agent inside it running `/implement` on that ticket. It assigns the ticket to you as a claim, so a second run won't start it again.

You pick the tickets one of two ways:

- by number: `/fan-out 20 24 40`;
- by parent: `/fan-out parent=17` takes the parent's **frontier**, its open sub-issues with no open blockers and no assignee.

It always shows you the list and waits for a yes before starting anything. It finishes with a table of lanes (ticket, agent name, workspace, branch) and the commands to watch them.

## When to reach for it

Type `/fan-out` when a spec has been broken into tickets and several of them are unblocked at once. The agent never runs it on its own. When the agents show `done`, run [`/fan-in`](./fan-in.md).

## Common questions

**What do I need installed?**
herdr (run `/fan-out` from a herdr pane), the `gh` CLI logged in to the repo, `jq`, and an `implement` skill the lane agents can run.

**Can lanes branch from something other than `main`?**
Yes: add `base=<branch>`. The branch must exist on `origin`. The base is stored in `git config branch.<lane>.fanOutBase` so `/fan-in` opens the PR against it.

**Why does it run agents with `bypassPermissions`?**
A lane is unattended. Without it every agent stalls on its first permission prompt. Each lane works in its own worktree, and nothing merges without you.

**An agent got stuck at start.**
`fan-out` reads its pane and shows you the question. Answer it in that lane's pane.

## It's working if

- Every ticket you confirmed has a lane in `herdr agent list`, shown `working`.
- Each ticket on GitHub is assigned to you.
- Running `/fan-out parent=<n>` again offers no ticket that already has a lane.
