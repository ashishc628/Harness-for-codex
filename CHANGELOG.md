# Changelog

All notable changes to this project are recorded here.

## Unreleased

- Add `scripts/agent-eval`, `evals/cases/`, and a scripted stand-in agent, so a
  change to `AGENTS.md` can be measured instead of guessed at. Reports pass@k,
  pass^k, and a backlog of failed checks by frequency.
- Add `scripts/selftest`, run in CI, which asserts the graders tell a careful
  agent from a sloppy one.

- Clarify the README introduction, use cases, compatible agents, and provided automation entrypoints.
- Add contribution guidance for future harness changes.
- Add issue and pull request templates for public collaboration.

## 0.1.0

- Establish a language-agnostic Codex harness.
- Add `AGENTS.md`, `CLAUDE.md`, standard automation scripts, task template, workflow docs, and GitHub Actions verification.
