<!-- markdownlint-disable -->

# Hardening Report: cyberark--conjur-action/v2.0.12

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **cyberark--conjur-action/v2.0.12** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### github-env-injection (severity: high)

In entrypoint.sh, the set_secrets() function writes secret values and environment variable names directly to $GITHUB_ENV without sanitization. The line `echo "${envVar}=${secretVal}" >> "${GITHUB_ENV}"` writes both `envVar` (derived from the caller-controlled `$INPUT_SECRETS` env var, which is split on '|' and '^^ uppercased) and `secretVal` (fetched from a remote Conjur server via curl) without applying the required `printf '%s' ... | tr -d '\n\r'` sanitization step. A value containing embedded newlines (e.g. a crafted Conjur secret response or a malicious `secrets` input) could inject arbitrary additional KEY=VALUE pairs into the runner's environment, potentially overwriting sensitive variables like ACTIONS_ID_TOKEN_REQUEST_TOKEN or other secrets used by subsequent steps.

Locations:

- `entrypoint.sh:170`

## Iteration Notes

### Iteration 1

**Fixes applied:** github-env-injection

**Notes:**

Fixed the GITHUB_ENV injection vulnerability in entrypoint.sh's set_secrets() function. Added sanitization of both `envVar` (derived from caller-controlled $INPUT_SECRETS) and `secretVal` (fetched from Conjur server) before writing to $GITHUB_ENV. The fix uses `printf '%s' "${value}" | tr -d '\n\r'` to strip embedded newlines and carriage returns, preventing injection of arbitrary KEY=VALUE pairs into the runner's environment. The sanitized values are stored in `safe_envVar` and `safe_secretVal` variables which are then used in the echo redirect to $GITHUB_ENV.

