---
name: disc-elicitation-prep
description: "Generate a structured elicitation question list for project-wide, epic, or topic-specific scope using Project Knowledge and optional source artifacts. Use before discovery calls, client meetings, or working sessions to prevent blank-page paralysis and make sure nothing critical is missed."
---

# Discovery — Elicitation Prep

Generates a structured question list for a discovery call or workshop. Questions are grouped by topic and prioritized by what must be resolved before requirements can be written. Prevents blank-page paralysis before sessions and ensures nothing critical is missed.

Reads project context from Project Knowledge automatically. Add the session topic or focus area in the prompt — that is all that is needed.

---

## When to use

- Before any client meeting where requirements or scope will be discussed
- Before a working session where a feature is not yet clear enough
- When preparing for a discovery call on a new epic or integration
- When you have a blank page and don't know where to start

---

## Inputs

- Project context (auto-read from Project Knowledge: `01_project_context.md`, `02_stakeholders.md`, `03_tech_context.md`, and the Product Context if it exists)
- Scope parameter — **required**:
  - `project-wide` — general gaps across the full engagement
  - `epic:<epic-name>` — questions specific to one epic
  - `topic:<topic>` — focused topic (e.g. payments, auth)
- Optional source artifacts: RFP / brief section, presales WBS epic rows, a diagram

## Output filename pattern

`questions_list_<scope>.md` — e.g. `questions_list_project_wide.md`, `questions_list_epic_user_management.md`, `questions_list_topic_payments.md`.

---

## Steps

**Step 1 — Read project context**

Read `01_project_context.md`, `02_stakeholders.md`, `03_tech_context.md`, and the Product Context if present. Extract product domain and goals, scope boundaries (in / out), the stakeholders relevant to this session, and known technical constraints related to the topic.

**Step 2 — Determine session scope**

| Session type | Signals | Focus |
|---|---|---|
| Kickoff | "kickoff", no specific feature | Broad — product vision, goals, users, process, constraints |
| Feature / Epic discovery | Feature or epic name provided | Mid-level — functional scope, flows, roles, edge cases |
| Integration deep-dive | Integration name provided | Narrow — connection flow, API constraints, error handling, permissions |
| Topic clarification | A topic where scope is not yet clear | Narrow — gaps, edge cases, feasibility questions |

**Step 3 — Generate the question list**

Generate questions grouped by topic. For each group, list questions in priority order (most blocking first) and tag them: `[MUST]` (must be resolved before writing requirements), `[GOOD TO HAVE]` (nice-to-have clarification), `[ASYNC OK]` (can be resolved outside the meeting).

**Step 4 — Add session prep notes**

After the list, add a short section: what to prepare or send beforehand, who should be in the room (per `02_stakeholders.md`), and the expected output of the session.

---

## Writing each question

The list structure (grouping, priority, MUST tags) is set in Step 3; these rules govern how each individual question is worded.

- **One clear goal per question.** Before writing, know the answer you want — a specific detail or a broad picture — and how the client could realistically answer; write for that answer, not for the wording.
- **Strip all filler.** If a word can be removed without changing the meaning, remove it. Clear and short, grasped in one pass.
- **Show your stance.** The question must reveal where you stand — understanding / clarifying / challenging. Never ask as if you don't know what you already know, or the reverse.
- **Context first, only where needed.** If the question needs setup, give one or two sentences before the ask; if self-evident, just ask.
- **Think from every angle.** Consider the user, the admin, and the system, not only the user-facing view.
- **Propose a default or offer alternatives.** Where a sensible default or small option set exists, include it so the question becomes a fast yes/no or pick-one rather than an open field.
- **Atomic.** One decision per question, so each can be answered and closed independently.
- **Don't fear "dumb" questions.** Better to ask about the simple than to miss the complex — no one else validates this.

---

## Output

Markdown file named per pattern above. Structure:

```markdown
# Elicitation Questions — <scope>

## Context
<1-2 sentences — what scope these questions cover>

## Questions by topic

### <Topic 1>
1. <question>  [MUST]
2. <question>  [GOOD TO HAVE]

### <Topic 2>
...

## Session prep notes
- Prepare / send beforehand: <...>
- Who should attend: <...>
- Expected output: <...>

## Open assumptions
<things the skill assumed when generating — BA should validate>
```

---

## Kickoff mode — additional topics to cover

When session type is **kickoff**, always include question groups for: product vision and business goals; target users and roles; scope (in / explicitly out / TBD); existing system or legacy (greenfield or existing product?); key constraints (deadline, budget type, team, compliance); preferred ways of working (cadence, approvals, tools); open risks or concerns from the client side.

---

## Anti-hallucination rules

- Do not invent questions based on general product type — base them on what is actually known and actually unknown from Project Knowledge.
- Do not assume technical answers; if a constraint is unclear, write a question about it, not an assumption.
- Do not add questions already answered in Project Knowledge — check before generating.
- If Project Knowledge is missing or empty, flag it first:

> "Project Knowledge is not loaded or incomplete. Questions below are based on general BA practice for this type of session — review carefully before using."

---

## After generation

1. BA reviews and removes questions already answered.
2. Reorder or adjust priority based on meeting time available.
3. Share with the client beforehand if helpful (`MUST` questions only).
4. Use as a live checklist during the session.
5. After the session, feed outputs into `disc-validate-notes` (cleanup) then `disc-meeting-to-req` (extraction).
