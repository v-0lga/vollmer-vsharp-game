---
name: implementation-workflow
description: 'Execute structured implementation work with minimal diffs. Use when implementing a feature/refactoring task without concept authoring.'
argument-hint: 'Task, scope, constraints, acceptance criteria'
---

# Implementation Workflow

## Purpose
Provide a compact, repeatable implementation flow for end-to-end coding tasks.

## When To Use
- Feature implementation without separate concept document
- Refactoring with clear target behavior
- Multi-step implementation tasks that need structure

## Procedure
1. Check for a matching concept document in `.documentation/.inbox/` (files with `-c-` prefix and `.concept.md` suffix). If found, locate the corresponding `.progress.md` sidecar (same base slug, `-p-` prefix, e.g. `00009-c-some-topic.concept.md` → `00009-p-some-topic.progress.md`). The sidecar shares the same counter as the concept. If no sidecar exists, create one using the concept's counter and the naming scheme `<conceptCounter>-p-<conceptName>.progress.md`. Use [progress-template.md](../concept-plan/progress-template.md) for structure. Derive the work packages from Kap. 5 of the concept and pre-populate the sidecar table before starting. The sidecar is the binding work list for the implementation and is not listed in `00000-issues.md`.
2. Analyze context and identify impacted files/components.
3. Create a short task list with small verifiable steps from the work packages.
4. If a `.progress.md` sidecar exists, resume its `🔄 in Arbeit` package or mark the next `⬜ offen` package in the defined order as `🔄 in Arbeit` before editing. Never start a second package while another is in progress.
5. Implement the current work package with minimal, non-invasive changes.
6. Remove obsolete code in touched scope when safe.
7. Re-check assumptions if complexity increases.
8. Validate the current work package with the narrowest useful build/test checks.
9. Update relevant docs when behavior/contracts changed. Only after implementation, required validation, and documentation are complete, mark the current work package as `✅ erledigt` with today's date and its validation evidence.
10. Repeat steps 4-9 until all work packages are `✅ erledigt`, unless the user explicitly limited the requested implementation scope.
11. If work must stop before completion, update the `.progress.md` sidecar with the current status, validation evidence, and the exact next work package. Report completed packages, the next package, blockers, and validation already performed; do not report the implementation as complete.
12. If the concept contains `Commit-Vorschlag für den finalen Gesamt-Commit`, update it with the actually implemented changes, tests, and documentation after all requested work packages are complete. Keep its status `vorläufig`; do not create a commit or mark the proposal `final` before the explicitly requested final code review succeeds.

## Completion Gate
- A concept implementation is complete only when every work package in the `.progress.md` sidecar is `✅ erledigt`, unless the user explicitly limited the requested implementation scope.
- Use `⛔ blockiert` only when further progress requires an external decision or unavailable prerequisite. Record the concrete cause and required next action in the sidecar.
- Never present a partially completed concept implementation as complete, and never leave the sidecar stale after an interruption or blocked execution.

## Guardrails
- Keep scope tight; avoid unrelated refactorings.
- Prefer existing patterns over new abstractions.
- Clarify trade-offs only when behavior/UX/architecture is materially affected.
- Do not create concept documents here; use `concept-plan` for that.