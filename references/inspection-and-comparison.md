# Inspection and Comparison

Reading history and comparing states. Covers `git log`, `git diff`, `git show`, `git blame`,
`git grep`, `git bisect`, and `git describe`.

**When this applies:** browsing commit history, diffing working tree/index/commits, inspecting an
object, finding who/when a line changed, searching content, or hunting the commit that introduced a
change.

## Browsing history with log

`git log [<revision-range>] [-- <path>]` lists commits. Shaping options:

- `--oneline`, `--graph`, `--decorate` for a compact visual history.
- `-p` / `--patch` (show diffs), `--stat`, `--name-status`.
- Filters: `-<n>`, `--since=<date>` / `--until=<date>`, `--author=<pattern>`, `--grep=<pattern>`,
  `--merges` / `--no-merges`, `--follow` (across renames of one file).
- "Pickaxe": `-S<string>` finds commits that add/remove a string; `-G<regex>` matches diff hunks.
- `--pretty=<format>` / `--format=<format>` (e.g. `format:%h %s`).

```
git log --oneline --graph --decorate --all
```

## Revision ranges

Many commands accept ranges: `A..B` = reachable from `B` but not `A`; `A...B` = symmetric difference;
`^<rev>` excludes. Full revision syntax lives in `refs-and-revisions.md`.

```
git log origin/main..HEAD     # local commits not yet pushed
```

## Diffing

`git diff` compares two states; the form decides which two:

| Command | Compares |
| --- | --- |
| `git diff` | working tree vs index (unstaged) |
| `git diff --cached` / `--staged` | index vs `HEAD` (staged) |
| `git diff <commit>` | working tree vs that commit |
| `git diff <c1> <c2>` | two commits |
| `git diff <c1>...<c2>` | changes on `c2` since the merge base |

Useful flags: `--stat`, `--name-only`, `-w` / `--ignore-all-space`, `--color-words`,
`-M` / `--find-renames`, `--no-index` (compare paths outside a repo).

```
git diff --staged
```

## Showing objects

`git show [<object>]` prints a commit's message and diff, a tag's message, a tree's names, or a blob's
contents. Object syntax includes `<commit>:<path>` and `<rev>^{tree}`.

```
git show HEAD
git show v1.0:README.md
```

## Who and where

- `git blame [-L <start>,<end>] <file>` annotates each line with the last commit, author, and date;
  `-L :<funcname>` scopes to a function; `-w` ignores whitespace; `-C` / `-M` detect moved/copied
  lines.
- `git grep <pattern>` searches tracked files (`--cached` searches the index, a `<tree-ish>` searches
  that tree); `-n`, `-i`, `-w`, `-l`, `-F`, `-E`, `-P`, and `-A/-B/-C` context behave like `grep`.

```
git blame -L 40,60 src/app.c
git grep -n "TODO" -- '*.py'
```

## Finding a bad commit

`git bisect` binary-searches history. Mark endpoints and let Git check out midpoints:

```
git bisect start HEAD v1.2     # HEAD bad, v1.2 good
git bisect run make test       # automate; exit 0=good, 125=skip, else bad
git bisect reset
```

`git describe` names a commit from the nearest reachable tag (e.g. `v1.0.4-14-g2414721`); `--tags`
includes lightweight tags, `--dirty` flags a modified tree.

## Edge cases / anti-patterns

- **Don't** confuse `git diff` (unstaged) with `git diff --cached` (staged) — they compare different
  pairs.
- **Don't** forget `--` before a path that could be read as a revision.
- **Don't** leave a `git bisect` session open — always `git bisect reset`.

## Do / Don't

- **Do** use `-S`/`-G` pickaxe to find when code appeared or vanished.
- **Do** automate bug-hunting with `git bisect run`.
- **Don't** rely on `git describe` without tags — it errors unless `--always`.

## Related

`snapshotting.md` · `rewriting-history.md` · `refs-and-revisions.md` · `remotes-and-sync.md`
