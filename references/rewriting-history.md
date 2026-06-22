# Rewriting History

Changing commits that already exist: amending, resetting, rebasing, and replaying. Covers
`git commit --amend`, `git reset` (commit forms), `git rebase` (including interactive),
`git cherry-pick`, `git revert`, and `git reflog` for recovery.

**When this applies:** fixing the last commit, moving a branch pointer, reordering/squashing commits,
copying commits between branches, undoing a commit safely, or recovering after a mistake.

## The golden rule

Rewriting **published** history forces everyone else to repair theirs. Amend, reset, and rebase
freely on local/unshared commits; for commits already pushed to a shared branch, prefer `git revert`,
which records a new undo commit instead of altering the past.

## Amend the last commit

`git commit --amend` replaces the tip commit with the staged content and (optionally) a new message;
`--no-edit` keeps the existing message.

```
git add forgotten.c
git commit --amend --no-edit
```

## Moving HEAD with reset

`git reset [<mode>] <commit>` moves the current branch and conditionally touches the index and
working tree:

- `--soft` — move `HEAD` only; index and working tree **unchanged** (changes stay staged).
- `--mixed` (default) — move `HEAD` and reset the index; working tree **unchanged** (changes become
  unstaged).
- `--hard` — move `HEAD`, index, **and** working tree (discards changes — destructive).

`ORIG_HEAD` records the pre-reset commit, so a mistaken reset is recoverable.

```
git reset --soft HEAD~1   # undo last commit, keep changes staged
```

## Rebasing

`git rebase <upstream>` reapplies your commits on top of another base; `--onto <newbase>` transplants
a range; `--keep-base` rebases onto the merge base. Resolve conflicts then `git rebase --continue`
(or `--skip` / `--abort`). `--autostash` shelves dirty changes around the operation.

Interactive rebase (`-i`) opens a todo list whose verbs reshape history: `pick`, `reword`, `edit`,
`squash`, `fixup`, `drop`, `exec`, and `break`. Pair `--autosquash` with `git commit --fixup` /
`--squash` to mark commits for automatic folding.

```
git rebase -i HEAD~5
```

## Copying and undoing commits

- `git cherry-pick <commit>...` replays the changes of specific commits as new commits on the current
  branch; accepts ranges `A..B`; `-x` appends a "(cherry picked from commit …)" line; `-n`
  applies without committing; `-m <parent-number>` cherry-picks a merge.
- `git revert <commit>...` makes a **new** commit that undoes an earlier one — the safe choice for
  shared history. Reverting a merge needs `-m <parent-number>` to pick the mainline parent.

```
git revert HEAD          # safe undo of the last commit
git cherry-pick abc123   # bring one commit to this branch
```

Both use the resolve → `git add` → `--continue` flow on conflict and share `--abort` / `--quit` /
`--skip`.

## Recovering with reflog

`git reflog` lists where `HEAD` and branches have pointed (`HEAD@{<n>}`, `HEAD@{<date>}`). After a bad
reset or rebase, find the prior position and return to it.

```
git reflog
git reset --hard HEAD@{1}
```

Entries live in `.git/logs/` and expire per `gc.reflogExpire` (default 90 days) /
`gc.reflogExpireUnreachable` (default 30 days). See `recovery-and-maintenance.md`.

## Edge cases / anti-patterns

- **Don't** rebase or `--amend` commits already pushed to a shared branch — use `git revert`.
- **Don't** confuse the reset modes: `--soft` keeps everything staged, `--hard` discards the working
  tree.
- **Don't** panic after a bad reset — `reflog` and `ORIG_HEAD` almost always recover it.

## Do / Don't

- **Do** clean up local history with interactive rebase before sharing.
- **Do** use `git revert -m 1` to undo a pushed merge.
- **Don't** use `git reset --hard` with uncommitted work you might still want.

## Related

`snapshotting.md` · `branching-and-merging.md` · `recovery-and-maintenance.md` ·
`inspection-and-comparison.md`
