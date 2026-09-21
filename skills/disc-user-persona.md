---
name: disc-user-persona
description: "Build discovery user personas (3–5 primary archetypes) for a scope, derived strictly from the Product Context (§1 users, §2 roles/attributes/states, §4 capabilities) and optional disc-market-research §2.1 segments — PC silence → labelled inference + Open Question, never invented as fact. Outputs a paste-ready personas_<scope>.md (one card per archetype: summary, mapped PC role(s), goals, needs, pains, behaviours/context, representative scenario, provenance + OQs) plus paste-ready Mermaid flowchart cards (one block per persona) the BA pastes into Miro. Situational Direct skill call; stands alone and optionally feeds the market-research persona canvas / SV §2.1, never blocks either. No fabricated names/photos/quotes; canonical role and glossary terms only; reads the PC, never edits it; no external/tool write."
---

# Discovery — User Persona

Build **user personas** for a discovery scope — 3–5 primary archetypes that make the product's users concrete for elicitation, prioritisation, and client alignment. Output is **paste-ready**: a `personas_<scope>.md` file plus **Mermaid flowchart cards** the BA pastes into Miro. This skill reads the Product Context and never edits it, and it writes to no tool.

## Grounding discipline — read first

A persona is more inference-prone than the rest of the set — goals, needs, frustrations, and behaviours are rarely stated verbatim in the PC. That collides head-on with the framework's no-invention rule, so the discipline **reshapes rather than relaxes**:

- Every persona attribute is either **grounded in a source** (PC §1 users, §2 roles/attributes/states, §4 capabilities; or `disc-market-research` §2.1 segments) **or** carries a provenance label — `[my inference]` / `[assumption to confirm]` — and a **matching Open Question**. Nothing unsourced is ever presented as decided fact.
- **No fabricated flourishes.** No invented stock name, photo, age, or verbatim quote — these read as "decided" when they are not and undermine the card. A persona is named by its **archetype/role**, not a fictional person.
- **Personas introduce no new decided scope.** An inferred need or pain is an Open Question, never a PC edit from this skill. Personas are a **derived view**, not a source of truth for another artifact.
- Canonical role and glossary terms only — never rename, merge, or reclassify a PC role.

## When to use

Situational — a Direct skill call, no mode. Use when concrete user archetypes would sharpen elicitation, prioritisation, or a client conversation:
- before an epic or session where "who is this really for" is unsettled;
- when the client asks for personas as a discovery artifact;
- to fill the **Persona / Problem Statement canvas** slot referenced by `disc-market-research` §2.1, or to give `disc-solution-vision` §2.1 an illustrative asset.

Skip it when PC §2 roles already make the users clear enough, or personas are out of scope for the engagement.

## Inputs (per run)

- **Scope** — **required**, named by the BA:
  - `project-wide` — archetypes across the whole product;
  - `role:<name>` — personas for one PC role;
  - `segment:<name>` — personas for one market segment (requires `disc-market-research` §2.1).
- **Product Context (PC)** — the primary content source: §1 (users / value proposition), §2 (role hierarchy, user attributes, states/transitions), §4 (capabilities per role). Read access required.
- **`disc-market-research` §2.1 output** — optional; where it exists, its Customer Segments (profile, behaviour & needs, decision-making) seed persona demographics and context. Not fetched here — consumed if already produced.

Nothing else is a content source. Do not invent from general product knowledge, and do not draw persona traits from a WBS Topic or a roadmap name alone.

## Procedure

### Step 1 — Derive the archetype set from the sources

Before writing personas:
- Read PC §2 roles and §1 users; where a `segment:` scope is set, read `disc-market-research` §2.1.
- Cluster into **3–5 primary archetypes** — do **not** produce one persona per PC role mechanically. Where roles share goals and behaviour, merge them into one archetype naming the roles it covers; where a role splits into distinct user types, separate them. Note any role/segment **deferred** (secondary, not given its own primary persona) for the reverse-gap pass.
- For each archetype, gather what the sources state and mark the gaps — every field either has a source or becomes a labelled inference + Open Question.

