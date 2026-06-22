# Git Version Control

> _Task-routed, faithful reference for Git command-line work — choosing the right command, exact flags and config keys, revision syntax, and the safe way to do destructive operations._

Part of **[Agent Kaizen](https://github.com/DirectorGunner/agent-kaizen)** — Agent Kaizen is designed to support reliable AI agent workflows, context engineering, and AI systems engineering in VS Code, Codex, and Claude Code. Build reusable agent skills, reduce unnecessary context loading, add validation loops, and apply Spec → Verifier → Environment scaffolding to new and existing projects.

This repository is the `git` skill: a reusable, trigger-rich task handbook that an AI coding agent (OpenAI Codex, Claude Code) loads on demand when a task matches its triggers.

## What this skill covers

This skill spans everyday and advanced Git: running or explaining commands, subcommands, options/flags, refspecs, and configuration keys; staging and committing; branching, switching, merging, rebasing, cherry-picking, reverting, and resetting; stashing and tags; inspecting history with `log`, `diff`, `show`, `blame`, `bisect`, and `grep`; remotes, `fetch`/`pull`/`push`, and upstream tracking. It also covers revision and range syntax (`HEAD~`, `@{}`, `A..B`), hooks and `.gitattributes`, submodules, worktrees, and sparse-checkout, recovery with `reflog`/`fsck`, maintenance with `gc`, and low-level plumbing for scripts. It fires even without the word "git" — when you edit `.gitignore`, `.gitattributes`, `.git/config`, or a hook, or read a git error or conflict markers. Destructive operations (`reset --hard`, `push --force`, `clean -fd`, history rewrites) are flagged so the agent prefers the reversible alternative and confirms intent first.

## What's inside

- `SKILL.md` — frontmatter (`name` + trigger-rich `description`) and a lean body.
- `references/` — right-sized topic files the agent loads only when relevant (plus `INDEX.md` and `topics.json`).
- `GOTCHA.md` — known pitfalls and edge cases.

## Use it

This skill is one git repo inside the Agent Kaizen **skills store**. The store nests two folders on purpose: the outer **`SKILLS\`** is a VS Code project for building and maintaining skills (its own workspace + tooling), and the inner lowercase **`skills\`** holds every skill as its own repo. That split lets a project pull skills two ways — the **whole `skills\` folder at once** (loads everything — **not recommended**) or **one skill at a time** (recommended: load only what a task needs and stay under Claude Code's skill-listing budget).

Paths below use **`%DEVROOT%`** — the `DEVROOT` environment variable pointing at the folder that contains `SKILLS\`. Set it once by running **`SetDevRoot.cmd`** in the SKILLS repo root (no admin); a new shell then resolves `%DEVROOT%` automatically.

Link **just this skill** (recommended) — a Windows directory junction, no admin:

```cmd
mklink /J .agents\skills\git  "%DEVROOT%\SKILLS\skills\git"
mklink /J .claude\skills\git  "%DEVROOT%\SKILLS\skills\git"
```

Or link the **whole store** at once (loads every skill — not recommended outside a skills-dev project):

```cmd
mklink /J .agents\skills  "%DEVROOT%\SKILLS\skills"
mklink /J .claude\skills  "%DEVROOT%\SKILLS\skills"
```

Remove a link (the store copy is untouched):

```cmd
rmdir .agents\skills\git
rmdir .claude\skills\git
```

The agent (OpenAI Codex, Claude Code) then auto-loads this skill whenever a task matches its triggers. Built and validated with **[skill-drafting](https://github.com/DirectorGunner/AI-skill-drafting)** to the Agent Kaizen gold standard.

## License

Licensed under **AGPL-3.0**, matching the [Agent Kaizen](https://github.com/DirectorGunner/agent-kaizen) project.
