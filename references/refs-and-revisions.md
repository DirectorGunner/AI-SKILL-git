# Refs and Revisions

How to name commits, refs, and ranges, plus the core vocabulary. Covers `gitrevisions` syntax,
refspecs, and key `gitglossary` terms.

**When this applies:** writing a commit/range argument for `log`, `diff`, `rebase`, `reset`, etc.;
understanding `HEAD`, the index, refs, or refspecs.

## Refs

A **ref** is a name pointing to an object (or to another ref — a **symref**, like `HEAD`). Branches
live under `refs/heads/`, tags under `refs/tags/`, remote-tracking branches under `refs/remotes/`.
`HEAD` is the current branch; a **detached HEAD** points straight at a commit instead.

## Naming a single commit

| Syntax | Means |
| --- | --- |
| `<sha1>` | full hash or a unique prefix |
| `<refname>`, `@` | a ref; `@` is shorthand for `HEAD` |
| `<rev>^`, `<rev>^<n>` | first parent / n-th parent (`^0` = the commit as a commit) |
| `<rev>~<n>` | n-th ancestor by first-parent (`main~3` == `main^^^`) |
| `<rev>^{<type>}` | dereference to a type (`^{commit}`, `^{tree}`) |
| `<rev>:<path>` | the blob/tree at `<path>` in that tree-ish |
| `:<n>:<path>` | a file in the index (merge stage 0–3) |
| `<ref>@{<n>}`, `<ref>@{<date>}` | reflog: prior value, or value at a date |
| `@{-<n>}` | the n-th previously checked-out branch (`@{-1}` = last) |
| `<branch>@{upstream}` / `@{u}`, `@{push}` | the upstream / push target |

Special pseudo-refs include `ORIG_HEAD` (pre-reset/rebase/merge), `MERGE_HEAD`, `FETCH_HEAD`.

```
git show HEAD~2          # two commits back
git diff @{u}...HEAD     # since the merge base with upstream
```

## Naming ranges

| Syntax | Means |
| --- | --- |
| `<rev>` | reachable from `<rev>` |
| `^<rev>` | exclude reachable-from-`<rev>` |
| `<r1>..<r2>` | in `r2` but not `r1` (= `^r1 r2`) |
| `<r1>...<r2>` | symmetric difference |
| `<rev>^@` | all parents of `<rev>` |
| `<rev>^!` | just `<rev>`, excluding its parents |

```
git log main..feature   # commits on feature not yet on main
```

## Refspecs

Fetch/push map refs with `[+]<src>:<dst>`; a leading `+` allows non-fast-forward updates. The default
clone fetch refspec is `+refs/heads/*:refs/remotes/origin/*`. A `^<pattern>` excludes. See
`remotes-and-sync.md`.

## Core vocabulary

- **object** — immutable storage unit keyed by SHA-1: **blob** (file bytes), **tree** (directory),
  **commit** (a history point), tag object.
- **commit-ish** / **tree-ish** — anything dereferenceable to a commit / tree.
- **index** — the staging area (a stored working tree, also holding merge stages).
- **working tree** — the checked-out files.
- **fast-forward** — advancing a branch pointer to a descendant with no merge commit.
- **upstream branch** — the branch your branch merges/rebases against (`branch.<name>.remote` +
  `branch.<name>.merge`).
- **reflog** — local history of where a ref has pointed.

## Edge cases / anti-patterns

- **Don't** confuse `^` (parent) with `~` (ancestor): `A^2` is the second *parent*, `A~2` is two
  commits back on the first-parent line.
- **Don't** read `A..B` as a file range — it's a commit range; use `--` to separate paths.
- **Don't** assume `@{1}` and `@{-1}` mean the same — one is a reflog entry, the other a prior branch.

## Do / Don't

- **Do** use `@{u}` / `@{push}` to compare against upstream without typing remote names.
- **Do** use `<rev>:<path>` to read a file from any commit.
- **Don't** rely on short SHA prefixes in scripts — they can become ambiguous; use full hashes.

## Related

`inspection-and-comparison.md` · `remotes-and-sync.md` · `plumbing.md` · `branching-and-merging.md`
