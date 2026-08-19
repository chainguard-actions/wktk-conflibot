<!-- markdownlint-disable -->

# Hardening Report: wktk--conflibot/v1.1.2-pre1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wktk--conflibot/v1.1.2-pre1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference actions by mutable tag instead of a full 40-character commit SHA, making the workflow vulnerable to supply-chain attacks if the tag is moved.

Failing references:
- `.github/workflows/conflibot.yml`: `uses: actions/checkout@v3` and `uses: wktk/conflibot@v1`
- `.github/workflows/test.yml`: `uses: actions/checkout@v3`

All should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/conflibot.yml:8`
- `.github/workflows/conflibot.yml:10`
- `.github/workflows/test.yml:13`

### missing-permissions (severity: medium)

Neither `.github/workflows/conflibot.yml` nor `.github/workflows/test.yml` declares a top-level `permissions:` block, and no job in either file has a job-level `permissions:` block. Without explicit permissions, the GITHUB_TOKEN is granted its default (often broad) permissions. Each workflow should declare the minimal required permissions (e.g. `permissions: contents: read`).

Locations:

- `.github/workflows/conflibot.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:
1. conflibot.yml: Pinned actions/checkout@v3 to SHA a37ce9120846195fa4ece8f58b268e6043cb2f26 and wktk/conflibot@v1 to SHA 59e255c49c920fd6b67471ab9ef3bf194439a3f2. Added top-level permissions block with 'contents: read' and 'pull-requests: write' (the conflibot action needs to write PR comments/warnings).
2. test.yml: Pinned actions/checkout@v3 to SHA a37ce9120846195fa4ece8f58b268e6043cb2f26. Added top-level permissions block with 'contents: read' only (the test workflow only reads the repository).

