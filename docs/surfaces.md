# Deploy Surfaces

A **surface** is one deployable target of a repository: an iOS build, an
Android build, a marketing website, an admin console. Most projects that adopt
this harness have several, and they are deployed by different commands that
look alike.

The harness already learned that agent instructions must live in a file rather
than in chat context. The deploy target needs the same treatment. When the
target only exists as a sentence in the opening message, it is the first thing
lost to context compaction — and the resulting failure is quiet: the agent
edits a plausible file, runs a deploy command that succeeds, and reports
success on the wrong target.

Surface targeting makes the target a fact in the repository.

## Enabling It

```sh
cp surfaces.example.yml surfaces.yml
```

Then edit it. Without `surfaces.yml`, every surface command is a documented
no-op and the rest of the harness behaves exactly as before.

## Configuration

```yaml
version: 1

shared:
  - "docs/"

surfaces:
  - id: website
    name: Marketing website
    paths:
      - "web/"
    deny:
      - "admin/"
    verify: "npm --prefix web run build"
    deploy: "npm --prefix web run deploy"
```

- `paths`: globs the surface owns. `**` matches across directories; a trailing
  `/` means everything beneath that directory.
- `deny`: paths this surface must never touch, even if a `paths` glob would
  allow them. Use it for the neighbours that are easy to confuse — the admin
  console next to the website, the web build next to the app build.
- `shared`: paths any surface may change, such as docs and harness files.
- `verify` / `deploy`: the commands for this surface. `deploy` is the *only*
  deploy command an agent may run for a task targeting this surface.

## Commands

```sh
scripts/surface list              # declared surfaces
scripts/surface show website      # one surface in detail
scripts/surface detect            # which surface owns each changed file
scripts/surface check website     # fail if changes escape the surface
scripts/surface plan website      # the verify and deploy commands to use
```

`detect` and `check` read uncommitted changes by default, including untracked
files. Pass `--base <ref>` to inspect a branch or commit range instead:

```sh
scripts/surface check website --base origin/main
```

## Where It Runs

- `SURFACE=website scripts/check` runs the containment check as part of normal
  verification. Plain `scripts/check` is unchanged.
- The optional `harness-surface` pre-commit hook runs the same check before a
  commit, and skips itself when `SURFACE` is unset.
- `scripts/doctor` reports the declared surfaces and the current `SURFACE`.

## Failure Output

```
Surface check failed for 'website'.
  admin/panel.tsx (denied by surface 'website')

These changes do not belong to the declared surface. Either correct the
target in the task brief or move the change to its own task.
Run 'scripts/surface detect' to see which surface owns each file.
```

The check names files, not intentions. If the diff is right and the declared
surface is wrong, fix the task brief; if the brief is right, the diff is the
bug.

## Limits

- Containment is path-based. Two surfaces that legitimately share a directory —
  an iOS and an Android build over the same `app/` tree — cannot be separated
  by paths alone. Use `deny` for the directories they must never cross into,
  and rely on `plan` to keep the deploy commands distinct.
- The check verifies *what changed*, not *what was deployed*. It narrows the
  wrong-target failure to the point where it is cheap to catch: before the
  commit, and before the deploy command is chosen.
