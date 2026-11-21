# R002 — Configuration resolution

## Purpose

Define how a tool combines configuration values from defaults, a file and environment
variables, so behaviour is deterministic and errors are actionable.

## Scope

- In scope: value precedence, required-key validation, deterministic errors.
- Out of scope: file formats beyond the chosen one, remote configuration sources.

## Requirements

### REQ-006 — Layered configuration

**Type:** ubiquitous

The loader shall resolve configuration from three layers: built-in defaults, a
configuration file, and environment variables.

### REQ-007 — Precedence

**Type:** event-driven

When the same key is present in more than one layer, the loader shall use the value from
the highest-precedence layer, with environment variables above the file and the file above
defaults.

### REQ-008 — Missing required key

**Type:** state-driven

While a required key is absent from all layers, the loader shall report a validation error
that lists every missing key.

### REQ-009 — Malformed configuration file

**Type:** unwanted

If the configuration file cannot be parsed, then the loader shall report a parse error and
exit with a non-zero code without applying partial configuration.

### REQ-010 — Successful resolution

**Type:** event-driven

When every required key resolves, the loader shall return the merged configuration to the
caller.

## Acceptance criteria

```gherkin
Scenario Outline: Precedence
  Given defaults contain "<key>" with value "<default>"
  And the configuration file contains "<key>" with value "<file>"
  And the environment contains "<key>" with value "<env>"
  When the loader resolves the configuration
  Then the value of "<key>" is "<expected>"
  # Covers: REQ-006, REQ-007

  Examples:
    | key    | default | file | env   | expected |
    | region | eu      | us   | ap    | ap       |
    | retries| 3       | 5    |       | 5        |
    | mode   | safe    |      |       | safe     |

Scenario: Missing required keys are listed together
  Given the file and environment provide none of the required keys
  When the loader resolves the configuration
  Then the error lists every required key exactly once
  # Covers: REQ-008

Scenario: Malformed file
  Given the configuration file is not parseable
  When the loader resolves the configuration
  Then a parse error is reported
  And no configuration is returned
  # Covers: REQ-009

Scenario: Successful merge
  Given all required keys resolve from at least one layer
  When the loader resolves the configuration
  Then the merged configuration is returned to the caller
  # Covers: REQ-010
```

## Assumptions

- Environment variable names are derived from key names by upper-casing and prefixing
  with the tool name; the exact rule is fixed once a stack is chosen.
- "Required key" is defined by the consuming component, not by this requirement.

## Status

Approved — see `P002-config-resolution.md`.
