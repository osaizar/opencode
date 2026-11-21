# Verification: R001

> Worked example of a verifier report. The findings below were found in an early draft
> of R001 and are shown resolved; the requirement in `docs/requirements/` includes the
> amendments.

## Summary

The requirement is internally consistent and every statement is covered by an acceptance
scenario. Two ambiguities found in the draft were resolved before approval. Verdict:
**pass** for the specification stage. No implementation exists yet, so implementation
scenarios are recorded as "not verifiable" rather than passed.

## Standards

Not applicable — no implementation exists yet. Once code exists, this section carries the
Standards axis from the `code-review` skill (documented repo standards plus the smell
baseline).

## Spec

All five EARS statements are testable and each is covered by at least one Gherkin
scenario. The two ambiguities listed under Findings were resolved in the approved draft.

## Acceptance criteria

| Scenario | Status | Evidence |
|---|---|---|
| Custom name | not verifiable | No implementation exists yet; scenario is testable as written. |
| Default name | not verifiable | No implementation exists yet; scenario is testable as written. |
| Too many arguments | not verifiable | Exit code 2 is now specified in REQ-004. |
| Empty name | not verifiable | Behaviour is now specified in REQ-005. |

## Checks

- No automated checks configured.
- Static review of `R001-greeting-cli.md`: all five EARS statements have unique IDs and one
  behaviour each.

## Findings

1. **[blocker, resolved]** REQ-004 originally said "print a usage error" without naming
   the stream or exit code, making it untestable. Amended to stderr and exit code 2.
2. **[suggestion, resolved]** Empty-string names were undefined: REQ-002 said "non-empty
   name" but no statement covered the empty case. Added REQ-005 with a matching scenario.
