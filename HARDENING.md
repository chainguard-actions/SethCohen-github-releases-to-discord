<!-- markdownlint-disable -->

# Hardening Report: SethCohen--github-releases-to-discord/v1.20.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **SethCohen--github-releases-to-discord/v1.20.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference GitHub Actions using mutable version tags instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the upstream repository is compromised.

Failing references:
- `.github/workflows/release-please.yml`: `google-github-actions/release-please-action@v3`
- `.github/workflows/test.yml`: `actions/checkout@v3`
- `.github/workflows/update-semver-tags.yml`: `actions/checkout@v4`, `haya14busa/action-update-semver@v1`

All should be pinned to full 40-character commit SHAs, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/release-please.yml:9`
- `.github/workflows/test.yml:9`
- `.github/workflows/test.yml:11`
- `.github/workflows/update-semver-tags.yml:12`
- `.github/workflows/update-semver-tags.yml:14`

### missing-permissions (severity: medium)

None of the workflow files declare a top-level `permissions:` block, and none of the individual jobs declare job-level `permissions:` blocks. Without explicit permissions, workflows run with the repository's default token permissions, which may be overly broad (e.g. `write` access to contents and pull-requests). Each workflow should declare the minimal permissions required.

Affected files:
- `.github/workflows/release-please.yml` — no permissions declared
- `.github/workflows/test.yml` — no permissions declared
- `.github/workflows/update-semver-tags.yml` — no permissions declared

Locations:

- `.github/workflows/release-please.yml:1`
- `.github/workflows/test.yml:1`
- `.github/workflows/update-semver-tags.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 workflow files:

1. `.github/workflows/release-please.yml`: Pinned `google-github-actions/release-please-action@v3` to `@db8f2c60ee802b3748b512940dde88eabd7b7e01 # v3`. Added `permissions: contents: write, pull-requests: write` (needed for release-please to create releases and PRs).

2. `.github/workflows/test.yml`: Pinned `actions/checkout@v3` to `@a37ce9120846195fa4ece8f58b268e6043cb2f26 # v3`. Added `permissions: contents: read` (minimal read-only access for checkout).

3. `.github/workflows/update-semver-tags.yml`: Pinned `actions/checkout@v4` to `@11d5960a326750d5838078e36cf38b85af677262 # v4` and `haya14busa/action-update-semver@v1` to `@7d2c558640ea49e798d46539536190aff8c18715 # v1`. Added `permissions: contents: write` (needed to push updated semver tags).

