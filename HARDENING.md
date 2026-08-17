<!-- markdownlint-disable -->

# Hardening Report: cyberark--conjur-action/v2.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **cyberark--conjur-action/v2.2.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable version tag instead of a SHA digest. The image 'docker://cyberark/conjur-action:2.2.2' can be silently replaced by a different image if the tag is overwritten in the registry, enabling a supply-chain attack. It should be pinned to a specific SHA256 digest, e.g. 'docker://cyberark/conjur-action@sha256:<64-hex-char-digest> # 2.2.2'.

Locations:

- `action.yml:32`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker container image reference in hardened/action/action.yml from 'docker://cyberark/conjur-action:2.2.2' to 'docker://cyberark/conjur-action:2.2.2@sha256:370bd8ac9ad670309dc6c12e157ffc9df992eb4077197cf9127a54230e630e2c'. The docker:// scheme and :2.2.2 tag are preserved inline alongside the digest, preventing supply-chain attacks via mutable tag overwrites.

