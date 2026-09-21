---
name: disc-pc-init
description: "Build a discovery project's Product Context document (`product_context.md`) from scratch by incrementally synthesizing BA-provided sources (presales WBS, discovery transcripts, RFPs, kickoff notes, client answers, diagrams) into one traceable product source-of-truth with the §1–§13 structure. Use whenever there is no product context file yet and you are creating the initial skeleton and first population during Discovery. Once the file exists and a new source must be folded in, use disc-pc-update instead."
---

# **Discovery — Product Context From Zero**

Build `product_context.md` as the single product source-of-truth for a Discovery engagement, synthesized only from sources the BA provides. The document grows source by source; this skill owns the empty skeleton and the first build through to a usable draft. No Jira/Confluence is involved in Discovery — the file is delivered for the BA to save into Project Knowledge manually.

## **When to use vs disc-pc-update**

* **disc-pc-init (this skill):** no product context file exists yet. You create the §1–§13 skeleton and populate it from the first source(s) until the BA has a coherent first draft.
* **disc-pc-update (separate skill):** the file already exists and a new source must be folded in as targeted OLD→NEW edits. Hand off to it once the file is established.

The seam matters: this skill is allowed to create and shape the file; disc-pc-update is not — it only patches an existing one.

## **Inputs**

Any combination, provided gradually and in an order the BA chooses. Do not expect specific documents and do not constrain the source list in advance. Typical Discovery sources: presales WBS, discovery / elicitation transcripts, RFPs and client briefs, kickoff notes, client answers to questions, diagrams / Miro exports, designer responses. If the incoming material is mixed or unstructured, triage it with `disc-intake` first; this skill folds in whatever the BA hands over.

## **Core principles**

These are the reason the document stays trustworthy — hold them on every source.

1. Know NOTHING about the product upfront. All product knowledge comes only from sources the BA adds or pastes in. Nothing comes from prior projects, the origin org, or assumptions.
2. Do not invent. If something is not covered by a source, it goes into §9 Open Questions, not into a guess.
3. Do not hedge with "design assumption", "likely", or "presumably". If a source is silent, write "not defined in sources" and log it as an open question.
4. If two sources contradict, record both versions with source references and move the conflict into §9 Open Questions. Do not pick a winner.
5. Cite sources precisely. Every statement carries a traceable inline reference, e.g. `(Source X §3)`, `(WBS row 88)`, `(Source Z, 00:14:22)`.
6. Preserve domain terms exactly as a source spells them. Once a glossary term appears in a source, do not translate or rename it.
7. The PC records the decided state, not history — with one deliberate exception: §13 Decisions Log, the one place a short line of changed-decision history lives (Discovery keeps no separate changelog). Everything else is current state only.

## **Procedure**

### **1. Create the skeleton**

On the first source, instantiate the full §1–§13 skeleton below as `product_context.md`. Section names are fixed; every section starts empty and is populated only as a source supports it. Do not pre-fill any section with what the product "probably" has. §13 starts empty apart from a single baseline line noting the file was established.

### **2. Per-source loop**

After each source the BA provides:

1. Incrementally update the file:
   * Add new information into the right sections.
   * Refine prior statements where this source clarifies them.
   * Record contradictions in §9 (both versions, with citations).
   * Add the source to §11.1 with date and a short description.
   * Close items in §9 that this source answers — keep them visible with a `Closed by: <source>` note for traceability.
2. Deliver the full updated `product_context.md` as a file for the BA to save.
3. Give a short changelog in chat (5–10 lines): which sections changed, which new capabilities / rules / entities / roles / integrations / constraints were added, which contradictions were recorded, which open questions were opened or closed.
4. Wait for the next source. Do not rebuild the document from scratch to show progress — updates are targeted and traceable.

### **3. Finalize**

When the BA says "збираємо" / "build product_context" / "finalize": verify the document is internally consistent, every section is populated where sources allow, the permission matrix legend is defined, and §9 Open Questions is current. Label load-bearing statements `[client decision]` / `[my inference]` / `[assumption to confirm]` where the distinction is not already obvious from the citation.

