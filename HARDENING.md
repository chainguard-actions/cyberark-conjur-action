<!-- markdownlint-disable -->

# Hardening Report: cyberark--conjur-action/v2.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **cyberark--conjur-action/v2.1.0** was hardened automatically. 1 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image referenced by a mutable tag ('2.1.0') instead of an immutable SHA digest. This means the image could be replaced with a different (potentially malicious) version without changing the action reference. The failing reference is: `image: 'docker://cyberark/conjur-action:2.1.0'`. It should be pinned to a SHA digest, e.g. `image: 'docker://cyberark/conjur-action@sha256:<64-hex-char-digest>'`.

Locations:

- `action.yml:28`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image reference in action.yml from the mutable tag 'docker://cyberark/conjur-action:2.1.0' to the immutable digest 'docker://cyberark/conjur-action@sha256:59ddceb0a898a76b05b61c8394f1ceb524c7e7fb7f8f653ab2d73dcb11552523' # 2.1.0. The original tag is preserved as a comment outside the YAML quotes for readability.

### Iteration 2

**Fixes applied:** github-env-injection

**Notes:**

Fixed the GITHUB_ENV injection vulnerability in entrypoint.sh set_secrets() function. The original code wrote `echo "${envVar}=${secretVal}" >> "${GITHUB_ENV}"` without sanitizing either value. The fix introduces two sanitization steps before the write: `safe_envVar=$(printf '%s' "${envVar}" | tr -d '\n\r')` and `safe_secretVal=$(printf '%s' "${secretVal}" | tr -d '\n\r')`, then writes `echo "${safe_envVar}=${safe_secretVal}" >> "${GITHUB_ENV}"`. This prevents embedded newlines in either the secret variable name (from `$INPUT_SECRETS`) or the secret value (from the Conjur server response) from injecting additional environment variable assignments into the runner's GITHUB_ENV file.

