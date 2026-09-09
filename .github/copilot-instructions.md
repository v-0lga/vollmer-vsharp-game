# Copilot Instructions

## General Guidelines
- Keep comments and README content in German; the user prefers German for documentation and comments, while code and test method names should be in English.
- For reading and searching files, use the available workspace read/search tools. Do not use PowerShell file-reading commands such as `Get-Content`; use PowerShell only for execution side effects where no dedicated workspace tool is available.
- The user expects absolute transparency: missing or unimplemented items must be stated clearly instead of being implied.

## Logging Preferences
- Do not replace `ex.Message` with `ex` in logs unless it truly outputs only the message; keep `ex.Message` for logs by default.

## Concept Implementation
- For concepts with a matching `.progress.md`, the sidecar is the binding work list.
- Before working on a package, mark it `🔄 in Arbeit`; never start more than one package at a time.
- Report a concept implementation as complete only when every requested work package is `✅ erledigt`. The user must explicitly define any narrower scope.
- If work stops early or is blocked, update `.progress.md` with status, validation evidence, the exact next work package, and all blockers. Do not imply that the implementation is complete.

## Decision Standard
- For every option set, open point, risk, and review finding, provide one concrete preferred action and a concise, evidence-based rationale.
- Alternatives may provide context, but never replace a clearly identified preferred option.
- Do not end with co-equal options or an open question when the available facts support a recommendation.
- Leave a decision open only when a decision-critical fact is unavailable. State the missing fact, how to obtain it, the provisional action, and the rationale for that action.

## Build and Test Validation
- Prefer project-provided, non-interactive build and test scripts when they exist. Before using one, verify its path, scope, and required parameters in the current project.
- If no project-specific script exists, use the project's documented native build and test commands. Do not invent repository-wide scripts or test categories.
- Do not buffer or truncate validation output in a way that hides failures.
- If a validation command times out, stop, report the affected project and available log path, and do not retry blindly.

## Test Selection and Runtime (Time Saving, Mandatory)
- Before every test run, choose the smallest meaningful scope and justify it in one sentence. Without a filter, the runner executes all tests of a project by default — that is only rarely justified.
- Determine the exact test method name by searching the test project before running a single test — never guess.
- Skip a build only when the project's own tooling supports that mode and the relevant production code has not changed. Build each project at most once per change set unless there is a documented reason to repeat it.
- Run validation commands unfiltered so real-time status remains visible; do not append pipelines that hide or truncate failures.
- After every test run, briefly report: the filter used, passed/failed/skipped counts, and whether the build was skipped.
- When tests fail, state the smallest reproducing filter with which the failures can be isolated.

## Testing Guidelines
- For tests in this repo, keep test method names in English and add 1-2 sentence summary comments explaining what each new test verifies.
- The user values high test coverage and edge-case tests; tests must not be removed lightly.
- In already categorized areas, tag new tests with the same `TestCategory`; in files without categories, follow the pattern used there.