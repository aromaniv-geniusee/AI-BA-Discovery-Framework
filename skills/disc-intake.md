---
name: disc-intake
description: "Triage incoming discovery material and route it to the next action. Use when the BA drops a mixed or unstructured batch — client message, transcript snippet, RFP section, presales WBS fragment, diagram, or a newly proposed feature — and asks what it means and what to do. Cross-check against the current Product Context and WBS, enumerate the items, state the real decision at stake per item, and route each without editing the Product Context or WBS."
---

# Discovery — Intake (triage & route)

Take an incoming batch and do two things: explain what each item actually is and the real decision behind it, then route it to the next action. This is the front-door step that *precedes* any commit — it grounds the material, makes sense of it, and decides where each piece goes. It is built for the discovery reality that the starting point is inconsistent: sometimes a full presales WBS plus an RFP, sometimes a handful of loose notes.

## What this skill is and is not

- **Is:** the analysis and routing step. Enumerate the items, ground each against the project, explain the decision, decide the next action.
- **Is not:** the PC commit step (→ `disc-pc-update`), the requirements-extraction step (→ `disc-meeting-to-req`), or the question-set builder (→ `disc-elicitation-prep`). It does not edit any artifact. It *may* draft a single client question inline when that is the route; assembling a full prioritized question set is the heavier `disc-elicitation-prep`.

## Inputs

One or more incoming items, any type and often mixed: client message, transcript snippet, RFP or brief section, presales WBS fragment, diagram / Miro export, newly proposed feature. Plus read access to the current Product Context (and the WBS where row numbers matter — read per the project's WBS-read method, MCP or CSV).

## Procedure

### 1. Enumerate

When the input is a batch, first split it into discrete items and number them. State the count and a one-line label per item before analysing — so the BA sees the whole surface and nothing is silently merged or dropped.

### 2. Ground each item

Targeted cross-check against the current PC (and WBS if relevant): what does the project already say on this exact point? Is this genuinely new, already covered, a change to something decided, or a contradiction? Do not analyse in a vacuum — a wrong read sends the item down the wrong route.

### 3. Explain the item and the decision behind it

State what the item actually is and the real problem or decision at stake — clearly and directly. Do not restate the item verbatim; surface the meaning the BA needs to act on. As long as the item requires and no longer.

### 4. Route to the next action

Classify each item and say it crisply as "this is X → do Y". The routes below are the common ones, not a closed set — if a different action fits better, use it and name it.

| Route | When | Hand-off |
| :---- | :---- | :---- |
| **Apply directly** | A decision already exists, or it is a pure narrowing / cleanup — no client input needed | Name the commit target: `disc-pc-update` (PC) · WBS edit · AC edit (`disc-bullet-ac`). Say whether it can be applied as-is or should still be confirmed with the client (the recurring "apply now vs bring to call" call). The commit itself happens via the matching skill on the BA's command. |
| **Ask the client** | The item raises something undecided or ambiguous; the client must rule | Draft the question — short, in English, addressed to the right contact per `02_stakeholders.md` (product owner vs technical owner). Flag it if it carries scope risk. |
| **Elaborate first** | It cannot be decided until it is modelled or broken down | Route to the right skill: `disc-elicitation-prep`, `disc-wbs-story-build`, or `disc-diagram-build`. Name what needs producing before a decision is possible. |
| **Bring to call** | Needs live discussion, multiple parties, or trade-offs | Note what to put on the agenda and why. |
| **No action** | Already covered, out of scope, or a note that only needs acknowledging | Say which, and that nothing changes. |

An item can carry more than one route (apply part directly, ask the client about the rest) — split it and route each part.

### 5. Stop at the route

End at the routing decision (plus the drafted question where the route is "ask the client"). Do not emit find/replace blocks, WBS edits, or requirement extractions. If the BA then says "закоміть у PC" or "витягни вимоги", that hands off to `disc-pc-update` or `disc-meeting-to-req`.

## What NOT to do

- Do not edit the Product Context or WBS from this skill — analyse and route only. (Drafting a single client question is allowed; producing PC edits is not.)
- Do not pick a winner on a real conflict — route it to the client and draft that question.
- Do not restate items verbatim instead of explaining them; lead with the decision at stake.
- Do not invent project facts — cross-check against the PC/WBS, or mark "not defined, needs confirming".

## Output

In-chat only: the enumerated item list, a clear explanation of each item and the decision behind it, and the route per item ("this is X → do Y", with apply-now vs confirm-with-client noted). Where the route is "ask the client", include the drafted question. No files, no commits.
