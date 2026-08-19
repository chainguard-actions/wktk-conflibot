<!-- markdownlint-disable -->

# Hardening Report: wktk--conflibot/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wktk--conflibot/v2.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags (@v6) instead of immutable 40-character commit SHA digests. This exposes the workflow to supply-chain attacks where a tag could be silently moved to point at malicious code. Affected references: actions/checkout@v6, actions/setup-node@v6.

Locations:

- `.github/workflows/conflibot.yml:11`
- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:15`
- `.github/workflows/update-dist.yml:18`
- `.github/workflows/update-dist.yml:20`
- `.github/workflows/verify-release.yml:12`
- `.github/workflows/verify-release.yml:13`

### missing-permissions (severity: medium)

Workflow files test.yml and verify-release.yml have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/test.yml:1`
- `.github/workflows/verify-release.yml:5`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 unpinned action references across 4 workflow files by replacing mutable @v6 tags with full 40-character commit SHAs (actions/checkout@df4cb1c069e1874edd31b4311f1884172cec0e10, actions/setup-node@249970729cb0ef3589644e2896645e5dc5ba9c38). Added top-level `permissions: contents: read` blocks to test.yml and verify-release.yml, which only need read access to check out and build the repository. The conflibot.yml and update-dist.yml already had appropriate permissions blocks.

