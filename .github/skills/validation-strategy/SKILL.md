---
name: validation-strategy
description: 'Plan and execute focused validation for code changes. Use for tests, regression checks, and risk-based verification.'
argument-hint: 'Changed behavior, risk areas, touched modules'
---

# Validation Strategy

## Purpose
Define and execute pragmatic, risk-based validation after code changes.

## When To Use
- After implementing bug fixes, features, or refactorings
- When deciding which tests/checks are necessary
- When validating regressions and edge cases

## Procedure
1. List changed behaviors and potential side effects.
2. Define required checks per risk — evaluate autonomously which categories apply:
   - **Happy path** — core behavior with valid inputs
   - **Edge cases** — unusual but valid inputs, empty collections, null-forgiving contexts
   - **Negative testing** — invalid inputs, missing data, malformed payloads
   - **Error handling** — exceptions, graceful degradation, fallback paths
   - **Boundary value analysis** — limits, thresholds, off-by-one, min/max ranges
   - **Concurrency** — races, parallel access, thread safety (only when shared state exists)
   - **Idempotency** — repeat calls, duplicate messages, retry safety (only when relevant)
   - **Regression path** — previously fixed bugs must not reappear
3. Prefer existing test framework and patterns in the repo.
4. Add/update tests only where behavior changed.
5. Run the narrowest build/test/type/lint checks for touched scope.
6. Summarize validation outcome and remaining risks.

## Guardrails
- Avoid over-testing unrelated areas.
- Keep assertions explicit and behavior-focused.
- If full verification is not possible, document residual risk clearly.
