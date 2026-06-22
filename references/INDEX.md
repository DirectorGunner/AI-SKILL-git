# Git References — Index

Start here. Each reference is a self-contained, original, task-organized guide. Load only what the
task needs (progressive disclosure). Metadata: `topics.json` (topic → file + keywords). Identifiers
(command names, flags, config keys, syntax) are preserved verbatim from the official Git docs.

| Reference | One-line summary | Read this when… |
| --- | --- | --- |
| [setup-and-config.md](setup-and-config.md) | `init`/`clone`, layered `git config` scopes and keys, `.gitignore` | starting or configuring a repository |
| [snapshotting.md](snapshotting.md) | The index, `add`/`status`/`commit`, `restore`, `rm`/`mv`, `clean` | staging, committing, or discarding changes |
| [branching-and-merging.md](branching-and-merging.md) | `branch`, `switch`/`checkout`, `merge`, conflicts, `tag` | working with branches, merges, or tags |
| [rewriting-history.md](rewriting-history.md) | `--amend`, `reset` modes, `rebase`, `cherry-pick`, `revert`, reflog | changing or undoing existing commits |
| [inspection-and-comparison.md](inspection-and-comparison.md) | `log`, `diff`, `show`, `blame`, `bisect`, `grep`, `describe` | browsing history or comparing states |
| [remotes-and-sync.md](remotes-and-sync.md) | `remote`, `fetch`, `pull`, `push`, upstream, refspecs | exchanging work with other repositories |
| [refs-and-revisions.md](refs-and-revisions.md) | revision/range syntax, refs, refspecs, glossary terms | naming commits/ranges or learning the vocabulary |
| [hooks-and-automation.md](hooks-and-automation.md) | `githooks` events, `.gitattributes` (eol/diff/merge/filter/export) | customizing Git behavior per event or per path |
| [recovery-and-maintenance.md](recovery-and-maintenance.md) | `reflog` recovery, `fsck`, `gc`, `prune` | recovering lost work or maintaining the repo |
| [stash-and-worktrees.md](stash-and-worktrees.md) | `stash`, `worktree`, `submodule`, `sparse-checkout`, `archive`, `bundle` | shelving work, multi-checkout, nesting, or export |
| [plumbing.md](plumbing.md) | `rev-parse`, `cat-file`, `hash-object`, `ls-files`/`ls-tree`, `update-ref`, `rev-list`, `for-each-ref`, `merge-base`, `check-ignore` | scripting or inspecting Git's object model |

## Reading paths

- **Everyday workflow:** setup-and-config → snapshotting → branching-and-merging →
  remotes-and-sync.
- **Fixing/undoing:** rewriting-history → recovery-and-maintenance (reflog first).
- **Understanding what a command means:** refs-and-revisions → inspection-and-comparison.
- **Automation/scripting:** plumbing → refs-and-revisions → hooks-and-automation.

## Safety notes (reconciled in the references)

- **Destructive commands:** `reset --hard`, `clean -f`, `push --force`, and history rewrites can lose
  work — each reference names the safer alternative (`--force-with-lease`, `git revert`, reflog
  recovery, `-n` dry runs).
- **Published history:** amend/reset/rebase only on unshared commits; use `git revert` for commits
  already pushed.
