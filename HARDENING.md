<!-- markdownlint-disable -->

# Hardening Report: cyberark--conjur-action/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **cyberark--conjur-action/v2.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable version tag ('docker://cyberark/conjur-action:2.2.0') instead of an immutable SHA digest. This means the image could be replaced with a different (potentially malicious) version without changing the tag, creating a supply-chain risk. The image reference should use a SHA digest, e.g. 'docker://cyberark/conjur-action@sha256:<64-hex-char-digest>'.

Locations:

- `action.yml:33`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced the mutable Docker image tag 'docker://cyberark/conjur-action:2.2.0' with the immutable SHA256 digest 'docker://cyberark/conjur-action@sha256:d831a825cc12459780d35af9a9129deaad01df6b89e1d049dd8a9402570a9734' in action.yml line 33. The original tag '2.2.0' is preserved as a comment outside the YAML quotes for readability.

