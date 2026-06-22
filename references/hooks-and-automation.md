# Hooks and Attributes

Customizing Git's behavior with event hooks and per-path attributes. Covers `githooks` and
`gitattributes`.

**When this applies:** running checks on commit/push, enforcing policy server-side, controlling
line-ending normalization, diff/merge drivers, content filters, or `git archive` export rules; or
when editing files under `.git/hooks/`, `.gitattributes`, or `.git/info/attributes`.

## Hooks

Hooks are executable programs Git runs at lifecycle events. They live in `$GIT_DIR/hooks/` (i.e.
`.git/hooks/`) unless `core.hooksPath` redirects them; they must have the executable bit set, and the
shipped samples are disabled by their `.sample` suffix.

### Client-side hooks

| Hook | Fires |
| --- | --- |
| `pre-commit` | before a commit is created (lint/format gate) |
| `prepare-commit-msg` | before the message editor opens |
| `commit-msg` | to validate/normalize the message |
| `post-commit` | after a commit (notification) |
| `pre-rebase` | before a rebase starts |
| `post-checkout` | after `checkout`/`switch`/`clone` updates the tree |
| `post-merge` | after a merge (and `git pull`) |
| `pre-push` | before refs are sent |
| `post-rewrite` | after `commit --amend` / `rebase` rewrite commits |

### Server-side hooks

`pre-receive` (once per push), `update` (once per ref), `post-receive`, and `post-update` run on the
receiving repository.

### Exit status

A **non-zero exit from a `pre-*` hook aborts** the operation (`pre-commit`, `commit-msg`,
`pre-rebase`, `pre-push`, `pre-receive`, `update`, …). `post-*` hooks are notifications — their exit
code does not change the result. Some commit hooks are skippable with `--no-verify`.

```bash
# .git/hooks/pre-commit  (chmod +x)
#!/bin/sh
npm run lint || exit 1   # non-zero blocks the commit
```

## Attributes

`.gitattributes` assigns behavior to paths. Lookup order (highest first):
`$GIT_DIR/info/attributes`, nearer `.gitattributes` files, `core.attributesFile`, then the system
file. Each line is `pattern attr...`; states are `attr` (set), `-attr` (unset), `attr=value`, and
`!attr` (reset to unspecified). Later lines win per attribute.

Key built-in attributes:

- `text`, `text=auto`, `-text`, and `eol=lf` / `eol=crlf` — line-ending normalization.
- `diff` / `diff=<driver>`, `-diff` (treat as binary).
- `merge` / `merge=<driver>` (built-in 3-way uses `<<<<<<<` `=======` `>>>>>>>` markers); `union`.
- `filter=<driver>` — `clean` on check-in, `smudge` on checkout.
- `export-ignore` / `export-subst` — control `git archive` output; `ident` — expand `$Id$`.
- `binary` — macro for `-diff -merge -text`.

```
*            text=auto
*.sh         text eol=lf
*.png        binary
*.bin        export-ignore
```

Query effective attributes with `git check-attr`.

## Edge cases / anti-patterns

- **Don't** assume hooks are shared — `.git/hooks/` is not committed; distribute via `core.hooksPath`
  or a tracked directory.
- **Don't** forget the executable bit (and on Windows, a proper shebang) or the hook is silently
  skipped.
- **Don't** rely on `--no-verify` for policy — server-side `pre-receive`/`update` hooks can't be
  bypassed by clients.

## Do / Don't

- **Do** enforce critical policy in server-side hooks, not just client-side.
- **Do** set `* text=auto` (plus explicit `eol`) to keep line endings sane cross-platform.
- **Don't** put secrets in hook scripts or attributes files.

## Related

`snapshotting.md` · `setup-and-config.md` · `recovery-and-maintenance.md` · `remotes-and-sync.md`
