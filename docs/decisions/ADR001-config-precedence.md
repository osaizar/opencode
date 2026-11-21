# ADR001 — Configuration precedence

## Status

Accepted

## Context

Configuration can come from built-in defaults, a configuration file, or environment
variables. R002 requires a deterministic rule for which value wins, and the rule must work
in local development, CI and deployed environments without surprising overrides.

## Decision

Precedence is `defaults < configuration file < environment variables`. The resolver merges
layers in that order, so a later layer overwrites an earlier one.

## Consequences

### Positive

- Environment variables can override committed configuration in CI without editing files.
- Defaults keep behaviour predictable when no configuration is supplied.
- The rule is a single ordered merge, easy to test with a scenario outline.

### Negative

- An environment variable can silently override an intentional file value.
- Operators must know the order; it is documented here and in `AGENTS.md` once a stack is
  chosen.

## Alternatives

| Alternative | Reason not chosen |
|---|---|
| File overrides environment | CI would have to edit files to change behaviour. |
| Error when a key is defined in multiple layers | Conflicts are expected between defaults and overrides; erroring would make overrides unusable. |

## Related

- Requirement: `R002-config-resolution.md`
- Plan: `P002-config-resolution.md`
