# claude-attribution-guard

Two [pre-commit](https://pre-commit.com) hooks that block commits and pushes
carrying Claude Code's attribution — `Co-Authored-By: Claude ...` /
`Claude-Session:` trailers, or a "Generated with Claude Code" footer — for
anyone who doesn't want that showing up in their history.

`no-claude-attribution-commit-msg` catches it at commit time.
`no-claude-attribution-pre-push` is a backstop for commits that got a bad
message another way (amend, rebase, cherry-pick from elsewhere, or a local
`--no-verify` commit).

## Usage

Requires the [pre-commit](https://pre-commit.com) tool
([install instructions](https://pre-commit.com/#install)). Add to your
repo's `.pre-commit-config.yaml`:

```yaml
repos:
  - repo: https://github.com/ca1ebd/claude-attribution-guard
    rev: v1.0.0
    hooks:
      - id: no-claude-attribution-commit-msg
      - id: no-claude-attribution-pre-push
```

Then, once per clone:

```bash
pre-commit install --hook-type commit-msg --hook-type pre-push
```

(Plain `pre-commit install` only wires up the default `pre-commit` stage —
these hooks run at `commit-msg` and `pre-push`, so both `--hook-type` flags
are needed.)

## Without pre-commit

The two scripts under `hooks/` are plain POSIX-ish shell and work as raw git
hooks too, no dependency on the pre-commit tool at all. Copy `hooks/lib.sh`,
`hooks/commit-msg`, and `hooks/pre-push` into `.git/hooks/` (or a tracked
directory pointed at by `git config core.hooksPath`) and skip the
`.pre-commit-hooks.yaml` plumbing entirely.
