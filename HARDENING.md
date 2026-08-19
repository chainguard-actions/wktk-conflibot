<!-- markdownlint-disable -->

# Hardening Report: wktk--conflibot/v1.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wktk--conflibot/v1.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable version tags instead of full 40-character commit SHA digests. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the action is compromised. Failing references: `actions/checkout@v3` (test.yml and conflibot.yml) and `wktk/conflibot@v1` (conflibot.yml). Each should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/conflibot.yml:8`
- `.github/workflows/conflibot.yml:10`
- `.github/workflows/test.yml:9`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and no individual job within them declares job-level permissions. Without explicit permissions, the GITHUB_TOKEN is granted its default (broad) permissions, violating the principle of least privilege. A `permissions:` block with only the required scopes (e.g. `pull-requests: write` for conflibot.yml) should be added.

Locations:

- `.github/workflows/conflibot.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned actions/checkout@v3 to full SHA f43a0e5ff2bd294095638e18286ca9a3d1956744 in both conflibot.yml and test.yml; pinned wktk/conflibot@v1 to full SHA 59e255c49c920fd6b67471ab9ef3bf194439a3f2 in conflibot.yml. (2) Added top-level permissions blocks: conflibot.yml gets `pull-requests: write` (required for the conflibot action to post PR comments), test.yml gets `contents: read` (minimum needed for checkout and running tests).

