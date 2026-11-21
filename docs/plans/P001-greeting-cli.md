# P001 — Greeting CLI

## Summary

Implement the greeting command described by `R001-greeting-cli.md`: parse one optional
positional argument, print the correct greeting to stdout, and emit usage errors to stderr
with deterministic exit codes.

**Requirement:** `R001-greeting-cli.md`
**Related decisions:** none

## Approach

- A single entrypoint handles argument parsing and delegates formatting to a pure
  `formatGreeting(name)` function with a fixed default value.
- Argument validation happens before output: zero arguments use the default, one non-empty
  argument uses it, anything else produces a usage error on stderr.
- Keep the formatter pure so it can be tested independently of process I/O.

## Alternatives considered

| Alternative | Why rejected |
|---|---|
| Format directly in the entrypoint | Mixes I/O and logic; harder to test the four scenarios precisely. |
| Use a full argument-parsing framework | Overkill for one optional argument; adds a dependency before a stack is chosen. |

## Changes

- **Entrypoint** — read positional arguments, validate count and emptiness, write to
  stdout/stderr, set the exit code.
- **Formatter** — pure function returning the greeting string; owns the default `World`.
- **Tests** — one test per Gherkin scenario, asserting stdout, stderr and exit code.

## Test strategy

- Unit-test the formatter for default and custom names.
- Drive the CLI as a subprocess for argument-count and exit-code scenarios, so stderr and
  exit codes are observable.
- Once a stack is chosen, add its test command to `AGENTS.md` and run it here.

## Risks

| Risk | Mitigation |
|---|---|
| Platform newline differences make exact-output assertions flaky | Compare after normalising the trailing newline in the test helper. |
| Exit code lost when the formatter throws | Validate arguments before formatting and keep the formatter pure. |

## Tasks

- [ ] Add the formatter with the default value.
- [ ] Add argument validation and stream/exit-code handling.
- [ ] Add tests for all four scenarios.
- [ ] Run available checks and the verifier subagent.

## Traceability

| Requirement | Change | Test |
|---|---|---|
| REQ-001 | Entrypoint argument parsing | All four scenarios |
| REQ-002 | Formatter + stdout path | `Custom name` |
| REQ-003 | Formatter default | `Default name` |
| REQ-004 | Argument-count validation | `Too many arguments` |
| REQ-005 | Empty-string validation | `Empty name` |

## Status

Approved.
