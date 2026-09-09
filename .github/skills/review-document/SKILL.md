---
name: review-document
description: 'Create structured German review documents with fixed sections and severity-based findings. Use when a standalone review artifact is requested.'
argument-hint: 'Review scope (PR/files/module) and optional references'
---

# Review Document

## Purpose
Generate a concise, structured review document in German.

**Output language: German** — all section content, findings, and recommendations must be in German.

## Usage
Do not ask follow-up questions. Derive missing metadata from prompt and context.

Save the review document in the '.documentation/.reviews/' folder. Extract the counter from the referenced concept's filename (e.g. `00008-c-...` → counter `00008`) and use the naming scheme `<conceptCounter>-rv-<name>.codeReview.md`. The review shares the same counter as the concept it relates to.

The review is not listed in `00000-issues.md` — only concepts appear there. Reviews are found by naming convention (`<counter>-rv-<slug>.codeReview.md`).

If no concept document is referenced, use the following fallback:
- Save the review under `.documentation/.reviews/`.
- Use the filename format `<yyyyMMdd>-rv-<short-slug>.codeReview.md`.
- Leave the `Issue` row empty as `| **Issue:** | |`.
- State in `# 1. Scope & Kontext` that the review was conducted without a referenced concept document.
- Create `.documentation/.reviews/` if it does not exist.
- The missing concept counter is not an error in this fallback case.

Apply these rules before generating the document:
- Derive a short German title from scope/topic
- Set `Erstelldatum` and `Letzte Änderung` to today's date
- If no explicit `Issue` reference is provided and the referenced concept counter is known, set `Issue` automatically to that counter.
- Include `Task` and `PBI` only if explicitly provided.
- Use the empty fallback row `| **Issue:** | |` only if no Issue reference and no concept counter are available.
- Findings tables contain only actual findings. Do not list confirmed non-findings, optional alternatives, or merely applicable-but-unproblematic content as findings.
- Do not add or update a commit proposal in review documents.
- Clickable Markdown links are strongly encouraged—to source code, external documentation, and sections of this document, where appropriate

All sections must be present. If no content exists for a section, write `— entfällt —`.
Findings in Kapitel 3–5 use the severity scale `Critical`, `Major`, `Minor`, `Suggestion`. Every
finding must name one preferred corrective action and its rationale; alternatives may be included
only as context and must not remain co-equal. This
finding severity is separate from the Kapitel 7 risk level (`kritisch`, `hoch`, `mittel`,
`kosmetisch`).
Use **Kapitel 7. Risiken** only for risks where a concrete preferred action and rationale can be given. `Nichts tun` is a valid option if explicitly justified. Every open point in **Kapitel 8. Offene Punkte** must include a recommended resolution and rationale, unless a decision-critical fact is unavailable; then state the missing fact, how to obtain it, and the provisional action with rationale.
Begin **Kapitel 7** with a three-column table containing all risks: risk ID, one-sentence description, and risk level (`kritisch`, `hoch`, `mittel`, `kosmetisch`).
Begin **Kapitel 8** with a three-column table containing all open points: open-point ID, short description, and status (`✅ geklärt` or `❌ zu klären`). Every heading in Kapitel 8 must include the matching leading status marker (`✅` or `❌`) after the section number.

## Template
Use the template from [review-template.md](review-template.md).

Copy the full structure from [review-template.md](review-template.md) and replace placeholders with review-specific content.
