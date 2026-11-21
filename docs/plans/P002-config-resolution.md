# P002 — Configuration resolution

## Summary

Implement layered configuration resolution for `R002-config-resolution.md` using a single
resolver that merges defaults, file values and environment values in a fixed order and
validates required keys before returning.

**Requirement:** `R002-config-resolution.md`
**Related decisions:** `ADR001-config-precedence.md`

## Approach

- Represent each layer as a plain key/value map.
- Merge in increasing precedence: defaults → file → environment. Last writer wins, which
  makes precedence explicit and easy to test.
- Validate required keys only after merging, collecting all missing keys into one error.
- Parse the file fully before merging so a parse error cannot produce partial state.

## Alternatives considered

| Alternative | Why rejected |
|---|---|
| Each consumer reads the layers it needs directly | Precedence rules would be duplicated and drift. |
| Deep merge of nested objects | No nested configuration is in scope; shallow merge is simpler and deterministic. |
| Fail on the first missing key | REQ-008 requires listing every missing key. |

## Library APIs

The configuration file format and environment helpers depend on the stack. **Once a stack
is chosen, resolve the relevant library ID with Context7 and record it here before
finalising API calls.** No library ID is fixed by this plan.

## Changes

- **Resolver** — merge the three layers and expose a single `resolve()` result.
- **Validation** — collect missing required keys and raise one aggregated error.
- **File parsing** — parse to a complete map before merging; surface parse failures.
- **Tests** — scenario outline for precedence plus error-path tests.

## Test strategy

- Table-driven test using the `Examples` rows from R002.
- Error-path tests asserting that parse failures return no configuration and that missing
  keys are all listed.
- Once a stack is chosen, add its test command to `AGENTS.md` and run it here.

## Risks

| Risk | Mitigation |
|---|---|
| Environment variables silently override intentional file values | Precedence is documented in ADR001 and covered by the scenario outline. |
| Partial configuration applied after a parse error | Parse into a complete map before any merge. |

## Tasks

- [ ] Add the layer representation and merge function.
- [ ] Add file parsing with full-parse-before-merge.
- [ ] Add required-key validation with aggregated errors.
- [ ] Add tests for precedence, missing keys, parse failure and success.
- [ ] Resolve library IDs with Context7 once the stack is chosen.
- [ ] Run available checks and the verifier subagent.

## Traceability

| Requirement | Change | Test |
|---|---|---|
| REQ-006 | Resolver layers | All scenarios |
| REQ-007 | Merge order | `Precedence` outline |
| REQ-008 | Validation | `Missing required keys are listed together` |
| REQ-009 | File parsing | `Malformed file` |
| REQ-010 | Resolver result | `Successful merge` |

## Status

Approved.
