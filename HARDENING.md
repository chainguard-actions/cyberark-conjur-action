<!-- markdownlint-disable -->

# Hardening Report: cyberark--conjur-action/v2.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **cyberark--conjur-action/v2.2.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable tag (`docker://cyberark/conjur-action:2.2.2`) instead of an immutable SHA digest. This means the image could be replaced with a different (potentially malicious) version without changing the tag, creating a supply-chain risk. It should be pinned to a specific SHA digest, e.g. `docker://cyberark/conjur-action@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:34`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced the mutable Docker image tag `docker://cyberark/conjur-action:2.2.2` with the immutable SHA256 digest `docker://cyberark/conjur-action@sha256:370bd8ac9ad670309dc6c12e157ffc9df992eb4077197cf9127a54230e630e2c` in action.yml line 34. The original tag is preserved as a comment outside the YAML quotes for readability.

