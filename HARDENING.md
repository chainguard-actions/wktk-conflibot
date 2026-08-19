<!-- markdownlint-disable -->

# Hardening Report: wktk--conflibot/v1.2.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wktk--conflibot/v1.2.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files use mutable tag-based `uses:` references instead of pinned 40-character SHA commit hashes. This exposes the workflows to supply-chain attacks if the referenced tags are moved to point to malicious commits.

Offending references:
- `.github/workflows/conflibot.yml` line 8: `uses: actions/checkout@v3`
- `.github/workflows/conflibot.yml` line 10: `uses: wktk/conflibot@v1`
- `.github/workflows/test.yml` line 13: `uses: actions/checkout@v3`

All should be replaced with full 40-character SHA digests, e.g. `uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/conflibot.yml:8`
- `.github/workflows/conflibot.yml:10`
- `.github/workflows/test.yml:13`

### missing-permissions (severity: medium)

Neither `.github/workflows/conflibot.yml` nor `.github/workflows/test.yml` declares a top-level `permissions:` key, and no job in either file has a job-level `permissions:` block. Without explicit permissions, workflows run with the repository's default token permissions, which may be broader than necessary. This is especially concerning for `conflibot.yml`, which is triggered by `pull_request_target` (a privileged trigger that runs in the context of the base branch with write access). Minimal permissions (e.g. `permissions: pull-requests: write` for conflibot, `permissions: contents: read` for test) should be declared explicitly.

Locations:

- `.github/workflows/conflibot.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three unpinned action references by resolving their full 40-character SHA digests: actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26 and wktk/conflibot@v1 → @59e255c49c920fd6b67471ab9ef3bf194439a3f2. Added top-level permissions blocks to both workflow files: conflibot.yml gets `contents: read` and `pull-requests: write` (required for the conflibot action to post PR comments on the privileged pull_request_target trigger), and test.yml gets `contents: read` (minimal permission for checkout and test execution).

