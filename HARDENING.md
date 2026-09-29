<!-- markdownlint-disable -->

# Hardening Report: cyberark--conjur-action/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **cyberark--conjur-action/v3.0.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml Docker image reference uses a mutable version tag (`3.0.1`) instead of an immutable SHA digest. This means the image could be silently replaced with a different (potentially malicious) version without any change to the action.yml file, creating a supply-chain attack vector. The reference `docker://cyberark/conjur-action:3.0.1` should be replaced with a SHA-pinned form such as `docker://cyberark/conjur-action@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image reference in action.yml from `docker://cyberark/conjur-action:3.0.1` to `docker://cyberark/conjur-action:3.0.1@sha256:30e452c23049bd1a7ef12a820a863a93fcbc6bb21dbce421fb6cacaf1d79a88c`. The `docker://` scheme and `:3.0.1` tag are preserved for readability while the SHA256 digest ensures the reference is immutable.