## **Document skeleton**

Instantiate exactly this structure. Slots are intentionally empty until a source fills them.

```
# Product Context — <project name from sources>
_Last updated: <date> · Sources processed: <list with dates>_

## 1. Product overview
Two-paragraph description of what the product is, for whom, key value
proposition, and business model(s). All facts must come from sources.

## 2. Users and roles
Create subsections only if sources support them:
- 2.1 Role hierarchy — roles as they appear in sources, each with a brief description.
- 2.2 User attributes — any attribute tracked on the user record.
- 2.3 User states and transition rules — states a user can be in, and what triggers transitions.

## 3. Domain model
Subsections emerge from sources (e.g. core entity, group / membership rules,
content model, messaging model). Only include subsections sources provide content for.

## 4. Functional domains
One subsection per functional area; domain names emerge from sources, not a fixed list.
For each domain: capabilities offered, which roles can perform which capability
(cite a source for each role-capability pair), business rules and constraints.

## 5. System rules and automations
Rules that run without a role triggering them: time-based state transitions,
automated data linking, mandatory validations, data-preservation rules, etc.

## 6. Integrations
External systems the product integrates with. For each: purpose, direction
(inbound / outbound / bidirectional), constraints. WHAT integrates and WHY, not tech stack.

## 7. Out of scope (MVP)
What is explicitly excluded from the MVP per sources.

## 8. Parallel tracks
Initiatives running alongside the MVP without blocking it. Status + known details per source.

## 9. Open questions
Items where sources are silent, ambiguous, or contradictory. For each:
- the question
- date and source where it first appeared
- routing: Client / Internal
- status: Open / Closed by <source>
- if closed: the resolution and the source that resolved it
Sort most-blocking-first.

## 10. Glossary
Domain-specific terms as they appear in sources. Each term: definition + source reference.

## 11. Source map
11.1 All sources processed (name, date, short description, type).
(Line-by-line traceability is not kept here — inline citations in each section carry it.)

## 12. Permission matrix
One consolidated feature-level table of permission-bearing capabilities across roles.
Not field-level — one row per capability, not per data field.
Columns: Domain (matches §4) · Capability · one column per role · Source(s) · Confidence.
Cell values: ✅ / ❌ / ✅ scoped / ❓ / N/A — pick consistently and define the
legend at the top once roles are known.

## 13. Decisions Log
The one historical lane in the PC. One line per changed decision, appended by
disc-pc-update — not the initial decisions, only later changes to them.
Columns: Date · What changed (old → new) · Why · Source.
At init, seed with a single baseline line: "<date> — PC established from <sources>."
```

## **What NOT to do**

* Do not write user stories or acceptance criteria — that is downstream work (`disc-wbs-story-build` / `disc-bullet-ac`).
* Do not create an NFR section — non-functional requirements live in a separate `nfrs` file owned by `disc-nfr`; reference it, do not duplicate it here.
* Do not structure §3 or §4 by WBS epics or by UI flows. Structure emerges from the product's own domain language as sources express it.
* Do not split sub-artifacts (permission matrix, open questions, decisions log) into separate files — everything except NFRs lives inside `product_context.md`.
* Do not write chronology into §1–§12 — only §13 carries history.
* Do not rebuild the document from scratch on update — increment only.
* Do not pre-fill any section with assumptions about what the product might contain.

## **Style**

Direct, no preamble. Markdown. Bullet points are full sentences, not fragments. Tables where they add clarity (especially §12).

## **Output**

Save as `product_context.md`. Deliver the full file after each source, plus the in-chat changelog. The BA saves the file into Project Knowledge manually — no external write.

## **Update trigger**

Once the file exists and is in active use, fold new sources in with the **disc-pc-update** skill, not this one. Return to disc-pc-init only when starting a brand-new project's product context.
