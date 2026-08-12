# Changelog

All notable changes to this project are recorded here.

## Unreleased

- Add deploy-surface targeting: `surfaces.yml`, `scripts/surface`, an optional
  pre-commit containment guard, and a `Surface` field in the task template, so
  agents cannot silently ship a change to the wrong target.
- Add `scripts/selftest` and run it in CI.

- Clarify the README introduction, use cases, compatible agents, and provided automation entrypoints.
- Add contribution guidance for future harness changes.
- Add issue and pull request templates for public collaboration.

## 0.1.0

- Establish a language-agnostic Codex harness.
- Add `AGENTS.md`, `CLAUDE.md`, standard automation scripts, task template, workflow docs, and GitHub Actions verification.
