---
name: concept-review
description: 'Create structured concept review documents with a mandatory GO/NO-GO verdict. Use when reviewing a concept or architecture document against its stated goals, non-goals, risks, and open points. Distinct from review-document which targets code/PR reviews.'
argument-hint: 'Path to the concept document and optional review focus or constraints'
---

# Concept Review

## Purpose
Generate a structured review document for concept and architecture documents.

**Output language: German** — all section content, findings, and recommendations must be in German.

## Usage
Derive all metadata from the concept document. Do not ask follow-up questions.

Save the review document in the '.documentation/.reviews/' folder. Extract the counter from the concept's filename (e.g. `00008-c-...` → counter `00008`) and use the naming scheme `<conceptCounter>-rv-<conceptName>.conceptReview.md`. The review shares the same counter as the concept it reviews.

The review is not listed in `00000-issues.md` — only concepts appear there. Reviews are found by naming convention (`<counter>-rv-<slug>.conceptReview.md`).

Apply these rules before generating:
- Derive a short German title from the concept title
- Set **Erstelldatum** and **Letzte Änderung** to today's date
- Always link to the reviewed concept document in the header table
- If no explicit **Issue** reference is provided, set **Issue** automatically to the counter derived from the reviewed concept.
- Include **Task** and **PBI** only if explicitly provided.
- Use the empty fallback row `| **Issue:** | |` only if the counter cannot be derived.
- Do not add or update a commit proposal in review documents.
- Clickable Markdown links to source code, concept sections, and external docs are strongly
  encouraged where useful

**All 8 sections must be present.** Write `— entfällt —` if a section has no content.
Exception: Kap. 2 (Verdict) is always mandatory and must never be `— entfällt —`.

Findings use severity `Critical`, `Major`, `Minor`, `Suggestion`. This is separate from
the Kapitel 7 risk level (`kritisch`, `hoch`, `mittel`, `kosmetisch`).

Use **Kapitel 7** only for risks with a concrete preferred action and rationale (`Nichts tun` is
valid if explicitly justified). Every open point in **Kapitel 8** must include a recommended
resolution and rationale, unless a decision-critical fact is unavailable; then document the
missing fact, how to obtain it, and the provisional action with rationale.

## Template
Use the template from [concept-review-template.md](concept-review-template.md).

Copy the full structure from [concept-review-template.md](concept-review-template.md) and
replace placeholders with review-specific content.
