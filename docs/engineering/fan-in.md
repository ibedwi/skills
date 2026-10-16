## What it does

`fan-in` walks every lane that [`/fan-out`](./fan-out.md) started and moves each one forward by its state:

| Lane | What `fan-in` does |
|---|---|
| Agent still working | Leaves it. |
| Agent blocked | Shows you its question. |
| Agent done, no PR | Pushes the branch and opens a PR against the lane's base, with `Closes #<n>`. |
| PR open | Reports the link and its checks. |
| PR merged | Removes the worktree and branch, and fast-forwards your main checkout. |
| PR closed unmerged | Asks whether to remove the lane. |

It never merges. Merging is your gate, and a lane is only cleaned up after its PR merges. It ends with a table of what it did and which tickets the open PRs would unblock.

## When to reach for it

Type `/fan-in` whenever you check on a round of lanes: after agents show `done`, after you merge some PRs, or just to see where things stand. It is safe to run repeatedly. The agent never runs it on its own.

## Common questions

**A lane finished but got no PR.**
If the agent said it didn't finish, or the branch has no commits past its base, `fan-in` reports the lane as stalled and shows the agent's last output instead of opening an empty PR.

**Who writes the PR body?**
A `pr` skill, if one is installed; otherwise `fan-in` summarizes the lane's commits. Either way the body closes the ticket.

**My main checkout has uncommitted changes. Will cleanup break it?**
No. It fast-forwards with `git merge --ff-only`, which works over uncommitted changes, and only when your checkout is on the lane's base.

## It's working if

- Finished lanes show up as PRs with `Closes #<n>`.
- After you merge, the next `/fan-in` removes those lanes and `git worktree list` no longer shows them.
- The report names newly unblocked tickets, ready for the next `/fan-out`.
