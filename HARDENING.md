<!-- markdownlint-disable -->

# Hardening Report: cyberark--conjur-action/v2.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **cyberark--conjur-action/v2.1.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image referenced by a mutable tag (`docker://cyberark/conjur-action:2.1.1`) instead of an immutable SHA digest. This means the image could be silently replaced with a malicious version without any change to the action definition, creating a supply-chain attack risk. It should be pinned to a SHA digest, e.g. `docker://cyberark/conjur-action@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:31`

### github-env-injection (severity: high)

In `entrypoint.sh`, the `set_secrets()` function writes `echo "${envVar}=${secretVal}" >> "${GITHUB_ENV}"` without first sanitizing either value with `printf '%s' ... | tr -d '\n\r'`. The `envVar` variable is derived directly from `$INPUT_SECRETS` (the caller-controlled `inputs.secrets` input), and `secretVal` is fetched from a remote Conjur server. A newline character embedded in either `envVar` or `secretVal` would allow injection of additional key=value pairs into the runner's environment, potentially overwriting sensitive environment variables such as `ACTIONS_ID_TOKEN_REQUEST_TOKEN` or other secrets used by subsequent steps.

Locations:

- `entrypoint.sh:160`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, github-env-injection

**Notes:**

1. action.yml line 31: Pinned Docker image from mutable tag `docker://cyberark/conjur-action:2.1.1` to immutable digest `docker://cyberark/conjur-action@sha256:d024479b413614235e94a3f2d347a8c2fe10e7c3612e09a9576b0a7455d79c17` with the original tag preserved as a comment. 2. entrypoint.sh line 160: Fixed GITHUB_ENV injection in `set_secrets()` by sanitizing both `envVar` (caller-controlled) and `secretVal` (from Conjur server) with `printf '%s' ... | tr -d '\n\r'` before writing to `$GITHUB_ENV`, preventing newline-based injection of additional environment variables.

