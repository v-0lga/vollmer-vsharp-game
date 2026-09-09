---
name: bug-fix
description: 'Find and fix root-cause defects in supported codebases. Use when investigating crashes, incorrect behavior, memory issues, regressions, or unexpected exceptions. Not for new features or refactorings (use concept-plan instead). Apply language-specific guidance only when it matches the affected project.'
argument-hint: 'Short bug description or error message'
---

# Bug Fix

## Purpose
Guide a structured bug investigation and fix in the affected project. **Regression test first, fix second** — the test must fail before the fix and pass after. Match the language, test framework, and validation commands to the repository context. Use German for output when the project documentation establishes German as its documentation language.

## Procedure
### 1. Reproduce
- Clarify exact steps to reproduce the bug (input, state, environment)
- Confirm whether the bug is deterministic or intermittent
- Identify the failing assertion, exception, crash, or wrong output

### 2. Isolate
- Narrow down to the smallest reproducing case
- Identify the affected component, class, function, or module
- Check recent changes (git log/blame) that could have introduced the regression

### 3. Root-Cause Analysis
- Trace execution path to the failure point
- Identify the actual defect (logic error, null reference, race condition, memory corruption, etc.)
- Document the root cause in 1–3 bullet points before touching any code

### 4. Regression Test (failing) — **FIRST, before the fix**
- Write a test that reproduces the root cause and **fails with the current code**
- The test must exercise the exact defect path identified in the root-cause analysis
- If the bug is an integration-level wiring defect (missing event subscription, uninitialized dependency, etc.), write an **integration test** that validates the complete chain — unit tests that mock away the defect are worthless here
- Use the concrete reproduction data (real inputs, real timestamps, real prices) wherever possible
- Decision guide for test type:
  - **Unit test:** logic error inside a single class/method, pure calculation bug, wrong branch condition
  - **Integration test:** missing wiring between components, event not subscribed, dependency not passed, data not flowing through the pipeline
- Run the test and **confirm it fails** before proceeding — a test that passes before the fix is not a valid regression test

### 5. Fix
- Make the minimal targeted change — do not refactor unrelated code
- Ensure the fix addresses the root cause, not just the symptom
- Apply language-specific checks (see below)
- Run the regression test from step 4 — it must now pass

### 6. Verify
- Confirm the original reproduction case no longer fails
- Run the **full existing test suite** — no regressions
- Check for side effects on related functionality
- If the fix touches shared infrastructure (constructor signatures, DI wiring, event pipelines), verify ALL consumers still work

## Language-Specific Notes
### C#
- Check for `NullReferenceException`: use `?.`, null checks, or `ArgumentNullException.ThrowIfNull()`
- Async bugs: verify `await` usage, avoid `async void`, check `ConfigureAwait`
- Threading: check for shared mutable state without locks, use `Interlocked` or `lock` correctly
- Resource leaks: ensure `IDisposable` objects are in `using` blocks
- Use the debugger (breakpoints, watch, call stack) and structured logging to trace state

### C++
- Memory: check for use-after-free, double-free, buffer overflows — use AddressSanitizer (`-fsanitize=address`) or Valgrind
- Undefined behavior: uninitialized variables, signed integer overflow, strict aliasing violations
- Threading: data races → use sanitizers (`-fsanitize=thread`), verify mutex scope
- Check compiler warnings at max level (`/W4` MSVC, `-Wall -Wextra` GCC/Clang) — treat new warnings as errors
- Prefer RAII and smart pointers (`unique_ptr`, `shared_ptr`) over raw ownership

## Output Summary (in German)
After completing the investigation, produce a short summary with:
- **Symptom:** Was war das beobachtete Fehlverhalten?
- **Ursache:** Was war die eigentliche Ursache?
- **Regressionstest (failing):** Welcher Test wurde VOR dem Fix geschrieben und hat mit dem defekten Code rot geschlagen? Warum wurde Unit- vs. Integrationstest gewählt?
- **Fix:** Welche Änderung wurde vorgenommen und warum? (Der Test aus dem vorherigen Schritt ist jetzt grün.)
- **Verifikation:** Wie wurde die Korrektur bestätigt? (Volle Test-Suite, manueller Repro-Test, etc.)
