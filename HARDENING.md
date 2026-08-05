<!-- markdownlint-disable -->

# Hardening Report: SethCohen--github-releases-to-discord/v1.17.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **SethCohen--github-releases-to-discord/v1.17.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in the workflow files use mutable tag refs instead of pinned 40-character commit SHAs, making the workflows vulnerable to supply-chain attacks if the referenced action tags are moved or compromised. Failing references:
- `.github/workflows/release-please.yml`: `google-github-actions/release-please-action@v3`
- `.github/workflows/test.yml`: `actions/checkout@v3`
- `.github/workflows/update-semver-tags.yml`: `actions/checkout@v4`
- `.github/workflows/update-semver-tags.yml`: `haya14busa/action-update-semver@v1`

Locations:

- `.github/workflows/release-please.yml:9`
- `.github/workflows/test.yml:8`
- `.github/workflows/update-semver-tags.yml:12`
- `.github/workflows/update-semver-tags.yml:14`

### missing-permissions (severity: medium)

None of the workflow files define a top-level `permissions:` block, and no job within them defines job-level `permissions:` either. Without explicit permissions, workflows run with the default (potentially broad) token permissions. All three workflow files are affected: `release-please.yml`, `test.yml`, and `update-semver-tags.yml`.

Locations:

- `.github/workflows/release-please.yml:1`
- `.github/workflows/test.yml:1`
- `.github/workflows/update-semver-tags.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 4 unpinned action references by resolving their commit SHAs: google-github-actions/release-please-action@v3 → db8f2c60ee802b3748b512940dde88eabd7b7e01, actions/checkout@v3 → a37ce9120846195fa4ece8f58b268e6043cb2f26, actions/checkout@v4 → 11d5960a326750d5838078e36cf38b85af677262, haya14busa/action-update-semver@v1 → 7d2c558640ea49e798d46539536190aff8c18715. Added top-level `permissions: {}` to all three workflow files and job-level permissions with minimum required access: release-please.yml gets contents: write + pull-requests: write (needed to create releases and PRs), test.yml gets contents: read (needed to checkout code), update-semver-tags.yml gets contents: write (needed to push updated semver tags).

