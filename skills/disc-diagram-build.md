---
name: disc-diagram-build
description: "Build stakeholder-review diagrams for a discovery flow, epic, process, or domain, derived strictly from the Product Context (PC silence → open question, never invented). Outputs paste-ready Mermaid where Mermaid fits (flowchart, state, sequence, ER, journey) and a structured draw.io spec for BPMN or use-case diagrams (Mermaid has no native BPMN/use-case). The BA pastes the code/spec into draw.io, Miro, or a Mermaid renderer manually — no diagramming-tool write. Use to generate a diagram from the PC; does not validate an existing design and does not edit the PC."
---

# Discovery — Diagram Build

Generate an editable, stakeholder-review diagram for a discovery target — a flow, epic, process, or domain — derived strictly from the Product Context. Output is **paste-ready**: Mermaid code where Mermaid fits, or a structured **draw.io spec** for BPMN/use-case. The BA pastes it into draw.io, Miro, or a Mermaid renderer manually. This skill does not write to any diagramming tool and does not edit requirements.

## When to use

- The BA wants a diagram built from the PC (a flow, a state machine, an integration sequence, the domain model, a business process, a use-case map).
- Not for checking an existing design against the PC — that is a validation task, not this one.

## Inputs (per run)

- **Target** — named by the BA: a flow, epic, process, domain, or topic to diagram.
- **Diagram type** — named by the BA, or recommended by this skill from the target (see routing).
- **Product Context (PC)** — the single source of truth for content: roles (canonical names), permissions, states, transitions, business rules, entities, integrations, scope, glossary. Read access required.
- **`03_tech_context.md`** — additional content source **only** for technical diagrams (integration sequence, system-level ER), where the interaction/architecture detail lives there rather than the PC. Mark `candidate` items honestly.

Nothing else is a content source. Do not invent from general product knowledge, and do not draw from a WBS Topic or a roadmap name alone.

## Diagram-type routing

Pick the type that fits the target; recommend one if the BA did not name it.

| Target shape | Type | Output |
|---|---|---|
| User/process flow with steps, branches, denials | **flowchart** | Mermaid `flowchart` |
| An entity/user lifecycle — states + transitions | **state** | Mermaid `stateDiagram-v2` |
| An interaction across actors/systems over time (esp. integrations) | **sequence** | Mermaid `sequenceDiagram` |
| The domain model — entities + relationships | **ER** | Mermaid `erDiagram` |
| A user's end-to-end experience with sentiment | **journey** | Mermaid `journey` |
| A business process with swimlanes/pools and BPMN gateways/events | **BPMN** | **draw.io spec** (Mermaid has no native BPMN) |
| Actors and their use cases (associations, include/extend) | **use-case** | **draw.io spec** (Mermaid has no native use-case) |

One diagram per target — never one giant connected diagram, never unrelated flows joined. Each diagram independently readable.

## Procedure

### Step 1 — Derive the target from the PC (PC-first)

Before drawing, build the content from the PC:
- Primary actor + supporting/escalation actor(s), **canonical role names only** (no simplified/client-facing renames).
- Role scope / permission boundary (own org / own group / assigned records / platform-wide / requires approval / escalate) — shown explicitly for scoped roles.
- Entry point(s) and exit/outcome(s); states and transitions; decision points and the business rules governing branches.
- For each role in a multi-role flow: what happens without permission (view-only / cannot edit / must escalate / system blocks). Do not assume the highest role's permissions apply to all.
- **PC silence is never filled.** Anything the PC does not state → an **open question** attached to the relevant step. Never invent screens, permissions, states, roles, or rules. If two PC statements conflict, do not resolve silently → a note flagging it.

### Step 2 — Brief the diagram in text, then wait for "ok"

Present the target as text before drawing, and wait for the BA's "ok". Cite the PC sections used.
- Roles — actors + scope.
- Start and end — the true entry and terminal outcome(s).
- Happy path — step by step, brief.
- Alternative scenarios — errors, denials, expired/blocked states, branches.
- Open questions — what the PC does not settle, written as real client-ready questions.

One target at a time (brief → ok → generate → next), unless the BA asks to batch briefs.

### Step 3 — Generate paste-ready output

Emit the Mermaid code block (or draw.io spec) per the rules below. One fenced block per diagram, so the BA can copy it cleanly. State where it pastes (draw.io imports Mermaid; Miro via its Mermaid/diagram import; or any Mermaid renderer). Optionally, the BA can validate/render the Mermaid before pasting — offer, do not require.

### Step 4 — Reverse-gap pass against the PC

After the diagram(s) are built, run the check the other way: re-read the PC sections for this target, list the concrete rules/behaviours they state, and check whether each landed in a built diagram (a step, branch, state, note). Report the misses as a short **Coverage gaps** list, each with its PC reference and where it would belong. Do not silently patch or invent — the BA decides what to patch, raise as an OQ, or route elsewhere. Keep it to genuine misses; a rule that clearly belongs to another target is a boundary note, not a gap.

## Mermaid diagram rules (flowchart — the common case)