### Step 2 — Brief the personas in text, then wait for "ok"

Present the archetype set as text before writing the file, and wait for the BA's "ok". Cite the PC sections / market-research used.
- The **archetype list** — name, the PC role(s) / segment each maps to, and a one-line summary.
- Which roles/segments were **clustered** into which archetype, and which were **deferred**.
- The **material inferences** you will make (the load-bearing `[my inference]` / `[assumption to confirm]` attributes), so the BA sees them before they land.
- The **Open Questions** the personas will carry, written as real client-ready questions.

One scope at a time (brief → ok → generate), unless the BA asks to batch.

### Step 3 — Emit `personas_<scope>.md`

On "ok", produce the paste-ready `.md` — one card per persona, per the field set below, every attribute either sourced (with an inline `(PC §n)` / `[Sn]` reference) or labelled `[my inference]` / `[assumption to confirm]`. Precede the file with a one-line coverage note (personas produced · roles/segments deferred).

### Step 4 — Emit Mermaid persona cards

After the `.md`, emit **one fenced Mermaid `flowchart` block per persona** — a condensed, scannable derived view of the card (not a 1:1 dump of the `.md`). State that it pastes into Miro (Mermaid import), draw.io, or any Mermaid renderer. See the Mermaid rules below.

### Step 5 — Reverse-gap pass against the PC

After the personas are built, run the check the other way: re-read PC §2 roles and §4 capabilities for this scope and check whether **each role is represented** by a persona or **deliberately deferred**. Report the misses as a short **Coverage gaps** list — a role with real distinct behaviour and no persona is a gap; a role clustered by design is a note, not a gap. Do not silently patch — the BA decides what to add or defer.

## Field set (per persona card in the `.md`)

| Field | Content | Grounding |
|---|---|---|
| **Archetype name + summary** | Role-based name (not a fictional person) + one line on who they are | PC §1/§2 |
| **Maps to PC role(s) / segment** | The canonical role(s) and any market segment this archetype covers | PC §2 / market-research §2.1 |
| **Goals / motivations** | What they want to achieve with the product | PC §4 capabilities / §1 value; else labelled |
| **Needs** | What they need from the product to meet those goals | PC §4; else labelled |
| **Pain points** | Frustrations the product should relieve | market-research / PC; usually labelled |
| **Behaviours & context** | How they work, frequency, environment, **tech comfort** | market-research §2.1; else labelled |
| **Representative scenario** | One short paragraph — a realistic use situation (stays in the `.md`, not in the Mermaid card) | derived; labelled where inferred |
| **Provenance + Open Questions** | Per-persona `[client decision]` / `[my inference]` / `[assumption to confirm]` and the client-ready OQs for its gaps | — |

## Mermaid rules (persona flowchart cards)

Follow the **Mermaid syntax rules in `disc-diagram-build`** (all shape/edge text in quotes; no emojis; no literal `\n`; do not use the word `end` in classNames). This skill does **not** restate them — it carries only the persona-specific layout and provenance classDef below.

