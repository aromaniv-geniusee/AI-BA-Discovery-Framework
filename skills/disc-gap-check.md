---
name: disc-gap-check
description: "Run a discovery-wide quality scan across the Product Context, the WBS, the NFRs file, and the Solution Vision (where it exists) to find gaps, inconsistencies, duplicates, and orphaned items before handoff. Review-only: reports findings and routes each fix to the skill that owns that artifact (disc-pc-update, disc-wbs-story-build, disc-bullet-ac, disc-nfr); never edits, and never resolves a real conflict silently. Reads the WBS per disc-wbs-schema."
---

# Discovery — Gap Check (health scan)

Discovery-wide quality scan across the requirements landscape — the Product Context, the WBS, the NFRs file, and the Solution Vision where it exists. Identifies gaps, inconsistencies, duplicates, and orphaned items, and gives the BA a single health report before handoff.

This is a **project-level scan, not a single-story review**, and it is **review-only**: it reports and routes, it never edits an artifact and never silently resolves a conflict.

## Sources (and their standing)

- **Product Context (`product_context.md`, §1–§13)** — the authoritative source of decided state. Read as a whole.
- **WBS** — read per **`disc-wbs-schema`** (Path B, Step-0 preflight, header-by-name). The scope baseline.
- **NFRs file** — owned by `disc-nfr`. In scope for coverage/answered-state checks.
- **Solution Vision (SV)** — **downstream and derived**; scanned only for **alignment to the PC/WBS**, never treated as a source of truth. Skip if it does not exist yet.

Precedence for every finding: the **PC is canonical**; the WBS is expected to be aligned to it; the SV must not diverge from either. A raw WBS AC or an SV line never overrides a recorded PC decision — that is a conflict to surface, not to resolve here.

## When to invoke

Before discovery handoff, or at any checkpoint where the BA wants a health read of the requirements landscape.

## Input

- Scope (optional): an epic name, functional domain, or "project-wide" (default).
- Reads the PC and NFRs file from Project Knowledge; the WBS per `disc-wbs-schema`; the SV where the BA points to it.

## Process

### Step 0 — preflight & load

Run the `disc-wbs-schema` Step-0 preflight for the WBS read (Code Execution + Drive; stop and ask if missing — never fall back silently). Load the PC, NFRs, and SV (if any). If any source returns partial data, **flag it explicitly** and scan what is present — do not fill gaps with assumptions.

### Step 1 — Gap analysis

For each functional domain / epic in scope:
- Is every PC §4 functional domain covered by at least one WBS story?
- Is every WBS epic traceable to a PC domain (not scope that appears only in the WBS)?
- Do stories cover — at discovery depth — the happy path plus at least one alternative and one key error state?
- Do WBS rows have AC (`Acceptance Criteria` cell populated), or are they empty and still owed to `disc-bullet-ac`?
- Are PC §9 Open Questions still open that would block handoff or a reliable estimate?
- Are NFR categories in the NFRs file left unanswered (`Client's answer` empty) where an answer is load-bearing?

Flag each gap with: location + description + severity (High / Medium / Low).

### Step 2 — Inconsistency check

- WBS raw AC ↔ PC decision divergence (field optionality, defaults, added/removed steps, scope) — PC is the resolved state.
- A `Role` used in the WBS that is not in the PC role model (§2), or a same-action/different-actor mismatch.
- A PC §7 "out of scope" item that still appears as a WBS story (or vice-versa).
- Phase/scope described in the PC not reflected in the WBS, or a WBS Phase value inconsistent with a PC decision.
- SV (where it exists) contradicting the PC or WBS.

Flag each with: source A vs source B + description. Pure **renames** (a role/label changed) are silent mapping, not an inconsistency — map to the current term and move on.

### Step 3 — Duplicate detection

- The same requirement stated in two or more PC sections.
- Two WBS rows covering the same functional scope.
- The same AC item repeated across multiple stories (should be one canonical story referenced by the others).

Flag with: the duplicate items + recommended resolution (merge / remove / keep one as primary).

### Step 4 — Orphan detection

| Orphan type | Check |
|---|---|
| Story without a domain | WBS story not traceable to any PC §4 domain |
| Domain without a story | PC §4 domain (or §3 entity) with no WBS story |
| Story without AC | WBS row exists, `Acceptance Criteria` cell empty |
| Open question without routing | PC §9 item with no Client/Internal routing or no owner |
| Undefined glossary term | A term used across sources but absent from PC §10 |
| SV section without backing | An SV section with no PC/WBS support behind it |

### Step 5 — Deliver report

```
## Discovery Requirements Health Report — [scope]
Scan date: [date]
Sources: Product Context · WBS (disc-wbs-schema) · NFRs · Solution Vision [if present]

### Summary
| Category | Count | High severity |
|---|---|---|
| Gaps | N | N |
| Inconsistencies | N | N |
| Duplicates | N | N |
| Orphans | N | — |

### Gaps
- [Domain / Epic]: [missing coverage] — Severity: High/Medium/Low — Fix via: [disc-… skill]

### Inconsistencies
- PC [§n] says [X] / WBS [row / epic] says [Y] — Action: [surface to client / align WBS to PC / raise open question] — Fix via: [disc-… skill]

### Duplicates
- [Item A] and [Item B] cover the same scope — Recommended: [merge / keep one] — Fix via: [disc-… skill]

### Orphans
- [WBS row]: no PC domain link
- [WBS row]: AC empty
- [PC §9 item]: no routing

### Recommended next steps
1. [Priority action] — owner skill: [disc-pc-update / disc-wbs-story-build / disc-bullet-ac / disc-nfr]
2. [Priority action] — owner skill: […]
```

## Routing fixes (review-only — this skill never edits)

Every finding names the skill that owns the fix; the fix happens there, on the BA's explicit command:
- PC content (decisions, open questions, glossary, scope) → **`disc-pc-update`**.
- Missing/duplicate stories, decomposition → **`disc-wbs-story-build`**.
- Missing/inconsistent AC → **`disc-bullet-ac`**.
- NFR coverage/answers → **`disc-nfr`**.
- SV misalignment → regenerate the affected SV section via **`disc-solution-vision`**.

**Never resolve a real conflict silently.** Surface it, stop, route it to the BA (and onward to the client where the decision is theirs).

## Rules

- **Read-only.** Do not modify the PC, WBS, NFRs, or SV during the scan.
- Do not infer requirements not explicitly stated in the sources; ambiguous → `TBD`, do not interpret.
- If a source returns partial data, flag it — do not fill gaps with assumptions.
- Severity: **High** = blocks handoff or creates rework/estimation risk · **Medium** = should fix before handoff · **Low** = cleanup, not urgent.

## MCP-unavailable fallback

If the WBS cannot be read via MCP (preflight fails and the BA cannot enable Code Execution):
1. Use the CSV export in Project Knowledge per `disc-wbs-schema`.
2. Run the scan from the provided content.
3. Note in the report: "Manual input — MCP not used. Verify against live data."

## What this skill does NOT do

- Does not edit any artifact — findings only.
- Does not do per-story AC depth review beyond coverage presence — that is `disc-bullet-ac`.
- Does not commit anything to the PC — that is `disc-pc-update`.
