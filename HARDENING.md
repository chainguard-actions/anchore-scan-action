<!-- markdownlint-disable -->

# Hardening Report: anchore--scan-action/v7.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **anchore--scan-action/v7.4.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file uses a mutable branch ref (@main) instead of a pinned full-length SHA commit hash. The reference `uses: "anchore/workflows/.github/workflows/oss-project-board-add.yaml@main"` can silently pull in changed or malicious code if the upstream repository is compromised or the branch is force-pushed. It should be pinned to a full 40-character hex SHA (e.g., `@<sha> # main`).

Locations:

- `.github/workflows/oss-project-board-add.yaml:15`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned `anchore/workflows/.github/workflows/oss-project-board-add.yaml@main` to the full commit SHA `48af2ac2ae466503efe50b585f3660dba2381bdd` with a `# main` comment for readability. The workflow already had `permissions: {}` set, so no other changes were needed.

