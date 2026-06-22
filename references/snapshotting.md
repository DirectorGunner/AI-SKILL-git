# Snapshotting Changes

Recording work into commits and managing the staging area (the index). Covers `git add`,
`git status`, `git commit`, `git restore`, `git reset` (path form), `git rm`, `git mv`, and
`git clean`.

**When this applies:** staging or unstaging changes, committing, checking working-tree state, or
discarding/cleaning local modifications.

## The index (staging area)

Git records the *index* into the next commit, not your working tree directly. The cycle is: edit →
stage → commit. `git status` shows the split — its short format uses two columns `XY` where `X` is
the staged (index) state and `Y` is the unstaged (working tree) state (`M` modified, `A` added,
`D` deleted, `R` renamed, `??` untracked, `!!` ignored).

```
git status -sb
```

## Staging with git add

- `git add <pathspec>` stages current content; re-run after further edits.
- `-A` / `--all` stages additions, modifications, **and** deletions; `-u` / `--update` limits to
  already-tracked paths.
- `-p` / `--patch` stages selected hunks interactively; `-n` / `--dry-run` previews; `-f` / `--force`
  adds ignored files; `-N` / `--intent-to-add` records a path for later.

```
git add -p
```

## Committing

`git commit` records the staged index. Key flags:

- `-m <msg>` / `--message=<msg>` (repeat for paragraphs); `-a` / `--all` auto-stages modified and
  deleted **tracked** files (not new ones).
- `--amend` rewrites the tip commit (don't amend already-pushed history); `--no-edit` keeps the
  message.
- `-s` / `--signoff`, `-S` / `--gpg-sign`, `--allow-empty`, `--no-verify` (skip `pre-commit` /
  `commit-msg` hooks).

Convention: a short summary line, a blank line, then the body. Identity comes from `user.name` /
`user.email` unless `GIT_AUTHOR_*` / `GIT_COMMITTER_*` are set.

```
git commit -m "Fix off-by-one in parser"
```

## Undoing in the working tree and index

`git restore` is the modern, file-scoped tool:

- `git restore <path>` restores the working tree **from the index** (discards unstaged edits).
- `git restore --staged <path>` restores the index **from HEAD** (unstages).
- `git restore --source=<tree> <path>` pulls content from any commit; `-p` selects hunks.

`git reset` (path form) `git reset [<tree-ish>] -- <path>` updates only the staged version (default
from `HEAD`) — the opposite of `git add`, equivalent to `git restore --staged`. The commit-moving
forms (`--soft`/`--mixed`/`--hard`) live in `rewriting-history.md`.

```
git restore --staged file.txt   # unstage
git restore file.txt            # discard unstaged edits
```

## Removing and moving

- `git rm <path>` deletes from **both** the working tree and index; `--cached` removes from the index
  only (keeps the file on disk — use to stop tracking); `-r` recurses.
- `git mv <source> <dest>` renames/moves and updates the index in one step.

```
git rm --cached secret.env
git mv old.txt new.txt
```

## Cleaning untracked files

`git clean` deletes untracked files; `-f` / `--force` is required by default (`clean.requireForce`).
`-d` includes untracked directories; `-n` / `--dry-run` previews; `-x` also removes **ignored** files
while `-X` removes **only** ignored files; `-e <pattern>` adds an exclude.

```
git clean -nd      # preview, then:
git clean -fd
```

## Edge cases / anti-patterns

- **Don't** confuse `-x` (untracked **and** ignored) with `-X` (ignored **only**) — they differ.
- **Don't** `--amend` or hard-reset commits you have already pushed.
- **Don't** run `git clean -f` without a `-n` preview; deletions are not recoverable via Git.

## Do / Don't

- **Do** stage selectively with `git add -p` to make focused commits.
- **Do** use `git restore`/`git restore --staged` instead of overloaded `git checkout` for files.
- **Don't** rely on `-a` to include brand-new files — it stages tracked changes only.

## Related

`setup-and-config.md` · `rewriting-history.md` · `branching-and-merging.md` ·
`inspection-and-comparison.md`
