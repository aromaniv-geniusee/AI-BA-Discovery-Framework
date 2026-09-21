---
name: disc-solution-vision
description: "Assemble the client-facing Solution Vision & Scope deliverable (§1–§6: Introduction, Market Analysis, Business Requirements, Stakeholder Analysis, Solution Scope, Constraints) from the already-decided discovery artifacts — Product Context, 01_project_context, 02_stakeholders, 03_tech_context, the WBS, the nfrs file, and diagram/prototype outputs. Downstream-only: the SV is never a source and introduces no new decisions; a gap becomes a TBD + Open Question, never a fabrication. Emits paste-ready markdown blocks one section at a time with a review gate after each, in the SV's own shape (prose + the template's tables), for the BA to paste into the SV document manually. §7 Technical Vision and all appendices are SA-owned and never authored here. Run late in discovery, after disc-gap-check and disc-nfr VALIDATE. No external write."
---

# Discovery — Solution Vision Assembly

Assemble the final **Solution Vision & Scope** deliverable (§1–§6) from what discovery has already decided. This skill **composes, it does not decide**: every block is pulled from an existing artifact, rendered in the SV's own shape, and handed back **paste-ready, one section at a time**, for the BA to review and drop into the SV document. The SV is the discovery hand-off deliverable — the last thing assembled, downstream of everything, and **never itself a source**.

## Scope of this skill — read first

- **Assembles SV §1–§6 only.** §7 Technical Vision and every appendix after it are **SA-owned** — never authored here. Where the document needs continuity, insert a one-line "To be completed by SA" pointer; never fabricate architecture, stack, or environments.
- **Downstream-only, never a source.** It reads decided artifacts and introduces **no new decisions**. Where a source is silent, the block gets `TBD` + a note (and, if load-bearing, routes to an Open Question via `disc-pc-update` / `disc-nfr`) — never a guess.
- **Output is a proposal to paste-and-edit, per section — not finished client copy.** Provenance labels stay in the blocks so the BA sees what to verify; the BA strips them and finalises wording before the client sees the section.
- **No external write.** Paste-ready markdown; the BA pastes into the SV document (Google Doc) manually.

## When to invoke

- **Late in discovery, near close**, once the decided state is stable enough to assemble.
- **Mode: Solution Vision Assembly.** Run the pre-check first — `disc-gap-check` + `disc-nfr` (VALIDATE) — so you assemble from a clean, consistent state rather than papering over gaps.

## Sources → sections (read automatically)

| SV section | Primary source | Notes |
|---|---|---|
| 1.1 Purpose | template text | fill product/system name only |
| 1.2 Definitions | PC §10 Glossary | verbatim terms |
| 1.3 References | PC §11.1 sources + `nfrs` + diagram/BPMN links | |
| 1.4 Contacts | `01` + `02_stakeholders` | Geniusee + client; gaps → TBD |
| 1.5 / 1.6 / 1.7 | — | skeleton only (see §1 rules) |
| 2.1 / 2.2 / 2.3 | `disc-market-research` output (if any) | else skeleton; never blocks |
| 3.1 Company Profile | `01` Project Overview | |
| 3.2 Business Problem | `01` + PC §1 | |
| 3.3 Goals & Objectives | `01` Business Goal | SMART table |
| 3.4 Vision Statement | `01` | AUTO-draft, labelled |
| 3.5 Business Requirements | `01` + PC | BR-01… business-level, not FRs |
| 3.6 / 3.6.1 Outcome + Success Criteria | `01` | SMART |
| 3.7 Business Risks | `01` risks + context | AUTO-draft, labelled |
| 4.1 Stakeholder Profiles | `02_stakeholders` | pass-through, no new analysis |
| 5.1 Solution Approach | PC §1/§4 + `03` | AUTO-draft, labelled |
| 5.2 User Roles Summary | PC §2 (+ §12 for Key Actions) | |
| 5.3 General Solution Models | `disc-diagram-build` / `disc-prototype-build` | list + link placeholders |
| 5.4 Product Features & Releases | WBS Phase allocation | via `disc-wbs-schema` |
| 5.5.1 Solution Components | PC §6 Integrations (+ `03`) | DFD = placeholder |
| 5.5.2 Quality Attributes | `nfrs` file | **transform** (see §5 rules) |
| 5.6 Out of Scope | PC §7 | |
| 6.1 / 6.2 / 6.3 | PC §8 + `01` + `03` | Assumptions / Dependencies / Limitations |
| 6.4 Solution Risks + 6.4.1 RACI | context + fixed grid | AUTO-draft + standard RACI |
| 6.5 Licensing & Installation | `03` / SA | skeleton + TBD if no source |
| 6.6 Applicable Standards | `nfrs` REG / compliance | else TBD |

