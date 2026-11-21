# R001 — Greeting CLI

## Purpose

A minimal command-line tool that prints a greeting, used as the worked example for the
requirements-first workflow. It is intentionally stack-neutral.

## Scope

- In scope: one optional positional argument, greeting output, usage errors.
- Out of scope: configuration files, colour output, internationalisation.

## Requirements

### REQ-001 — Optional name argument

**Type:** ubiquitous

The tool shall accept a single optional positional argument named `name`.

### REQ-002 — Greeting with a name

**Type:** event-driven

When the tool is invoked with a non-empty `name`, the tool shall print `Hello, <name>!` to
stdout and exit with code 0.

### REQ-003 — Default greeting

**Type:** state-driven

While no `name` argument is provided, the tool shall print `Hello, World!` to stdout and
exit with code 0.

### REQ-004 — Too many arguments

**Type:** unwanted

If more than one positional argument is provided, then the tool shall print a usage error
to stderr and exit with code 2.

### REQ-005 — Empty name

**Type:** unwanted

If the `name` argument is an empty string, then the tool shall print a usage error to
stderr and exit with code 2.

## Acceptance criteria

```gherkin
Scenario: Custom name
  Given the tool is invoked with the single argument "Ada"
  When it runs
  Then stdout is exactly "Hello, Ada!"
  And the exit code is 0
  # Covers: REQ-001, REQ-002

Scenario: Default name
  Given the tool is invoked with no arguments
  When it runs
  Then stdout is exactly "Hello, World!"
  And the exit code is 0
  # Covers: REQ-001, REQ-003

Scenario: Too many arguments
  Given the tool is invoked with the arguments "Ada" "Grace"
  When it runs
  Then stderr contains a usage error
  And stdout is empty
  And the exit code is 2
  # Covers: REQ-004

Scenario: Empty name
  Given the tool is invoked with the single argument ""
  When it runs
  Then stderr contains a usage error
  And stdout is empty
  And the exit code is 2
  # Covers: REQ-005
```

## Assumptions

- "Argument" means a shell positional argument after the program name.
- Output strings are compared exactly, including the trailing newline convention of the
  chosen platform once a stack is selected.

## Status

Approved — see `P001-greeting-cli.md`.
