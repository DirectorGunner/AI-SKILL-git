# Remotes and Syncing

Exchanging work with other repositories. Covers `git remote`, `git fetch`, `git pull`, `git push`,
and refspecs.

**When this applies:** adding/inspecting remotes, downloading updates, integrating upstream changes,
publishing commits, or working with refspecs and upstream tracking.

## Managing remotes

A *remote* is a named URL whose branches you track under `refs/remotes/<name>/`. `git remote`
subcommands:

- `git remote add <name> <URL>` (e.g. `origin`); `-f` fetches immediately; `-t <branch>` limits
  tracked branches.
- `git remote -v` lists with URLs; `git remote show <name>` details one.
- `git remote rename <old> <new>`, `git remote remove <name>`.
- `git remote set-url [--push] <name> <newurl>`, `git remote get-url <name>`.
- `git remote prune <name>` drops stale remote-tracking refs.

```
git remote add origin https://example.com/repo.git
```

Key config: `remote.<name>.url`, `remote.<name>.fetch`.

## Fetching

`git fetch [<repository> [<refspec>]]` downloads objects and updates remote-tracking branches (e.g.
`refs/remotes/origin/main`) **without** touching your branch or working tree. Useful flags: `--all`,
`-p` / `--prune` (drop deleted remote refs), `-t` / `--tags`, `--depth=<depth>` / `--unshallow`,
`--dry-run`. Results are also recorded in `FETCH_HEAD`.

```
git fetch --all --prune
```

### Refspecs

A refspec maps source to destination refs: `[+]<src>:<dst>`. The leading `+` allows non-fast-forward
updates. The default fetch refspec is `+refs/heads/*:refs/remotes/origin/*`. A `^<pattern>` is a
negative (exclude) refspec.

## Pulling

`git pull` is `git fetch` **plus** integration of the upstream branch. By default it merges; with
`--rebase` (or `pull.rebase=true`) it rebases your commits on top instead. `--ff-only` refuses to
create a merge when histories diverge; `--autostash` shelves local changes around the operation.

```
git pull --rebase origin main
# equivalent to: git fetch origin && git rebase origin/main
```

## Pushing

`git push [<repository> [<refspec>]]` uploads local commits and refs. Common flags:

- `-u` / `--set-upstream` records the upstream so later `git push`/`git pull` need no arguments.
- `--tags` / `--follow-tags` publish tags (not sent by default); `--delete` removes a remote ref.
- `--force-with-lease` safely overwrites only if the remote still matches what you last saw — prefer
  it over `--force`. `--atomic` makes a multi-ref push all-or-nothing.

```
git push -u origin main
git push origin --delete stale-branch
```

`push.default` (default `simple`) controls behavior with no refspec; deletion syntax is
`git push origin :branch` or `--delete`.

## Edge cases / anti-patterns

- **Don't** assume `git fetch` changes your working tree — it only updates remote-tracking refs; use
  `git pull` or a manual merge/rebase to integrate.
- **Don't** use `--force`; reach for `--force-with-lease` to avoid clobbering others' work.
- **Don't** expect tags to travel with a normal push — pass `--tags` / `--follow-tags`.

## Do / Don't

- **Do** `git fetch` + review before integrating on shared branches.
- **Do** set upstream with `-u` on the first push of a branch.
- **Don't** `git pull` with uncommitted conflicting changes — stash or commit first (or `--autostash`).

## Related

`branching-and-merging.md` · `setup-and-config.md` · `refs-and-revisions.md` · `rewriting-history.md`
