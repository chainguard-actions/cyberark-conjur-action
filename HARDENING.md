<!-- markdownlint-disable -->

# Hardening Report: cyberark--conjur-action/v2.0.11

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **cyberark--conjur-action/v2.0.11** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### github-env-injection (severity: high)

In entrypoint.sh, the set_secrets() function writes secret values and environment variable names to $GITHUB_ENV without sanitization. Specifically, `echo "${envVar}=${secretVal}" >> "${GITHUB_ENV}"` is executed where:
- `secretVal` is the raw response from a curl call to the Conjur secrets server — an externally-controlled value that could contain newlines
- `envVar` is derived from `INPUT_SECRETS` (the `inputs.secrets` action input, which is user/caller-controlled), split on '|' and ';' delimiters

A secret value containing newline characters (e.g. `\nFOO=injected`) would inject additional key=value pairs into $GITHUB_ENV, allowing environment variable injection into subsequent workflow steps. The required sanitization step (`printf '%s' "$VAR" | tr -d '\n\r'`) is absent before the write.

Locations:

- `entrypoint.sh:120`

## Iteration Notes

### Iteration 1

**Fixes applied:** github-env-injection

**Notes:**

Fixed the github-env-injection vulnerability in entrypoint.sh set_secrets() function. Before writing to $GITHUB_ENV, both `envVar` (user-controlled from INPUT_SECRETS) and `secretVal` (externally-controlled from curl response to Conjur server) are now sanitized using `printf '%s' "$VAR" | tr -d '\n\r'` to strip newline and carriage return characters. This prevents a malicious secret value containing newlines (e.g. `\nFOO=injected`) from injecting additional key=value pairs into $GITHUB_ENV and affecting subsequent workflow steps.