## Pre-flight — before writing

1. **Read the source artifacts** named above (PC, `01`, `02`, `03`, `nfrs`, and the WBS via `disc-wbs-schema`; plus any `disc-diagram-build` / `disc-prototype-build` / `disc-market-research` outputs).
2. **Present an inputs-and-gaps summary** (brd-style): what is available, what is missing per section, which sections will be **skeleton-only**. Note whether `disc-market-research` output exists for §2 — **propose adding it**; if not, §2 stays skeleton, **do not block**.
3. **Ask which sections to assemble this run** — all §1–§6, or a subset — and confirm the product/system name for 1.1.
4. Wait for the BA's go. Do not assemble before confirmation.

## Procedure — section by section, review gate after each

For each requested section §1…§6, in order:
- pull from the mapped source(s) only;
- render **paste-ready in the SV's own shape** — prose plus the template's tables;
- label load-bearing statements `[client decision]` / `[my inference]` / `[assumption to confirm]`; unknowns → `TBD` + a short note;
- name the artifact each block came from (a BA-facing trace line, stripped before the client);
- **present the block, then STOP for BA review** before moving to the next section.

### §1 Introduction
- **1.1 Purpose** — the template's standard paragraph; fill the product/system name.
- **1.2 Definitions** — from PC §10, terms verbatim; do not re-coin definitions.
- **1.3 References** — from PC §11.1 (sources) plus the `nfrs` file and any diagram/BPMN/prototype links.
- **1.4 Contacts** — from `01` (Geniusee side) and `02_stakeholders` (client side). Emails/phones/messengers → `TBD` where a source is silent; **never invent contact details**.
- **1.5 Document Versions / 1.6 Approvals / 1.7 Distribution** — **skeleton only**: emit the empty table and, for 1.5, a single seeded initial-version row. **Never generate approver names or an approval decision** — 1.6 stays Pending. These are BA-maintained.

### §2 Market Analysis
- **Not authored from the PC.** If `disc-market-research` output exists, offer to drop it into 2.1/2.2/2.3. If not, insert the three subsection skeletons with a note: *populate via `disc-market-research` or BA-manual (presales)*.
- **Never fabricate** market size, segments, or competitors, and **never block** the SV on §2.

### §3 Business Requirements
- **3.1 Company Profile / 3.2 Business Problem** — from `01` Project Overview + PC §1.
- **3.3 Goals & Objectives** — from `01` Business Goal, as the SMART table (`# | Business Goal | Business Objective`).
- **3.4 Vision Statement** — AUTO-draft from `01`, labelled `[my inference]`/`[assumption to confirm]` for the BA to polish.
- **3.5 Business Requirements** — numbered **BR-01, BR-02…**, **business-level** (not functional FRs — those are not an SV artifact). Optionally split MVP vs later phases if `01`/PC frame it that way.
- **3.6 Desired Outcome + 3.6.1 Success Criteria** — from `01`; Success Criteria as the SMART table.
- **3.7 Business Risks** — **AUTO-draft** from `01` risks and PC/context, as the `Risk | Impact | Response Strategy` table, labelled. No discovery RMA exists — unknown risks stay `TBD`, not invented.

### §4 Stakeholder Analysis
- **4.1 Stakeholder Profiles** — a **pass-through reformat of `02_stakeholders.md`** into the SV table (`# | Name | Organization | Position | Role/Expectations/Concerns`). **This skill runs no stakeholder analysis of its own** — it reshapes the decided `02`. Gaps → `TBD`.