Build stakeholder-review flowcharts, not technical architecture diagrams. A reviewer should follow each happy path in 5–10 seconds and be able to comment on a branch without untangling arrows.

**Title + metadata:** wrap the flow in a subgraph whose label is the title (`<Target> · <Flow Name>`). Directly under it, one single-line metadata note — `Actor: … -- Scope: …` — styled `:::scope`.

**Shapes:**
- Start event → plain circle `id(("Start …"))`; End event → filled circle styled `:::endev`. Circles are for the true entry and terminal events only; intermediate states/actions are rectangles, never circles. Do not use the stadium/pill for start/end.
- User action → rectangle `["…"]`. One step = one action. Two actions joined with `+`/`and` → split into a chain `A --> B`. Do not split a field/attribute list into separate steps — it stays one node.
- System action → rectangle, `System:` prefix.
- Decision / gateway → diamond phrased as a question `{"…?"}`; branch labels are the answers (Yes / No / Valid / Expired).
- Permission denial / blocked → a normal branch node; no special colour.
- Destructive action → a `Confirm …?` decision with an explicit terminal Cancel (no-change) branch.
- Cross-role handoff → rectangle `Handoff to [Role]`. External step → rectangle `Off-platform: …`.

**Colours (paste this classDef into every flowchart):** two note colours only — red = open questions; yellow = scope/explanatory notes. Green fills only the terminal end event.
```
classDef oq fill:#FBE0E0,stroke:#D14343,color:#7A1F1F;
classDef scope fill:#FFF4CC,stroke:#E6B800,color:#665600;
classDef endev fill:#CDEBD6,stroke:#3C7D4F,color:#1E4D2B;
```

**Layout:** direction `LR` by default; happy path left-to-right, entry top-left, outcomes bottom-right. Alternative/error/denial branches sit **below** the related happy-path step, each a near-independent mini-flow. Avoid long diagonal/crossing connectors and central hub nodes — duplicate a block into a branch rather than draw many arrows to it. Keep whitespace around branches for comments; split into subflows if too large.

**Notes discipline:** short labels inside step blocks (1–2 lines); explanations/assumptions/OQs go in side notes beside the step, never between two main-flow steps. **OQ notes are written as real, client-ready questions** — give the actual question and, where useful, why it matters and the options at stake. Do not write meta-phrases like "PC is silent" / "needs validation" — the artifact is client-facing; just ask the question. When an OQ affects another target, say so inside it. Include only notes that affect logic; business-readable language.

**Syntax:** put all shape/edge text in quotes (`["Text"]`, `-->|"Edge"|`). No emojis, no literal `\n`, do not use the word `end` in classNames.

## Rules for the other Mermaid types

- **state / sequence / ER / journey** use their native Mermaid syntax. Carry over the discipline that fits: canonical names; OQ notes as real client-ready questions (as `note` elements where the type supports them, else a trailing Open Questions list under the block); PC silence → OQ; conflict → flagged note.
- **sequence** for integrations: name each participant with its canonical/system name from PC §6 / `03_tech_context`; show direction and the key messages; mark a `candidate` integration as such.
- **ER** for the domain model: entities and relationships from PC §3 only; attributes only where the PC states them; unknown cardinality → OQ, not a guess.

## draw.io spec (BPMN / use-case — Mermaid can't do these natively)

Output a structured spec the BA lays out in draw.io (or imports). Keep it faithful to the PC; PC silence → OQ line.

**BPMN spec format:**
```
# BPMN — <process name>   (source: PC §…)
Pools / Lanes:
- <Lane = role, canonical name>
Start event: <…>
Tasks (per lane, in order):
- [<Lane>] <task>  (type: user / service / manual)
Gateways:
- <Gateway name> {question} → <branch>: <target> | <branch>: <target>
End events: <…>
Open questions:
- <client-ready question>
```

**Use-case spec format:**
```
# Use-case — <system / scope>   (source: PC §…)
Actors:
- <Actor = canonical role>  (primary / secondary)
Use cases:
- UC-<n>: <use case>  — actor(s): <…>
Relationships:
- <UC-a> «include» <UC-b>
- <UC-a> «extend» <UC-b>
Open questions:
- <client-ready question>
```

## Output

One paste-ready fenced block per diagram (Mermaid) or one spec block (draw.io), each preceded by its brief and followed by its open questions. Multiple targets → multiple separate blocks, never merged. No diagramming-tool write — the BA pastes manually.

## Anti-hallucination

- Content comes from the PC only (plus `03_tech_context` for technical diagrams). PC silence → OQ. Conflict → flagged note. Never invent screens, permissions, states, roles, entities, or rules.
- Canonical role and glossary names only — never rename/merge/reclassify.
- Diagram target and type come from the BA (or a recommended type from the target) — never infer a different scope or top up content the PC doesn't state.

## What this skill does NOT do

- Does not write to Figma / draw.io / Miro — output is paste-ready code/spec.
- Does not validate an existing design against the PC.
- Does not edit the Product Context.
