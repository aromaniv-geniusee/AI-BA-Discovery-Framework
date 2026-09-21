---
name: disc-prototype-build
description: "Build a situational low-fidelity HTML prototype that visualizes discovery requirements for base-UX alignment — screen structure, fields, actions, states, and flow — derived strictly from the Product Context and the WBS stories/AC (PC silence → open question, never invented). Deliberately wireframe-level, not final design: neutral, boxy, clearly a draft. Output is a self-contained HTML file the BA opens, saves, and shares; no diagramming/design-tool write. Use when a rough visual would settle base UX faster than prose; does not produce production UI and does not edit the PC."
---

# Discovery — Prototype Build (low-fi)

Build a **low-fidelity HTML prototype** that turns discovery requirements into something the client and BA can look at and react to — screen structure, the fields on it, the actions available, the states it can be in, and how steps connect. It exists to **align on base UX**, not to design the product.

## Fidelity calibration — read first

This is **wireframe-level, deliberately not final design.** Do **not** apply the `frontend-design` skill or any distinctive/branded visual direction — that is delivery/design-phase work and would send the wrong signal in discovery ("this is the design"). Here the prototype must read as an obvious draft: neutral greys, plain boxes, system font, placeholder labels, no brand, no imagery, no polish. The value is in **structure and behaviour agreement**, and lowering fidelity keeps the conversation on *what the screen does and needs*, not on colour and typography.

## When to use

Situational — only when a rough visual settles base UX faster than prose:
- a form with non-trivial fields/validation,
- a list/table with filters,
- a dashboard/layout arrangement,
- a multi-step flow the client needs to *see* to react to.

Skip it when a diagram (`disc-diagram-build`) or the AC alone already make the UX clear.

## Inputs (per run)

- **Target** — named by the BA: the screen, form, flow, or feature to prototype.
- **Product Context (PC)** — the source of truth for content: the functional domain (§4) and its capabilities, the entity and its fields (§3), roles and their permissions (§2), states and transitions.
- **WBS stories + AC** — the story and its bullet-AC (via `disc-bullet-ac` / the WBS) for the fields, actions, conditions, and branches the screen must express.
- **Diagrams** — an existing flow (`disc-diagram-build`) where the prototype visualizes a step in it.

## Content source — PC-first, never invented

- Every field, action, state, and role-gated behaviour shown comes from the **PC or the AC**. Low fidelity governs the *visual*, not the *content* — the content is still traceable.
- **PC/AC silence is never filled with a guessed field or rule.** Where the screen needs something the sources do not state, show it as an explicit **placeholder** and raise a matching **open question** — do not invent a field, validation, or action.
- Canonical role and glossary terms only; never rename or reclassify.
- If two sources conflict, do not resolve silently — annotate it and flag it.

## Procedure

### Step 1 — Brief the prototype, then wait for "ok"

Before building, present in text: the target screen(s); the roles who use it and their scope; the fields (with which are required, per AC); the actions/buttons and what each does; the states the screen can be in (empty, populated, error, blocked/denied); and the open questions where sources are silent. Cite the PC sections and AC used. Wait for the BA's "ok".

### Step 2 — Build the low-fi HTML

Produce one self-contained HTML file per the fidelity and technical rules below. Represent, at wireframe level:
- **Regions**: header/nav, main content, side panels — as plain labelled boxes.
- **Fields**: labelled inputs with type hinted (text / date / select / checkbox), required marked; group per the entity.
- **Actions**: buttons/links named by what they do ("Save changes", not "Submit"), placed where the flow expects them.
- **States**: show the load-bearing ones — the populated view plus at least the empty state and one error/blocked state (as separate panels or a simple toggle), per the AC.
- **Role-gating**: where a role sees/does less, show that variant (a disabled action, a hidden panel) rather than assuming the fullest permission.
- **Multi-step flow**: simple step navigation (prev/next) so reviewers can walk the sequence.

### Step 3 — Annotate for traceability & open questions

On or beside each screen, add light annotations: which PC §/AC each region ties to, and the **open questions** as real client-ready questions attached to the relevant element. Keep annotations visibly separate from the mock content (a margin note / callout style), so the draft stays readable.

### Step 4 (optional) — Coverage note

Briefly note which AC items/states did **not** make it into the prototype and why (out of this screen's scope, or a genuine gap) — the BA decides whether to extend it or raise it.

## Fidelity rules (what low-fi means here)

- **Greyscale + one neutral accent** only; system/sans font; generous borders so regions read as boxes. No brand colours, gradients, shadows-for-style, icons-as-decoration, or real images (use labelled placeholder boxes).
- **Placeholder content is labelled as such** — `[user name]`, `[list of events]` — never realistic fake data that could be mistaken for a decision.
- **A visible "DRAFT — low-fi, not final design" banner** at the top of the file, always.
- **Interactivity only where it clarifies UX** — step navigation, a state toggle, showing/hiding a role-gated region. No real logic, no data persistence, nothing that implies a built feature.
- Structure over beauty: spacing and grouping should make the information architecture obvious; that is the whole deliverable.

## Technical constraints

- **One self-contained HTML file** — inline CSS, minimal inline JS only for the interactivity above. No external stylesheets, fonts, scripts, images, or network calls, so it opens anywhere in a browser and travels in Google Drive.
- **No browser storage** (`localStorage`/`sessionStorage`) and no backend — state is in-memory only for the session.
- A basic quality floor: opens in a browser, is legible on a laptop screen, keyboard-focusable controls. Nothing beyond that — polish is out of scope by design.

## Output

A self-contained `.html` file (e.g. `prototype_<target>.html`), delivered for the BA to open, save into the Drive working folder, and share for review. Present the file. Precede it with the Step-1 brief and follow it with the open questions. No design/diagramming-tool write — the BA shares the file manually. The prototype may later feed the Solution Vision (`disc-solution-vision`) as an illustrative asset, never as a source of truth.

## What this skill does NOT do

- Does not produce production or high-fidelity UI, and does not apply `frontend-design` — discovery UX alignment only.
- Does not invent fields, actions, validations, or data not in the PC/AC.
- Does not write to Figma/design tools, and does not edit the Product Context.
