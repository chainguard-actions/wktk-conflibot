<!-- markdownlint-disable -->

# Hardening Report: wktk--conflibot/v1.1.2-pre2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wktk--conflibot/v1.1.2-pre2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable version tags instead of full 40-character commit SHA digests, making them vulnerable to supply-chain attacks if the tag is moved. In conflibot.yml: `actions/checkout@v3` (line 8) and `wktk/conflibot@v1` (line 10). In test.yml: `actions/checkout@v3` (line 13). All should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/conflibot.yml:8`
- `.github/workflows/conflibot.yml:10`
- `.github/workflows/test.yml:13`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and no job within them declares job-level permissions. Without explicit permissions, the GITHUB_TOKEN is granted its default (broad) permissions. This is especially risky in conflibot.yml which is triggered by `pull_request_target` — a privileged trigger that runs in the context of the base branch with write access to the repository. Explicit minimal permissions (e.g. `permissions: pull-requests: write`) should be declared.

Locations:

- `.github/workflows/conflibot.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all action references to full 40-char SHAs — actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26 and wktk/conflibot@v1 → @59e255c49c920fd6b67471ab9ef3bf194439a3f2, with original tags preserved as inline comments. (2) Added top-level permissions blocks — conflibot.yml gets 'contents: read' and 'pull-requests: write' (needed for the conflibot action to comment on PRs), while test.yml gets 'contents: read' only (sufficient for checkout and running tests).

