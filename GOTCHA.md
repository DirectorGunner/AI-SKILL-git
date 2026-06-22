# Git — Gotchas

Recurring failure modes when relying on the Git reference, and what to do instead. Read alongside `SKILL.md`.

- Treat command names, flags, refspecs, and config keys as exact; Git behavior is version- and config-sensitive — verify against the installed Git version and the repo's `.git/config` before relying on a flag.
- Many commands rewrite history or are destructive (`reset --hard`, `push --force`, `rebase`, `clean -fd`); confirm intent and prefer reversible alternatives — the reference describes behavior, not authorization to run them.
- A command's effect depends on the current branch, index, and worktree, and porcelain differs from plumbing; confirm the repository state before acting.
- If a command, flag, or option is not in the references, say so and mark it `TODO` — never invent one.