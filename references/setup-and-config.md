# Setup and Configuration

Creating repositories and controlling Git's behavior through configuration. Covers `git init`,
`git clone`, the layered `git config` system, and `.gitignore`.

**When this applies:** starting a repository, cloning an existing one, setting identity or behavior
options, or deciding which files Git should ignore.

## Creating a repository

`git init [<directory>]` makes a new repository, writing a `.git` directory (objects, `refs/heads`,
`refs/tags`, templates). It is safe to re-run in an existing repository. Useful flags:

- `--bare` — a repository with no working tree, for sharing/serving.
- `-b <name>` / `--initial-branch=<name>` — name the first branch (otherwise the built-in default,
  currently `master`, or `init.defaultBranch`).
- `--separate-git-dir=<git-dir>` — store the real repo elsewhere; the working tree gets a `.git` file.
- `--template=<template-directory>`, `--object-format=<format>` (`sha1`/`sha256`).

```
git init -b main my-project
```

## Cloning

`git clone <repository> [<directory>]` copies a repository and sets up `refs/remotes/origin/`
tracking. Common flags:

- `-b <name>` / `--branch=<name>` — check out a specific branch (or tag).
- `-o <name>` / `--origin=<name>` — name the remote something other than `origin`.
- `--depth <depth>` — shallow clone with truncated history; `--single-branch` follows.
- `--bare`, `--mirror` (implies `--bare`, maps all refs).
- `--recurse-submodules[=<pathspec>]`, `--filter=<filter-spec>` (partial clone), `--sparse`.

```
git clone --depth 1 https://example.com/repo.git
```

## Configuration scopes

`git config` reads and writes key/value pairs in INI-style files. Scope flags select the file;
**writes default to `--local`**, reads consult all files in order:

| Scope | Flag | File |
| --- | --- | --- |
| system | `--system` | `$(prefix)/etc/gitconfig` |
| global | `--global` | `~/.gitconfig` or `$XDG_CONFIG_HOME/git/config` |
| local | `--local` | `.git/config` |
| worktree | `--worktree` | `$GIT_DIR/config.worktree` (needs `extensions.worktreeConfig`) |

Precedence runs system → global → local → worktree → command-line (`-c`). Useful operations:
`git config set <name> <value>`, `git config get [--all] <name>`, `git config list`,
`git config unset <name>`, and `--show-origin` / `--show-scope` to see where a value came from.

```
git config --global user.name "A U Thor"
git config --global user.email "author@example.com"
```

Frequently set keys: `user.name`, `user.email`, `core.editor`, `core.filemode`, `init.defaultBranch`,
`color.ui`, and `alias.*` (e.g. `alias.co=checkout`).

## Ignoring files

Untracked files are excluded by `.gitignore` patterns. Sources, in precedence order: command-line
patterns, per-directory `.gitignore` files (nearer files override), `$GIT_DIR/info/exclude`, and the
user file named by `core.excludesFile` (default `$XDG_CONFIG_HOME/git/ignore`). The **last matching
pattern wins**. Syntax: `#` comment, blank line ignored, `*` (not `/`), `?`, `[a-z]` ranges, leading
`!` re-includes, trailing `/` matches directories, a leading/middle `/` anchors to the file's
location, and `**` spans directories.

```
node_modules/
*.log
!important.log
```

Already-tracked files are unaffected by `.gitignore`; stop tracking one with `git rm --cached`
(see `snapshotting.md`).

## Edge cases / anti-patterns

- **Don't** commit secrets to `.git/config` or files; ignoring a file does not untrack it if already
  committed.
- **Don't** assume the default branch is `main` — it depends on `init.defaultBranch`.
- **Don't** use `--global` inside a repo expecting per-repo effect; that is `--local`.

## Do / Don't

- **Do** set `user.name`/`user.email` (globally or per-repo) before committing.
- **Do** prefer `--depth`/`--single-branch` for large repos you only need shallowly.
- **Don't** edit `.git/config` by hand when `git config` can do it safely.

## Related

`snapshotting.md` · `remotes-and-sync.md` · `stash-and-worktrees.md` · `refs-and-revisions.md`
