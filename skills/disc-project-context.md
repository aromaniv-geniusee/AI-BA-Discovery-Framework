---
name: disc-project-context
description: "Generate `01_project_context.md` for a Discovery engagement from kickoff notes, PM messages, presales WBS fragments, RFPs, client briefs, onboarding notes, transcripts, or free-text project descriptions. Use at the start of a discovery project to capture project overview, business goal, scope, users, constraints, risks, and discovery identifiers. Ask up to 3 targeted questions only when business goal or high-level scope is unclear."
---

# Discovery — Project Context

Generate `01_project_context.md` as the foundational project-context file for Project Knowledge. Use it as the baseline reference for future BA work during Discovery. It is the project-context file, not a Solution Vision — the SV is a discovery *output* assembled later and must not be treated as an input here.

## Inputs

Any combination, and the starting point is often inconsistent:
- Kickoff notes, PM Slack messages, free-text project description
- RFPs, client briefs, presales notes
- Presales WBS (Geniusee template or client-provided — epic/scope structure)
- Architectural or flow diagrams (PDF, screenshots — uploaded as images), Miro exports

There is no single guaranteed input. If the incoming material is mixed or unstructured, triage it with `disc-intake` first, then build the project context from what it surfaces.

## Procedure

### 1. Parse the input

Extract the following if present:

| Field | Look for |
|---|---|
| Client & product | Company name, product, domain |
| Engagement model | Discovery, Presales → Discovery, Pilot, T&M/Fixed if already framed |
| Business goal | Why the client wants this, expected outcome, success criteria |
| Scope | In-scope items, exclusions, open areas (high level, not story-level) |
| Users & roles | User groups, access levels, internal/external roles |
| Constraints | Deadline, budget model, dependencies, resourcing gaps |
| Risks | Explicit risks or obvious red flags from the source |
| Discovery identifiers | Google Drive folder(s), diagramming tool, Figma, Slack, repo/staging if any |

### 2. Identify critical gaps

Before generating, verify that:
- Business Goal is clear enough to write factually
- High-level Scope exists

If either is missing or too vague, ask a maximum of 3 targeted questions.

Example:

> I have enough to generate the project context. Two things are unclear:
> 1. Is this a paid Discovery or presales-funded?
> 2. Are there any explicit out-of-scope items mentioned?
> If you do not know yet, I will mark them as TBD.

Do not invent missing facts.

### 3. Generate the document

Use the template below. Mark unknown values as `TBD`. Keep the document concise, factual, and usable in future discovery work.

```markdown
# Project Context — [Project Name]
_Last updated: [date]_

## Project Overview
[2–4 sentences: what the product is, who the client is,
what domain, engagement model (Discovery / Pilot / etc.)]

## Business Goal
[WHY — what the client wants to achieve.
What does success look like for them at discovery close.]

## Scope
**In:** [features, modules, integrations in scope for discovery]
**Out:** [explicitly excluded — design, infra, specific platforms, etc.]
**TBD:** [open scope questions not yet resolved]

## Users & Roles
[Optional — fill if known at kickoff; detailed model lives in 02_stakeholders.md]
- [Role name] — [what they do / access level]
- [Role name] — [what they do / access level]

## Key Constraints
- [Deadline or milestone / discovery timebox]
- [Budget type or allocation constraint]
- [Resource gaps, dependencies, external blockers]

## Risks
| Risk | Impact | Notes |
|---|---|---|
| [Risk description] | High / Med / Low | [Mitigation or open question] |

## Discovery Identifiers
| Tool | Value |
|---|---|
| Google Drive | [working-docs folder link(s)] |
| Diagramming | [draw.io / Miro] |
| Figma | [link, if any] |
| Slack | [channel name(s)] |
| Other | [e.g. GitHub repo, Staging URL] |
```

### 4. Add BA review note

After the generated document, append:

```markdown
Review before adding to Project Knowledge:
- [ ] TBD fields to resolve
- [ ] Scope Out — confirm with client
- [ ] Discovery identifiers — fill in if missing
```

## Rules

- If a field is not present in the input, write `TBD`.
- Do not invent engagement model, risks, users, or identifiers.
- Do not use generic filler text.
- Do not document the detailed stakeholder model or tech stack here — those belong in `disc-stakeholders` and `disc-tech-context` respectively. Communication cadence is not captured as a separate discovery artifact; do not document it here.
- Do not treat a Solution Vision as an input — in Discovery it does not exist yet.

## Handling large inputs

- RFPs and presales decks can be long. Do NOT copy the full text into the project context.
- Extract ONLY what belongs in the project context:
  - Product context (1–2 paragraphs)
  - Business goals (3–7 bullets)
  - Scope highlights (in/out — high level, NOT story-level)
  - Key stakeholders (names, roles)
  - Glossary anchors (5–10 key terms)
  - Constraints and risks
- Presales WBS: extract the epic list and phase structure, not stories.
- If a section is ambiguous or contradictory — flag with `[TBD — clarify with PM/client]`. Do not paraphrase to hide uncertainty.

## Output

Save as `01_project_context.md`. The BA saves it into Project Knowledge manually — no external write.

## Update trigger

Regenerate or revise this file when:
- scope changes significantly
- a new stream appears
- a major constraint changes
