<!-- markdownlint-disable -->

# Hardening Report: HaaLeo--publish-vscode-extension/v2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **HaaLeo--publish-vscode-extension/v2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in CI.yml use mutable version tags instead of pinned 40-character SHA commit digests, making the workflow vulnerable to supply-chain attacks if those tags are moved or compromised. Failing references: `actions/checkout@v4` (lines 30, 64), `actions/setup-node@v4` (line 32), `coverallsapp/github-action@v2` (lines 43, 50).

Locations:

- `.github/workflows/CI.yml:30`
- `.github/workflows/CI.yml:32`
- `.github/workflows/CI.yml:43`
- `.github/workflows/CI.yml:50`
- `.github/workflows/CI.yml:64`

### missing-permissions (severity: medium)

CI.yml has no top-level `permissions:` key and none of the three jobs (`build-lint-test`, `coverage`, `publish`) define job-level `permissions:` blocks. Without explicit permissions, the workflow inherits the default repository permissions, which may be overly broad (e.g., write access to contents). Minimal permissions such as `contents: read` should be declared explicitly.

Locations:

- `.github/workflows/CI.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Updated hardened/action/.github/workflows/CI.yml with the following changes:

1. **unpinned-uses**: Pinned all `uses:` references to full 40-character SHA digests:
   - `actions/checkout@v4` → `actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4` (lines 30, 64)
   - `actions/setup-node@v4` → `actions/setup-node@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4` (line 32)
   - `coverallsapp/github-action@v2` → `coverallsapp/github-action@5cbfd81b66ca5d10c19b062c04de0199c215fb6e # v2` (lines 43, 50)

2. **missing-permissions**: Added explicit permissions blocks:
   - Top-level: `permissions: contents: read` (safe default)
   - `build-lint-test` job: `permissions: contents: read`
   - `coverage` job: `permissions: contents: read`
   - `publish` job: `permissions: contents: write` (required to push tags with `git push --tags --force`)

