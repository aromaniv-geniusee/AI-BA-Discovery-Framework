---
name: disc-pc-update
description: "Produce OLD→NEW find/replace edits that bring an existing discovery `product_context.md` to its current decided state from one or more new sources, maintaining §9 Open Questions and §13 Decisions Log. Trigger only when the BA explicitly asks for find/replace or Product Context edits. Purely updates the Product Context; does not write stories/AC, and does not touch the separate `nfrs` file."
---

# **Discovery — Product Context Update (find/replace)**

Produce targeted OLD→NEW edits that bring an existing `product_context.md` to its current decided state. The BA applies them by find/replace and re-saves the updated file. The PC holds the *fixed decision* — the only history it carries is the §13 Decisions Log line for a changed decision. No external write; all output is paste-ready.

## **Trigger gate — read first**

Only produce edits when the BA explicitly requests find/replace (or an equivalent commit instruction: "зроби правки в PC", "actualize the PC", "закоміть рішення"). If a source merely arrived and the BA has not asked for edits, do not output OLD→NEW blocks — at that point the source is for analysis and discussion, not editing.

This gate exists because editing is a deliberate commit step, separate from thinking through a source. The BA wants to analyze and settle a decision first, then explicitly ask for the edits once it is final. When unsure whether the BA wants edits yet, ask — do not assume.

## **When to use vs disc-pc-init**

* **disc-pc-update (this skill):** the file exists; the BA asks to reflect one or more sources as edits.
* **disc-pc-init:** no file yet; build the §1–§13 skeleton and first draft.

## **Inputs**

* The current `product_context.md` — read it in full first. It is large; never edit from memory.
* One or more new sources, any type: transcript, call decisions, client answers, RFP, WBS delta, approved-feature list. Multiple sources can be processed in one pass. What matters is that the effect of every source on the PC is fully analyzed, not the number of sources.

## **Procedure**

### **1. Read both, then cross-check**

Read every new source and the current PC in full. For each claim a source makes, locate what the PC currently says on that exact point. Aim for complete coverage — every source claim is checked against the PC. Incomplete cross-check is how reversals slip through.

### **2. Triage every change into one category**

| Category | Meaning | Action |
| ----- | ----- | ----- |
| **Reversal** | Source decides something the PC already states *differently* | Replace the old statement AND append a §13 Decisions Log line. Never leave both versions. Most dangerous — check for these first. |
| **New** | Source adds something the PC is silent on | Add into the right section, with citation. If it answers an existing §9 item, close that item. |
| **Conflict** | Source contradicts the PC and no authority resolves it | Do not pick a winner and do not invent an edit. Stop and resolve with the BA in-pass (step 3). |
| **Already covered** | Source restates what the PC already has | No edit; treat as confirmed. |

Anything a source raises but does not resolve — an ambiguity, a silence, a new question — goes into §9 Open Questions (routed Client / Internal, sorted most-blocking-first), not into a guessed edit.

### **3. Clarify the real decision, and resolve conflicts in-pass**

For Reversals and Conflicts, state the actual decision at stake in one line — what changed and why it matters — before the edit, so it is reviewed rather than rubber-stamped.

**Conflicts stop the pass at the point they arise.** When a Conflict surfaces, do not defer it and do not continue emitting edits past it. Clarify the conflict, ask the BA which way to go, wait for the decision, then continue from where it stopped.

Precedence when claims clash:
* An explicit client decision in a new source supersedes an older PC statement → Reversal, replace, and log to §13.
* A recorded PC client-decision is not overridden by a raw WBS acceptance criterion or by silence — surface the divergence as a Conflict; the working assumption is that the PC is canonical and the WBS is aligned to it.
* Two equal-authority sources that disagree with no resolution → Conflict, resolve in-pass.

### **4. Produce the edits**

For each Reversal and New item, output an OLD→NEW block. Present OLD and NEW as two **separate fenced code blocks**, each under its own `OLD:` / `NEW:` label — never inline, never merged into one block — so the BA copies each verbatim without the content rendering as markdown. Precede each block with its category tag + location (the header line the BA reviews before applying).

* **OLD:** the exact current PC text to find (verbatim, enough surrounding text to be unique for a single find/replace).
* **NEW:** the exact replacement.

For each Reversal, additionally output an append block for §13 Decisions Log: `<date> · <old → new> · <why> · <source>`.

Point-edits by default; full-section replacement only when more than roughly half a section changes. Write the *final decided state* — never "previously X, now Y" inside §1–§12. Keep inline citations on every new or changed statement (`(Sx HH:MM:SS)` / `(Sx §n)` / `(WBS Rn)`). Tag each block with its category so the BA sees what they are applying.

## **What NOT to do**

* Do not produce edits before the BA explicitly asks for find/replace.
* Do not rewrite or re-emit the whole file — only targeted OLD→NEW blocks.
* Do not silently resolve a real conflict — stop and ask in-pass.
* Do not write chronology into §1–§12 — history lives only in §13, one line per changed decision.
* Do not write stories/AC (that is `disc-bullet-ac` / `disc-wbs-story-build`) and do not touch the separate `nfrs` file (that is `disc-nfr`).

## **Output**

Category-tagged OLD→NEW find/replace blocks plus any §13 append lines, ready to apply manually. No file regeneration. After the BA applies them and re-saves the updated PC, verify the edits landed if asked.
