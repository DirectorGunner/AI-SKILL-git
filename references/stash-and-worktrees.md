# Stash, Worktrees, and Submodules

Setting work aside, checking out several branches at once, embedding repositories, and exporting.
Covers `git stash`, `git worktree`, `git submodule`, `git sparse-checkout`, `git archive`, and
`git bundle`.

**When this applies:** shelving uncommitted changes, running multiple working trees, nesting another
repo, checking out a subset of a large repo, or packaging a tree/repo for transfer.

## Stashing changes

`git stash` saves a dirty working tree and index onto a stack and reverts to a clean `HEAD`:

- `git stash` (or `push`) shelves changes; `-u` / `--include-untracked` also stashes untracked files;
  `-k` / `--keep-index` keeps staged changes in the index; `-p` selects hunks.
- `git stash list`, `git stash show [<stash>]` inspect entries (addressed as `stash@{<n>}`).
- `git stash pop` applies **and** removes the top entry; `git stash apply` applies but keeps it;
  `git stash drop` / `clear` remove entries; `git stash branch <name>` restores onto a new branch.

```
git stash -u
git stash pop
```

## Multiple worktrees

`git worktree` attaches additional working trees to one repository, so you can have several branches
checked out at once:

- `git worktree add [-b <new-branch>] <path> [<commit-ish>]` creates a linked worktree.
- `git worktree list`, `git worktree remove <path>`, `git worktree prune`, `git worktree move`.

A branch can be checked out in only one worktree at a time. Each worktree has its own `HEAD` and
index; metadata lives under `$GIT_DIR/worktrees/`.

```
git worktree add -b hotfix ../hotfix main
```

## Submodules

`git submodule` embeds another repository at a path, pinned to a specific commit, recorded in the
tracked `.gitmodules` file (`submodule.<name>.url` / `.path` / `.branch`):

- `git submodule add <repository> [<path>]` adds one.
- `git submodule update --init --recursive` clones/populates them (or clone with
  `--recurse-submodules`).
- `git submodule status`, `git submodule update --remote` (advance to upstream),
  `git submodule foreach <command>`, `git submodule deinit`.

```
git clone --recurse-submodules <url>
git submodule update --init --recursive
```

## Sparse-checkout

`git sparse-checkout` limits the working tree to a subset of paths (the rest use the skip-worktree
bit). Cone mode (default, `--cone`) takes directories and is fast:

- `git sparse-checkout set <dir>...`, `add <dir>...`, `list`, `reapply`, `disable`.

Patterns live in `$GIT_DIR/info/sparse-checkout`; config keys `core.sparseCheckout` and
`core.sparseCheckoutCone`.

```
git sparse-checkout set src/ docs/
```

## Exporting: archive and bundle

- `git archive --format=zip -o out.zip <tree-ish>` exports a tree as tar/zip, honoring
  `export-ignore` / `export-subst` (see `hooks-and-automation.md`); `--prefix=<prefix>/` nests paths.
- `git bundle create repo.bundle --all` packs refs+objects into one file for offline transfer;
  `git clone`/`git fetch` can read a bundle. Incremental: `git bundle create inc.bundle last..main`.

```
git archive --format=zip -o release.zip HEAD
git bundle create repo.bundle --all
```

## Edge cases / anti-patterns

- **Don't** treat stash as permanent storage — entries are easy to lose; commit important work.
- **Don't** expect to check out the same branch in two worktrees — Git refuses without `--force`.
- **Don't** forget submodules are pinned to a commit — update and commit the new pointer deliberately.

## Do / Don't

- **Do** use `git stash -u` when untracked files matter; `pop` vs `apply` deliberately.
- **Do** use a worktree (not a stash) when you need to build/test another branch concurrently.
- **Don't** push into a bundle — bundles are read-only sources.

## Related

`snapshotting.md` · `branching-and-merging.md` · `setup-and-config.md` · `hooks-and-automation.md`