- **One `flowchart` block per persona** — never one giant diagram joining unrelated personas. Each card independently readable and paste-able.
- **Card = a titled subgraph** whose label is `Persona · <Archetype name>`, `direction TB` inside, laid out as a short chain of labelled attribute nodes: Summary → Goals → Needs → Pains → Key behaviours. Keep it scannable — the representative scenario and full field detail stay in the `.md`, not on the card.
- **Provenance is shown by node colour** (the colour carries what the `.md`'s labels carry in text — do not also print `[my inference]` inside the node text):
  - `:::sourced` — grounded directly in PC / market-research;
  - `:::inferred` — `[my inference]`;
  - `:::confirm` — `[assumption to confirm]`;
  - `:::oq` — Open Questions, as real client-ready questions in a note node off the card (red, consistent with `disc-diagram-build`).
- **Paste this classDef into every persona flowchart:**
```
classDef sourced fill:#CDEBD6,stroke:#3C7D4F,color:#1E4D2B;
classDef inferred fill:#FFF4CC,stroke:#E6B800,color:#665600;
classDef confirm fill:#FDE9D0,stroke:#D98A2B,color:#7A4A12;
classDef oq fill:#FBE0E0,stroke:#D14343,color:#7A1F1F;
```

## Output format

Precede with the one-line coverage note. Then the `.md`, then one Mermaid block per persona, then the Open Questions.

```markdown
# User Personas — <scope>
_Coverage: <N> personas · roles/segments deferred: <list or none> · Sources: PC §1/§2/§4 [+ market-research §2.1] · Date: <YYYY-MM-DD>_

## Persona 1 — <Archetype name>
**Summary:** <one line>  (PC §1)
**Maps to:** <canonical role(s)> [ / <segment>]
**Goals / motivations:**
- <goal>  (PC §4)
- <goal>  [my inference]
**Needs:**
- <need>  (PC §4)
**Pain points:**
- <pain>  [assumption to confirm]
**Behaviours & context:**
- <behaviour / frequency / environment / tech comfort>  [Sn] / [my inference]
**Representative scenario:**
<one short paragraph — realistic use situation; labelled where inferred>

**Open Questions**
- <client-ready question> — routing: Client / Internal

---
[Persona 2 …]

## Coverage gaps
- <PC role with distinct behaviour and no persona> — recommended: add / confirm defer
```

Then, per persona:

```
flowchart LR
  subgraph P1["Persona · <Archetype name>"]
    direction TB
    S1["Summary: <one-line who-they-are>"]:::sourced
    G1["Goals: <primary goals>"]:::sourced
    N1["Needs: <key needs>"]:::inferred
    PN1["Pains: <main pain points>"]:::confirm
    B1["Behaviours: <behaviours + tech comfort>"]:::inferred
    S1 --> G1 --> N1 --> PN1 --> B1
  end
  OQ1["Open question: <real client-ready question>"]:::oq
  B1 -.-> OQ1
  classDef sourced fill:#CDEBD6,stroke:#3C7D4F,color:#1E4D2B;
  classDef inferred fill:#FFF4CC,stroke:#E6B800,color:#665600;
  classDef confirm fill:#FDE9D0,stroke:#D98A2B,color:#7A4A12;
  classDef oq fill:#FBE0E0,stroke:#D14343,color:#7A1F1F;
```

The `:::class` on each node is illustrative — every node takes the class matching its **actual** provenance.

## Anti-hallucination

- Content comes from the PC (§1/§2/§4) and, for a `segment:` scope, `disc-market-research` §2.1 only. PC silence → a labelled inference + a matching Open Question, never a stated fact.
- **Never fabricate** a name, photo, age, quote, or any demographic the sources do not support.
- Canonical role and glossary names only — never rename/merge/reclassify a PC role.
- If two sources conflict, do not resolve silently → a flagged note.
- Persona scope and count come from the BA (or the recommended 3–5 clustering) — never infer a different scope or top up traits the sources do not state.

## What this skill does NOT do

- Does not edit the Product Context — reads §1/§2/§4, never rewrites (a PC change goes through `disc-pc-update`).
- Does not run market research — consumes `disc-market-research` §2.1 if present; does not fetch the web itself.
- Does not build process/flow diagrams — persona cards are not flows (`disc-diagram-build` owns process/state/sequence/ER diagrams and derives strictly from the PC with no inference).
- Does not write stories, AC, or NFRs (`disc-wbs-story-build` / `disc-bullet-ac` / `disc-nfr`).
- Does not write to Miro / Figma / any tool, and introduces no new decided scope — output is paste-ready; the BA pastes manually.
- Does not treat itself, or let anything treat it, as a source of truth for another artifact — personas feed the market-research canvas / SV §2.1 as illustrative assets, never block them.

## Style

Direct, no preamble. Markdown. Bullets are full sentences. English.
