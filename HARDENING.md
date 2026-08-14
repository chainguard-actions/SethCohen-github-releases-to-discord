<!-- markdownlint-disable -->

# Hardening Report: SethCohen--github-releases-to-discord/v1.19.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **SethCohen--github-releases-to-discord/v1.19.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files reference external actions using mutable version tags instead of pinned full-length SHA commit hashes. This exposes the workflow to supply-chain attacks where a tag could be silently moved to point to malicious code.

- `.github/workflows/release-please.yml`: `uses: google-github-actions/release-please-action@v3`
- `.github/workflows/test.yml`: `uses: actions/checkout@v3`
- `.github/workflows/update-semver-tags.yml`: `uses: actions/checkout@v4` and `uses: haya14busa/action-update-semver@v1`

All should be replaced with the full 40-character commit SHA, e.g. `uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/release-please.yml:9`
- `.github/workflows/test.yml:8`
- `.github/workflows/update-semver-tags.yml:11`
- `.github/workflows/update-semver-tags.yml:13`

### missing-permissions (severity: medium)

None of the three workflow files define a top-level `permissions:` block, and none of the individual jobs define job-level `permissions:` blocks. Without explicit permissions, workflows run with the default repository permissions (which may include broad write access), violating the principle of least privilege.

Affected files:
- `.github/workflows/release-please.yml`
- `.github/workflows/test.yml`
- `.github/workflows/update-semver-tags.yml`

Each workflow should declare the minimal permissions required (e.g. `permissions: contents: read`).

Locations:

- `.github/workflows/release-please.yml:1`
- `.github/workflows/test.yml:1`
- `.github/workflows/update-semver-tags.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three workflow files:

1. `.github/workflows/release-please.yml`: Pinned `google-github-actions/release-please-action@v3` to SHA `db8f2c60ee802b3748b512940dde88eabd7b7e01`. Added `permissions: contents: write, pull-requests: write` (required for release-please to create PRs and releases).

2. `.github/workflows/test.yml`: Pinned `actions/checkout@v3` to SHA `a37ce9120846195fa4ece8f58b268e6043cb2f26`. Added `permissions: contents: read`.

3. `.github/workflows/update-semver-tags.yml`: Pinned `actions/checkout@v4` to SHA `11d5960a326750d5838078e36cf38b85af677262` and `haya14busa/action-update-semver@v1` to SHA `7d2c558640ea49e798d46539536190aff8c18715`. Added `permissions: contents: write` (required to push semver tags).

