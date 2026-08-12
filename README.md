# Harness for Codex

A language-agnostic repository harness for OpenAI Codex, coding agents,
Claude Code, and Cursor. It provides durable `AGENTS.md` instructions,
standard automation entrypoints, task templates, verification scripts, and
lightweight workflow docs for agent-driven software work.

Use this repository as a Codex harness, `AGENTS.md` template, or reusable
coding-agent project scaffold when you want every future task to start from a
predictable baseline:

- one place for agent instructions through `AGENTS.md`
- one bootstrap command
- one check command
- one test command
- one full evaluation command
- optional hooks and devcontainer metadata
- lightweight documentation for decisions and tasks

## Use Cases

- Start a new repository with Codex-ready agent instructions.
- Standardize coding-agent workflows across OpenAI Codex, Claude Code, and Cursor.
- Give automation agents stable commands for setup, checks, tests, and handoff evaluation.
- Keep project decisions and task briefs in predictable locations.
- Keep multi-target projects (app builds, website, admin console) from editing
  the wrong surface, and from reporting a deploy that never shipped.
- Add a language stack later without replacing the harness contract.

## Quick Start

```sh
scripts/bootstrap
scripts/check
scripts/doctor
```

## What This Provides

- `AGENTS.md`: repository-local operating instructions for Codex and compatible agents.
- `CLAUDE.md`: Claude Code bridge that imports the shared `AGENTS.md` guidance.
- `scripts/bootstrap`: dependency preparation when a known stack is present.
- `scripts/check`: lint, format, type, and test checks when available.
- `scripts/test`: focused test-suite entrypoint.
- `scripts/eval`: complete handoff verification through doctor, bootstrap, and check.
- `scripts/surface`: deploy-surface targeting, containment, and ship confirmation.
- `tasks/TEMPLATE.md`: task brief template for work that needs durable context.
- `docs/decisions.md`: durable decisions future agents should preserve.

## Deploy Surfaces

Most projects ship to more than one target — a mobile build, a website, an
admin console — through deploy commands that look alike. Two things decide
whether a task actually succeeded, and neither was checked: *which target the
change was for*, and *whether the deploy shipped anything*. A deploy command
exiting 0 proves only that the command ran.

Declare the targets once, in `surfaces.yml`:

```sh
cp surfaces.example.yml surfaces.yml
scripts/surface list
scripts/surface check web    # fails if the diff escapes the surface
scripts/surface run web      # verify, deploy, then confirm it is live
```

Each surface declares the paths it owns, the paths it must never touch, the one
command that deploys it, and a `confirm` command that proves the new version is
actually live. A surface's `root` may be a subdirectory or a separate checkout,
so this works for a monorepo and for sibling repositories alike.

`SURFACE=web scripts/check` folds containment into normal verification, and
`tasks/TEMPLATE.md` records the surface so it survives context compaction.
Projects without `surfaces.yml` are unaffected. See
[docs/surfaces.md](docs/surfaces.md).

## Workflow

1. Write the task brief in `tasks/` when the work needs context.
2. Implement changes in the relevant project files.
3. Record durable decisions in `docs/decisions.md`.
4. Run `scripts/check` before finishing.

## Automation Entrypoints

- `scripts/bootstrap`: install or prepare dependencies when a known stack is present.
- `scripts/check`: run formatting, linting, type checks, and tests when available.
- `scripts/test`: run the test suite when available.
- `scripts/eval`: run `doctor`, `bootstrap`, and `check` as a handoff gate.
- `scripts/doctor`: print repository and tooling readiness.
- `scripts/hooks`: install optional local hooks through `pre-commit`.

If you use `just`, the same commands are exposed in [`justfile`](justfile):

```sh
just check
just eval
```

These scripts are safe defaults for an empty or early-stage repository. Extend
them as the project grows.

## Compatible Agents

- OpenAI Codex reads `AGENTS.md` for repository instructions.
- Cursor can use `AGENTS.md` as shared project guidance.
- Claude Code reads `CLAUDE.md`, which imports the same shared instructions.

Keep shared rules in `AGENTS.md` so agent behavior stays consistent across
tools. Add tool-specific notes only when a tool requires a different bridge.

## Harness Metadata

[`harness.yml`](harness.yml) records the canonical command names, expected
documentation files, and task-loop stages. Keep it aligned with `AGENTS.md` and
the scripts in this repository.

## Optional Environment

The repository includes a minimal [dev container](.devcontainer/devcontainer.json)
that runs `scripts/bootstrap` after creation. It is optional; local development
works without Docker.
