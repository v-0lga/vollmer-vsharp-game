---
name: concept-plan
description: 'Create structured concept and implementation plan documents. Use when designing a new feature, use case, or larger refactoring — NOT for bug fixes (use the bug-fix skill instead). Produces a standardized German-language document with a fixed set of sections that are always present.'
argument-hint: 'Short topic description'
---

# Concept & Implementation Plan

## Purpose
Generate structured concept and implementation plan documents for **features, use cases, and larger refactorings**. Not intended for bug fixes — use the `bug-fix` skill for those.

**Output language: German** — all generated content must be written in German, including section content, bullet points, diagram labels, and explanations.

## Usage
Do not ask the developer follow-up questions. Derive all missing document metadata from the provided description and context.

Save the concept document in the '.documentation/.inbox/' folder using the naming scheme `<nextCounter>-c-<conceptName>.concept.md`. Determine the next free counter by reading `.documentation/00000-issues.md` and incrementing the highest counter by 1.

Apply these rules before generating the document:
- Assign the next free global counter as prefix (e.g. `00012-c-`). This counter is the concept's unique ID. All derived documents (progress sidecars, reviews) reuse the same counter.
- Derive a **short, precise German title** from the topic description, use it as `<conceptName>` in the filename (lowercase, hyphens, no umlauts).
- Set **Erstelldatum** and **Letzte Änderung** to today's date
- Header-Referenzen priorisieren: Falls der User explizite Referenzen liefert, übernimm **Issue**, **Task** und **PBI** entsprechend.
- Wenn keine explizite **Issue**-Referenz geliefert wurde, setze **Issue** automatisch auf den ermittelten Concept-Counter (z. B. `00025`).
- Falls keinerlei Referenzen vorhanden sind, bleibt die **Issue**-Zeile dennoch mit dem Counter befüllt; die leere Fallback-Zeile `| **Issue:** | |` wird nur verwendet, wenn der Counter ausnahmsweise nicht ermittelbar ist.
- Include **Ausarbeitung** only if the user explicitly provided it
- Add a mandatory section `## Commit-Vorschlag für den finalen Gesamt-Commit` with a ready-to-use preliminary commit text block.
- Set the commit proposal status to `vorläufig`. It must explicitly state that no separate concept commit is created and that the proposal will be finalized only after implementation and the final successful code review.
- Commit-Vorschlag format:
	- Subject: begins with the Concept-Counter and describes the expected final change; do not prescribe `docs` or another commit type during concept creation.
	- Body with 2-4 bullets covering the planned scope, key decision, planned validation, and risks/open points (if present).
- Clickable Markdown links are strongly encouraged—to source code, external documentation, and sections of this document, where appropriate

After saving the concept file, append a new row to `.documentation/00000-issues.md` with the counter, date, status `🆕 new`, and document link.

Also create a matching progress sidecar in the same folder using the SAME counter as the concept with prefix `-p-`: `<conceptCounter>-p-<conceptName>.progress.md`. Use [progress-template.md](progress-template.md) for the sidecar structure. The sidecar is not listed in `00000-issues.md` — only concepts appear there. Sidecars are found by naming convention (`<counter>-p-<slug>.progress.md` alongside `<counter>-c-<slug>.concept.md`).

Then produce the complete document using the template below.

**All 9 sections (Kap. 0–8) must always be present.** If a section has no content for this specific document, write `— entfällt —` beneath the heading. Never skip or remove a section.
Every chapter Kap. 1–6 begins with a `## tl;dr` subsection of 2–4 bullet points summarizing the key facts of that chapter. Kap. 0 is a strict 5-row table; never add free text there.
Kap. 5 must define every implementation phase as an ordered work package `AP-##` that can be resumed independently. For every work package, state: goal/scope, affected components, dependencies, required validation, and a concrete done criterion. The same work packages are copied unchanged into the `.progress.md` sidecar.
Every decision, trade-off, risk, and open point must include a concrete preferred action and a concise rationale grounded in repository facts or explicit decision criteria. Alternatives may be listed, but must never remain co-equal when a recommendation is possible. A decision may remain open only when a decision-critical fact is unavailable; state the missing fact, how to obtain it, the provisional action, and its rationale.
Use **Kapitel 7. Risiken** only for risks where a concrete recommendation can be made. The recommendation must name the preferred action and why it is preferred; `Nichts tun` is a valid option if explicitly justified. Put unclear facts, missing decisions, unresolved assumptions, missing references, and questions that would otherwise require follow-up into **Kapitel 8. Offene Punkte**.
Begin **Kapitel 7** with a three-column table containing all risks: risk ID, one-sentence description, and risk level (`kritisch`, `hoch`, `mittel`, `kosmetisch`).
Begin **Kapitel 8** with a three-column table containing all open points: open-point ID, short description, and status (`✅ geklärt` or `❌ zu klären`). Every heading in Kapitel 8 must include the matching leading status marker (`✅` or `❌`) after the section number.

If the input is primarily an architecture/trade-off discussion:
- Capture at least 2 options with pros/cons in **Kapitel 5. Implementierungsplan**
- Add an explicit `Favorit` and rationale in **Kapitel 3 oder 5**; explain why it is preferred over the listed alternatives using at least two relevant criteria.
- Capture unresolved assumptions and decision dependencies in **Kapitel 8**

## Template
Use the template from [concept-template.md](concept-template.md).

Copy the full structure from [concept-template.md](concept-template.md) and replace placeholders with task-specific content.
