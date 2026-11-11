# Tests

**Status:** No executable tests are defined. This directory establishes the future test policy.

## Test organization
- `core/` — core component tests, when components exist.
- `workflows/` — workflow-specific tests.
- `integration/` — interactions between approved components.
- `fixtures/` — non-sensitive, synthetic test data.

## Requirements
Every test should state its ID, purpose, setup, exact version, steps, expected result, actual result, and evidence. Use synthetic or explicitly authorized test data. Keep test credentials out of source control.

## Result states
Use **PASS**, **FAIL**, **BLOCKED**, and **NOT RUN**. A result applies only to the version and environment actually tested. Do not infer release approval from a passing test.
