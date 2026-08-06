<!-- markdownlint-disable -->

# Hardening Report: SethCohen--github-releases-to-discord/v1.18.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **SethCohen--github-releases-to-discord/v1.18.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of full 40-character SHA commit digests. This exposes the workflow to supply-chain attacks where a tag is silently moved to point to malicious code. Affected references: `google-github-actions/release-please-action@v3` (release-please.yml:10), `actions/checkout@v3` (test.yml:8), `actions/checkout@v4` (update-semver-tags.yml:13), `haya14busa/action-update-semver@v1` (update-semver-tags.yml:15). Each should be replaced with a pinned SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/release-please.yml:10`
- `.github/workflows/test.yml:8`
- `.github/workflows/update-semver-tags.yml:13`
- `.github/workflows/update-semver-tags.yml:15`

### missing-permissions (severity: medium)

None of the three workflow files define a `permissions:` block at the top level or at the job level. Without explicit permissions, workflows run with the repository's default token permissions (often `write` for many scopes), violating the principle of least privilege. Each workflow should declare the minimal permissions required, e.g. `permissions: contents: read`.

Locations:

- `.github/workflows/release-please.yml:1`
- `.github/workflows/test.yml:1`
- `.github/workflows/update-semver-tags.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 4 unpinned action references by replacing mutable tags with full SHA digests: google-github-actions/release-please-action@v3 → @db8f2c60ee802b3748b512940dde88eabd7b7e01, actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26, actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262, haya14busa/action-update-semver@v1 → @7d2c558640ea49e798d46539536190aff8c18715. Added permissions blocks to all 3 workflow files: release-please.yml gets contents:write + pull-requests:write (needed to create releases and PRs), test.yml gets contents:read (read-only checkout), update-semver-tags.yml gets contents:write (needed to update semver tags).

