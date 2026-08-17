<!-- markdownlint-disable -->

# Hardening Report: cyberark--conjur-action/v2.0.12

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **cyberark--conjur-action/v2.0.12** was hardened automatically. 1 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### github-env-injection (severity: high)

In entrypoint.sh, the set_secrets() function writes unsanitized values to $GITHUB_ENV without the required newline-stripping sanitization step. The line `echo "${envVar}=${secretVal}" >> "${GITHUB_ENV}"` writes two values that can contain newlines: (1) `envVar` is derived from the user-controlled `$INPUT_SECRETS` input (the 'secrets' action input, split on '|' and uppercased), and (2) `secretVal` is the raw response from the Conjur API, which could contain embedded newlines. An attacker who controls the 'secrets' input or who can influence the Conjur secret value could inject additional KEY=VALUE lines into $GITHUB_ENV, causing arbitrary environment variables to be set for subsequent workflow steps. The required sanitization pattern `safe=$(printf '%s' "$VALUE" | tr -d '\n\r')` is absent before the write.

Locations:

- `entrypoint.sh:155`

## Iteration Notes

### Iteration 1

**Fixes applied:** github-env-injection

**Notes:**

Fixed the github-env-injection vulnerability in hardened/action/entrypoint.sh. In the set_secrets() function, before writing to $GITHUB_ENV, both `envVar` (derived from user-controlled $INPUT_SECRETS) and `secretVal` (raw Conjur API response) are now sanitized using `printf '%s' "$VALUE" | tr -d '\n\r'` to strip embedded newlines and carriage returns. This prevents injection of additional KEY=VALUE lines into $GITHUB_ENV for subsequent workflow steps.

### Iteration 2

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

1. Pinned actions/checkout@v1 to full SHA 50fbc622fc4ef5163becd7fab6573eac35f8462e (both occurrences). 2. Pinned AutoModality/action-clean@v1.1.0 to full SHA d004b47eb728f2a83316fc70ba9a62ef97278013 (both occurrences). 3. Added top-level 'permissions: contents: read' block to restrict default GITHUB_TOKEN permissions. 4. Added explicit 'permissions: contents: read' to the github-secrets job which previously had no permissions block.

