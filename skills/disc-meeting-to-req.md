---
name: disc-meeting-to-req
description: "Convert a discovery meeting transcript or cleaned session notes into structured decisions, action items, requirements, and open questions — implementation-agnostic. Use after a discovery or client session to produce the structured content that then commits to the Product Context (decisions and open questions) and feeds WBS story work."
---

# Discovery — Meeting to Requirements

## Purpose

Convert a meeting transcript or cleaned session notes into structured, validated content: decisions, action items, requirements, and open questions — implementation-agnostic. This is extraction, not creation; it never invents scope.

## When to invoke

- After a client call or stakeholder session.
- When raw notes need to become actionable artifacts.
- Before new scope is folded into the Product Context or the WBS.
- Best run on notes already cleaned by `disc-validate-notes`; if the input is messy, clean it there first.

## Input

Meeting transcript or session notes — any format (auto-transcript export, typed notes, pasted chat, voice-memo summary).

Context (read automatically from Project Knowledge):
- `01_project_context.md` — scope, glossary, domain terminology
- `02_stakeholders.md` — speaker identification, decision authority
- Product Context if present — to tell genuinely new items from ones already decided

## Process

1. Read the input.
2. Categorize content into four sections:
   - **Decisions** — explicit agreements made in the session. Note who made each (decision authority matters — a binding decision comes from someone empowered per `02_stakeholders.md`).
   - **Action Items** — tasks with owner and due date if mentioned.
   - **Requirements** — new functional or non-functional needs surfaced.
   - **Open Questions** — items raised but not resolved; route each Client / Internal.
3. For each Requirement:
   - Write as a numbered, plain-text statement.
   - **No implementation tag** (no FE/BE/Design split).
   - Use the project glossary.
   - Mark assumptions explicitly (`Assumption: …`) rather than presenting them as agreed.
4. Anti-hallucination:
   - Use only what was said in the source.
   - Do not infer scope or decisions not explicitly made.
   - Mark unclear items as `TODO` or `Assumption: [details]`.

## Output format

```markdown
### Decisions
1. [decision text] — decided by: [name / role]

### Action Items
| # | Item | Owner | Due |
|---|------|-------|-----|
| 1 | ... | ... | ... |

### Requirements
1. [requirement statement]

### Open Questions
1. [question] — routing: Client / Internal — flagged for follow-up
```

## Output destination

- Return as structured markdown in-chat by default.
- The BA decides what to commit and when. Nothing is written automatically.
- Downstream handoff: **Decisions and Open Questions** commit to the Product Context via `disc-pc-update` (Decisions Log §13 for any changed decision; Open Questions §9). **Requirements** feed `disc-wbs-story-build`. This skill does not commit them itself.

## Behavior

- Extraction, not creation. Factual, neutral tone.
- Anti-hallucination: strict — never invent decisions or requirements not in the source.

## What this skill does NOT do

- Does not decompose epics — use `disc-wbs-story-build`.
- Does not write AC — use `disc-bullet-ac`.
- Does not split requirements by implementation area.
- Does not commit to the Product Context — that is `disc-pc-update`, on the BA's explicit request.
