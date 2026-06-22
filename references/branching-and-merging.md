# Branching and Merging

Creating and moving between lines of development and combining them. Covers `git branch`,
`git switch`, `git checkout`, `git merge`, conflict resolution, and `git tag`.

**When this applies:** creating/listing/deleting branches, switching the working tree, merging
history, resolving conflicts, or tagging releases.

## Branches

`git branch` lists, creates, deletes, and renames branches (pointers under `refs/heads/`):

- Create: `git branch <name> [<start-point>]`. List: `-l` / `--list`, `-a` / `--all` (adds
  remote-tracking), `-r` / `--remotes`.
- Delete: `-d` / `--delete` (refuses unmerged); `-D` forces. Rename: `-m` / `--move`; copy `-c`.
- Upstream: `-u <upstream>` / `--set-upstream-to=<upstream>`, `--unset-upstream`,
  `--track` / `--no-track`. Filters: `--merged` / `--no-merged`, `--contains`.

```
git branch --merged    # branches safe to delete
```

## Switching

Prefer `git switch` for moving between branches (clearer than the dual-purpose `git checkout`):

- `git switch <branch>` changes the working tree and index.
- `-c <new-branch> [<start-point>]` / `--create` creates and switches; `-C` resets if it exists.
- `--detach` enters detached HEAD; `-` or `@{-N}` returns to a previous branch.

```
git switch -c feature HEAD~3
```

`git checkout` still does both jobs — `git checkout -b <new-branch>` (create + switch) and the
file-restore form `git checkout [<tree-ish>] -- <pathspec>` (now `git restore`, see
`snapshotting.md`). Use `--` to separate pathspecs from branch names.

## Merging

`git merge <commit>` joins another history into the current branch:

- **Fast-forward:** if `HEAD` is an ancestor of the target, the pointer simply advances — no merge
  commit. Force a merge commit with `--no-ff`; require fast-forward with `--ff-only`.
- `--squash` stages the combined result without committing or moving `HEAD`.
- `--no-commit` pauses before the commit; `-m <msg>` sets the message; `-s <strategy>` /
  `-X <strategy-option>` (e.g. `ours`, `theirs`) tune strategy.

```
git merge --no-ff feature
```

`merge.ff` (`false` / `only` / default) sets the default behavior.

## Resolving conflicts

When a merge conflicts, `MERGE_HEAD` points at the other branch and Git writes markers into the file:

```
<<<<<<< HEAD
your version
=======
their version
>>>>>>> feature
```

`merge.conflictStyle` can switch to `diff3` / `zdiff3`, which add a `|||||||` base section. Resolve by
editing, `git add`-ing the file, then `git merge --continue` (or `git commit`). `git merge --abort`
restores the pre-merge state; `git merge --quit` leaves the index/working tree as-is.

## Tagging

`git tag <name> [<commit>]` marks a point under `refs/tags/`:

- **Annotated** (`-a`, or `-s` / `-u <key-id>` to sign) stores tagger, date, and message — use for
  releases. `-m <msg>` implies `-a`.
- **Lightweight** (no option) is just a name; `git describe` ignores these by default.
- `-d` deletes; `-l` lists; `-v` verifies. Tags are **not pushed by default**.

```
git tag -a v1.0 -m "Release 1.0"
git push origin v1.0
```

## Edge cases / anti-patterns

- **Don't** assume `git merge` always makes a merge commit — fast-forward leaves no record unless
  `--no-ff`.
- **Don't** forget tags need an explicit `git push origin <tagname>` (or `--tags`).
- **Don't** leave a detached HEAD's new commits unreferenced — create a branch before switching away.

## Do / Don't

- **Do** use `git switch` / `git switch -c` for branch work and reserve `git checkout` for legacy use.
- **Do** delete merged branches with `-d` (safe) rather than `-D`.
- **Don't** resolve a conflict by deleting markers without checking both sides.

## Related

`snapshotting.md` · `rewriting-history.md` · `remotes-and-sync.md` · `refs-and-revisions.md`
