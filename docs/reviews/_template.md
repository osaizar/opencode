# Verification: R###

> Verification report template. Save to `docs/reviews/<requirement-id>-verification.md`.

## Summary

<One paragraph: verdict and the main reason.>

## Standards

<Findings from the Standards axis, with `file:line` references. If there is no code to
review, write "Not applicable — no implementation exists yet.">

## Spec

<Findings from the Spec axis, quoting the requirement line for each. If there is no code
to review, write "Not applicable — no implementation exists yet.">

## Acceptance criteria

| Scenario | Status | Evidence |
|---|---|---|
| <scenario name> | pass / fail / not verifiable | <command output or observed behaviour> |

## Checks

- <command>: <result>
<!-- If no automated checks are configured, write exactly: "No automated checks configured." -->

## Findings

1. **[blocker|suggestion]** <finding> — `file:line`

## Verdict

<pass / fail — pass requires all acceptance scenarios to pass and no blocking findings.>
