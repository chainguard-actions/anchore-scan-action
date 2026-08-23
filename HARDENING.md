<!-- markdownlint-disable -->

# Hardening Report: anchore--scan-action/v7.2.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **anchore--scan-action/v7.2.3** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow uses a reusable workflow referenced by branch name (@main) instead of a full 40-character commit SHA. This means the workflow can be silently updated by whoever controls the referenced repository, enabling a supply-chain attack. Failing reference: `anchore/workflows/.github/workflows/oss-project-board-add.yaml@main`

Locations:

- `.github/workflows/oss-project-board-add.yaml:8`

### script-injection (severity: high)

Sub-rule (a): GitHub Actions expressions are interpolated directly inside `run:` shell command strings. (1) `${{ steps.grype.outputs.cmd }}` is used as the command to execute in two `run:` steps — a compromised or attacker-influenced step output could inject arbitrary shell commands. (2) `${{ steps.scan.outputs[matrix.output-format] }}` is interpolated directly into a `run:` shell command (`test -f '${{ steps.scan.outputs[matrix.output-format] }}'`), allowing a crafted output value to break out of the single-quoted string and inject shell commands. All three occurrences must be moved to `env:` variables and the env vars must be double-quoted in the shell script.

Locations:

- `.github/workflows/test.yml:43`
- `.github/workflows/test.yml:50`
- `.github/workflows/test.yml:68`

### missing-permissions (severity: medium)

None of the workflow files define a top-level `permissions:` block, and no individual job defines its own `permissions:` block. Without explicit permissions, GitHub Actions defaults to the repository's default token permissions (often broad write access), violating the principle of least privilege. All five workflow files are affected.

Locations:

- `.github/workflows/test.yml:1`
- `.github/workflows/update-grype-release.yml:1`
- `.github/workflows/tag-release.yml:1`
- `.github/workflows/release-drafter.yml:1`
- `.github/workflows/oss-project-board-add.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings across five workflow files:

1. unpinned-uses (oss-project-board-add.yaml line 8): Pinned `anchore/workflows/.github/workflows/oss-project-board-add.yaml@main` to full SHA `@48af2ac2ae466503efe50b585f3660dba2381bdd # main`.

2. script-injection (test.yml lines 43, 50, 68): Moved all three `${{ }}` expressions out of `run:` shell strings into `env:` blocks. `steps.grype.outputs.cmd` → env var `GRYPE_CMD` (used in two steps), and `steps.scan.outputs[matrix.output-format]` → env var `SCAN_OUTPUT` (used in test -f, changed from unsafe single-quoted to double-quoted).

3. missing-permissions: Added `permissions:` blocks to all five workflow files — `permissions: {}` for test.yml, oss-project-board-add.yaml, and update-grype-release.yml (which uses an app token for write operations); `permissions: contents: write` for tag-release.yml and release-drafter.yml (which need to create/update tags and draft releases respectively).

