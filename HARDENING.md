<!-- markdownlint-disable -->

# Hardening Report: cyberark--conjur-action/v2.0.11

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **cyberark--conjur-action/v2.0.11** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### github-env-injection (severity: high)

In entrypoint.sh, the `set_secrets` function writes `envVar` and `secretVal` to `$GITHUB_ENV` without sanitization. `envVar` is derived from `INPUT_SECRETS` (a workflow-controlled environment variable containing the secret path/alias mapping), and `secretVal` is the raw value fetched from Conjur. Either value could contain embedded newlines (`\n`) that would allow injection of additional key=value pairs into the GitHub environment. The required sanitization step (`printf '%s' "$VAR" | tr -d '\n\r'`) is absent before the write: `echo "${envVar}=${secretVal}" >> "${GITHUB_ENV}"`.

Locations:

- `entrypoint.sh:155`

### unpinned-uses (severity: high)

The workflow file `.github/workflows/main.yml` references actions using mutable tags instead of pinned 40-character SHA digests, making the workflow vulnerable to supply-chain attacks if those tags are moved or compromised. Unpinned references found: `actions/checkout@v1` (used twice, in both jobs) and `AutoModality/action-clean@v1.1.0` (used twice, in both jobs).

Locations:

- `.github/workflows/main.yml:18`
- `.github/workflows/main.yml:36`
- `.github/workflows/main.yml:42`
- `.github/workflows/main.yml:57`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/main.yml` has no top-level `permissions:` block, and the `github-secrets` job has no job-level `permissions:` block. Only the `authn-jwt` job defines permissions. Without explicit permissions on the `github-secrets` job, it inherits the default (broad) repository permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/main.yml:40`

## Iteration Notes

### Iteration 1

**Fixes applied:** github-env-injection, unpinned-uses, missing-permissions

**Notes:**

1. entrypoint.sh: Added newline sanitization before writing to GITHUB_ENV. Both `envVar` (from INPUT_SECRETS) and `secretVal` (from Conjur) are now sanitized with `printf '%s' "${VAR}" | tr -d '\n\r'` before the echo write, preventing injection of additional key=value pairs. 2. .github/workflows/main.yml: Pinned actions/checkout@v1 to SHA 50fbc622fc4ef5163becd7fab6573eac35f8462e and AutoModality/action-clean@v1.1.0 to SHA d004b47eb728f2a83316fc70ba9a62ef97278013 (both occurrences of each). 3. .github/workflows/main.yml: Added `permissions: contents: 'read'` to the github-secrets job to enforce least-privilege (the job only needs to check out code, not write id-tokens).

