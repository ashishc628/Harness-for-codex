# Decisions

Record durable project decisions here.

## 2026-05-18: Establish Codex Harness

- Created standard `scripts/bootstrap`, `scripts/check`, and `scripts/test` entrypoints.
- Added `AGENTS.md` so Codex has repository-local operating instructions.
- Kept the harness language-agnostic until a concrete project stack is introduced.

## 2026-05-18: Add Explicit Evaluation Loop

- Added `scripts/eval` to make full handoff verification a single command.
- Added `scripts/doctor` for environment and tool discovery.
- Added `harness.yml` so command names and documentation expectations are machine-readable.
- Reordered `AGENTS.md` to lead with commands and a task loop.

## 2026-05-18: Add Cross-Agent and Environment Bridges

- Added `CLAUDE.md` as a thin import bridge to avoid duplicating `AGENTS.md`.
- Added optional pre-commit hook installation through `scripts/hooks`.
- Added a minimal devcontainer that runs `scripts/bootstrap` after creation.
- Tightened GitHub Actions with read-only permissions, timeout, concurrency, and `scripts/eval`.
- Added a `justfile` as an optional task-runner facade over the canonical scripts.

## 2026-05-18: Improve Public Project Readiness

- Reworked the README opening sections to make the harness purpose, compatible agents, and standard workflow clearer.
- Added `CONTRIBUTING.md`, `CHANGELOG.md`, issue templates, and a pull request template for public collaboration.
- Left license selection as an owner decision because it affects legal reuse terms.

## 2026-08-12: Measure the Instructions, Not Just the Repository

- Added `scripts/agent-eval` with task cases under `evals/`. `scripts/eval`
  verifies this repository; it never verified the `AGENTS.md` this repository
  exists to ship. Those are different layers that had been sharing a name.
- Kept graders binary and deterministic. A 1-5 quality score is not actionable
  and two reviewers will not agree on it; where a scale seems necessary, the
  check usually wants splitting into several binary ones.
- Graded behavior rather than output. The cases that matter check whether the
  agent ran verification, left unrelated files alone, and asked instead of
  guessing - none of which are visible in a correct-looking diff.
- Treated a correct guess on an ambiguous task as a failure. There is no right
  guess; an agent that guesses right has still learned to guess.
- Reported pass@k and pass^k rather than one rate, because "it worked once" and
  "it works every time" are different claims and instructions need the second.
- Added a scripted agent with `good` and `sloppy` variants so the suite runs in
  CI with no model or network, and so the graders are themselves tested: the
  sloppy variant writes correct code and must still fail.