### §5 Solution Scope
- **5.1 Solution Approach** — AUTO-draft from PC §1/§4 + `03` (build / buy / hybrid), labelled.
- **5.2 User Roles Summary** — from PC §2 (roles + descriptions) and PC §12 (permission matrix → Key Actions), as `Name | Description | Key Actions`.
- **5.3 General Solution Models** — list the models discovery produced (`disc-diagram-build` flows, `disc-prototype-build` mockups, any BPMN/use-case). Discovery outputs are **specs/HTML, not hosted** → give one line per model plus a **link placeholder** for the BA's final hosted URL. **Never fabricate a link.**
- **5.4 Product Features and Releases** — from the **WBS Phase allocation** read per `disc-wbs-schema`, as the `Epic | Topic | MVP | Phase 2 | Future Phases` matrix; append a link to the full WBS. A direct transform of the WBS read — no re-prioritisation.
- **5.5.1 Solution Components** — from PC §6 Integrations (+ `03`), as `# | System Interface | Description`; the data-flow diagram is a **placeholder** for the BA/SA to insert.
- **5.5.2 Quality Attributes** — **transform the `nfrs` file** into the SV flat table `ID | Category | Quality Attribute | Description | Comments`. The `nfrs` file is Utility-Tree **scenario blocks**; here you **reshape**, not re-author: assign IDs by category (PERF-1, AVAI-1, USAB-1…), map the scenario's quality attribute to `Category` + `Quality Attribute`, and its **Response measure** to `Description`. Keep `TBD`s as `TBD`. This is a reshape of decided NFRs — it creates none.
- **5.6 Out of Scope** — from PC §7, as `# | User Story/Request | Description`.

### §6 Constraints
- **6.1 Assumptions / 6.2 Dependencies / 6.3 Limitations** — from PC (§8 parallel tracks + integrations), `01` constraints, and `03`. Keep each as the template's bullet list.
- **6.4 Solution Risks** — **AUTO-draft** from context, as `Risk Description | Response Strategy`. **6.4.1 RACI Matrix** — the SV's **standard fixed grid** (rows = risk/management areas; columns BA/PM/SA/PO) presented as-is for the BA to confirm; do not silently re-assign.
- **6.5 Licensing and Installation** — usually SA/tech; skeleton + `TBD` if no source in `03`.
- **6.6 Applicable Standards** — from `nfrs` Regulatory/compliance entries where present; else `TBD`.

### §7 and after — NOT authored
Technical Vision (§7) and all appendices are **SA-owned**. Do not write them. If the document needs a placeholder for continuity, insert a single line — *"§7 Technical Vision — to be completed by the Solution Architect"* — and nothing more.

## Consolidated file (optional)

After the sections are approved, **ask** whether to emit one consolidated `solution_vision.md` in outputs as a convenience. It is **not required** — the client document is canonical and every block already exists per-section. Produce it only on request.

## Rules

- **Downstream-only**: never a source; introduce no new decisions; a gap → `TBD` + Open Question, never a fabrication.
- **Never invent** stakeholder names/emails, market figures, risks, model links, or NFR values.
- **Never generate approvals or claim sign-off** — 1.6 stays Pending/skeleton.
- **Provenance labels in every content block**; the BA strips them before the client sees the section.
- A **PC / WBS / `nfrs` conflict** surfaced during assembly → **stop, flag, route** to the BA (and onward to the client where it is theirs) — do not resolve it inside the SV.
- **§7 and appendices are never authored here.**
- **Paraphrase**; do not lift large verbatim blocks from sources beyond what a table cell needs.

## MCP-unavailable / missing-source fallback

- **WBS unreadable** (Step-0 preflight fails) → follow the `disc-wbs-schema` fallback (CSV export or BA paste); mark 5.4 *"from manual input — verify against live WBS."*
- **A source file absent** → assemble the rest; mark that section's blocks `TBD`, name the missing file, and move on. Never fabricate to fill a missing source.

## Output

Paste-ready markdown, **one SV section per turn, in the SV's shape, with a review gate after each**. Optional consolidated `solution_vision.md` on request. The BA pastes into the SV document manually — no external write. The SV is the discovery hand-off deliverable and feeds nothing back upstream.

## What this skill does NOT do

- Does not author **§7 Technical Vision** or any appendix — SA-owned.
- Does not run **stakeholder analysis** (§4 is a pass-through from `02`), **author NFRs** (§5.5.2 reshapes `nfrs`), or run **market research** (§2 ← `disc-market-research`).
- Does not introduce new decisions, invent data, or write to any tool/Drive.
- Does not treat itself, or let anything treat it, as a source for another artifact.
