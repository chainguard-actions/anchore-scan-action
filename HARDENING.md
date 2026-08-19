<!-- markdownlint-disable -->

# Hardening Report: anchore--scan-action/v7.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **anchore--scan-action/v7.3.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow uses a reusable workflow referenced by a mutable branch name (`@main`) instead of a pinned full-length SHA commit hash. This means the workflow could silently pull in changed (potentially malicious) code on future runs without any review. The offending reference is: `uses: "anchore/workflows/.github/workflows/oss-project-board-add.yaml@main"`. It should be pinned to a specific 40-character commit SHA, e.g. `uses: anchore/workflows/.github/workflows/oss-project-board-add.yaml@<full-sha> # @main`.

Locations:

- `.github/workflows/oss-project-board-add.yaml:16`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the reusable workflow reference in .github/workflows/oss-project-board-add.yaml from `anchore/workflows/.github/workflows/oss-project-board-add.yaml@main` to the full commit SHA `a71346be63db56f324237b6a35fd35ffcec6d737`, with `# @main` preserved as a comment for readability.

