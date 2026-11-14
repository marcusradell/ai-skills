---
name: tdd
description: Test-driven development.
---

# Test-driven development

## Methodology

These are some ways-of-working to adhere to and some authorative figure that mentioned them:

1. Test lists by JB Rainsberger
2. Shallow integration tests by James Shore
3. YAGNI by Kent Beck
4. Observability by Charity Majors
5. Trunk-based development by Dave Farley
6. Characterization testing by Michael Feather

## Test list

Write a test list according to TDD and ZOMBIES.

Only write test cases for the ZOMB-letters and not for the abstract IES-letters. Ensure that the test list is as simple as possible.

Use given/when/then syntax for the test titles, and keep it terse.

## Layers

We want tests on three different layers, going from outside towards inside layer:

1. Business layer. Observability monitors against production infrastructure.
2. Integration layer. Integration tests written against local infrastructure.
3. Logic layer. Unit tests written against pure functions. No mocks.

## Test

Write one automated test at a time, following the red/green/refactor cycle and arrange/act/assert setup in the test.

Temporary tests can be removed in a refactor step once they are not needed.
