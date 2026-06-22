---
name: git
description: >-
  Use when working with Git version control - running or explaining git commands, subcommands,
  options/flags, refspecs, or configuration keys; staging and committing; branching, switching,
  merging, rebasing, cherry-picking, reverting, or resetting; stashing; tags; inspecting history with
  log/diff/show/blame/bisect/grep; remotes, fetch/pull/push and upstream tracking; revision and range
  syntax (HEAD~, @{}, A..B); hooks and .gitattributes; submodules, worktrees, sparse-checkout;
  recovery with reflog/fsck and maintenance with gc; or low-level plumbing for scripts. Also fires
  when editing .gitignore, .gitattributes, .git/config, or files under .git/hooks/, when reading a
  git error or conflict markers, or when choosing the right command for a version-control task - even
  when the user never says "git" by name. Gives faithful, task-routed reference with worked examples.
covers:
  - reflog
  - refspec
  - init
  - clone
  - config
  - gitignore
  - user.name
  - scopes
  - global
  - core.excludesfile
  - add
  - status
---

# Git

Task-organized, faithful guidance for Git command-line work: which command to reach for, the exact
flags and config keys, revision syntax, and the safe way to do destructive operations. Content is
original; identifiers (command names, flags, config keys, numbers) are preserved exactly.

## When to use this skill

Reach for this skill whenever a task touches Git: choosing or explaining a command; staging,
committing, or undoing changes; branching/merging/rebasing; remotes and push/pull; reading history;
revision/range syntax; hooks and attributes; submodules/worktrees/sparse-checkout; recovery and
maintenance; or scripting with plumbing — including when the user opens `.gitignore`,
`.gitattributes`, `.git/config`, or a hook, or pastes a git error or conflict markers, without
naming "git".

## How to use it (runtime workflow)

1. Read this file, then consult the **task router** below (or `references/topics.json`).
2. Open only the one or two reference files relevant to the task — do not read everything
   (progressive disclosure).
3. Answer strictly from the reference; preserve command/flag/config names exactly as written.
4. If something is not covered by the references, say so and mark it `TODO` — never invent a flag,
   option, or config key.
5. For destructive actions (`reset --hard`, `push --force`, `clean -f`, history rewrites), prefer the
   safe alternative the reference names and confirm intent first.

## Coordination with terminal skills

Use this Git skill for Git semantics: refs, history, staging, commits, remotes, refspecs,
force-with-lease, bundles, archives, worktrees, recovery, and which Git command is safe for the task.

When the task also requires running Git from Windows PowerShell, pair this skill with
`powershell-vsdevshell` for shell mechanics only: native command invocation, quoting, argument arrays,
paths, exit-code capture, and log routing. Load the Git skill first for the Git decision, then use the
PowerShell skill to execute that decision correctly in the terminal.

Do not substitute terminal guidance for Git safety guidance. For destructive or history-rewriting Git
operations, read the relevant Git reference file, preserve recoverability first, and honor explicit
user constraints such as "no fetch" or "do not push".

## Task router

| If the task is about… | Read |
| --- | --- |
| `git init`/`git clone`, `git config` scopes and keys, identity, `.gitignore` | `references/setup-and-config.md` |
| staging (`add`), `status`, `commit`, `restore`, unstaging, `rm`/`mv`, `clean` | `references/snapshotting.md` |
| `branch`, `switch`/`checkout`, `merge`, conflicts, `tag` | `references/branching-and-merging.md` |
| `rebase` (incl. interactive), `reset` modes, `--amend`, `cherry-pick`, `revert`, `reflog` recovery | `references/rewriting-history.md` |
| `log`, `diff`, `show`, `blame`, `bisect`, `grep`, `describe` | `references/inspection-and-comparison.md` |
| `remote`, `fetch`, `pull`, `push`, upstream tracking, refspecs | `references/remotes-and-sync.md` |
| revision/range syntax (`HEAD~`, `^`, `@{}`, `A..B`), refs, glossary terms | `references/refs-and-revisions.md` |
| `githooks`, `.gitattributes`, line-ending/filter/merge drivers, export rules | `references/hooks-and-automation.md` |
| `reflog` recovery, `fsck`, `gc`, `prune`, lost commits, repo maintenance | `references/recovery-and-maintenance.md` |
| `stash`, `worktree`, `submodule`, `sparse-checkout`, `archive`, `bundle` | `references/stash-and-worktrees.md` |
| low-level/plumbing (`rev-parse`, `cat-file`, `hash-object`, `ls-files`, `update-ref`, `rev-list`, `for-each-ref`, `merge-base`, `check-ignore`) | `references/plumbing.md` |

## Gotchas

Recurring failure modes and what to do instead live in the sibling [GOTCHA.md](GOTCHA.md).

## Verification notes

Validate the package structure (the validator ships with the skill-drafting skill; paths use the `DEVROOT` env var — adjust to your layout):

```powershell
python "$env:DEVROOT\SKILLS\skills\skill-drafting\scripts\validate_skill_package.py" "$env:DEVROOT\SKILLS\skills\git"
```

This package is built and checked with the three-layer method (Spec → Verifier → Environment).
Ground-truth checks live in `AI/work/verify_git.py`: every reference is listed in
`references/INDEX.md`, has exactly one `references/topics.json` entry, resolves its cross-links, and
contains a summary, a "When this applies" line, at least one example, and a do/don't or anti-patterns
section. The verifier also greps a preserve-glossary of load-bearing identifiers (command names,
flags, config keys, syntax tokens) against the shipped references so accidental renames are caught,
and enforces that no source branding/attribution leaks into the original content. Keep those
invariants when editing.

## Reference map

Start at `references/INDEX.md`. Metadata: `references/topics.json` (topic → file + keywords).

## Official documentation

Includes original content written for this skill. The official documentation is at https://git-scm.com/docs — consult it to verify anything uncertain, conflicting, or version-specific.
