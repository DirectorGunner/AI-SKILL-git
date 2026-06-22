# Plumbing

Low-level commands for scripting and inspecting Git's object model. Covers `git rev-parse`,
`git cat-file`, `git hash-object`, `git ls-files`, `git ls-tree`, `git update-ref`, `git rev-list`,
`git for-each-ref`, `git merge-base`, and `git check-ignore`.

**When this applies:** writing scripts/automation over Git internals, inspecting objects/refs, or
debugging what porcelain commands do under the hood. For everyday work prefer the porcelain commands
in the other references.

## Parsing arguments and repo info

`git rev-parse` turns revision/ref expressions into object names and reports repository structure:

- `--verify` (resolve exactly one to a SHA), `--short[=<n>]`, `--abbrev-ref` (short ref name).
- `--git-dir`, `--show-toplevel`, `--show-prefix`, `--is-inside-work-tree`, `--is-bare-repository`.

```
git rev-parse --abbrev-ref HEAD     # current branch name
git rev-parse --show-toplevel       # repo root
```

## Reading objects

- `git cat-file` inspects objects: `-t` (type), `-s` (size), `-p` (pretty-print contents), `-e`
  (exists), plus `--batch` / `--batch-check` for bulk queries.
- `git hash-object` computes an object ID; `-w` also writes the blob into the database; `--stdin`
  reads from input; `-t <type>` sets the type.

```
git cat-file -p HEAD:src/app.c      # file contents at a commit
git hash-object -w newfile.txt      # store and print the blob id
```

## Index and trees

- `git ls-files` lists files known to the index/working tree: `--cached` (default, tracked),
  `--others` (untracked), `--ignored` (with `--exclude-standard`), `--stage` (mode/object/stage),
  `--deleted`, `--modified`.
- `git ls-tree <tree-ish>` lists a tree's entries: `-r` (recurse), `-d` (trees only), `-l` (sizes),
  `--name-only`.

```
git ls-files --others --exclude-standard   # untracked, respecting ignores
git ls-tree -r --name-only HEAD
```

## Refs and traversal

- `git update-ref <ref> <new-oid> [<old-oid>]` updates a ref safely (optionally verifying the old
  value); `-d` deletes; `-m <reason>` writes a reflog message; `--stdin` batches atomically.
- `git for-each-ref` iterates refs by pattern with `--sort=<key>` and `--format=<format>` fields like
  `%(refname:short)`, `%(objectname)`, `%(upstream)`, `%(committerdate)`.
- `git rev-list` is the commit-graph walker behind `git log`: `--count`, `--all`, `--no-merges`,
  `--first-parent`, `--objects`, and ranges `A..B` / `A...B`.
- `git merge-base <a> <b>` finds the common ancestor for a three-way merge; `--is-ancestor <a> <b>`
  answers ancestry via exit status (0 = yes, 1 = no); `--all` lists all bases.

```
git rev-list --count HEAD                          # number of commits
git for-each-ref --sort=-committerdate refs/heads  # branches by recency
git merge-base --is-ancestor v1.0 HEAD && echo yes
```

## Debugging ignores

`git check-ignore -v <path>` reports whether a path is ignored and which pattern/source decided it.
Exit status: `0` = at least one path ignored, `1` = none, `128` = error.

```
git check-ignore -v build/output.o
# .gitignore:5:*.o    build/output.o
```

## Edge cases / anti-patterns

- **Don't** parse porcelain output in scripts when a plumbing command (or `--porcelain` /
  `for-each-ref --format`) gives stable, machine-readable output.
- **Don't** hand-edit refs files — use `git update-ref` so the reflog and locking stay correct.
- **Don't** assume `--is-ancestor` prints output — it communicates only through its exit code.

## Do / Don't

- **Do** use `git rev-parse` / `git for-each-ref` / `git rev-list` as scripting building blocks.
- **Do** use `git cat-file -p` to read any object by name.
- **Don't** reach for plumbing when a porcelain command already does the job clearly.

## Related

`refs-and-revisions.md` · `inspection-and-comparison.md` · `recovery-and-maintenance.md` ·
`setup-and-config.md`
