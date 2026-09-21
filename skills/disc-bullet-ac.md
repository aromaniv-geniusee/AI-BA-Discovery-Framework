---
name: disc-bullet-ac
description: "Write discovery-depth Acceptance Criteria as a flat bullet list (up to 2 levels) for an approved or rough user story, in the project bullet form — not EARS/Gherkin, not dev-ready. Sharpen presale/rough WBS AC into clear, testable discovery-level behaviour so scope is settled and developers can estimate more accurately. Source of truth is the current Product Context; the WBS raw AC is a seed. Use after a story exists (disc-wbs-story-build) or when refining a presale WBS row, before handoff. Surfaces open questions; does not commit to the PC or write to the Sheet."
---

# Discovery — Bullet Acceptance Criteria

Write Acceptance Criteria for a story as a **flat bullet list (up to 2 levels)** in the project's discovery form. Output is paste-ready for the WBS `Acceptance Criteria` cell; open questions surface for the WBS `Comments / Questions` cell and PC §9.

## Depth calibration — read first

This is **discovery-depth, not dev-ready**. The goal is to take rough or presale-generated AC and sharpen them into clear, testable behaviour that (a) settles what is in scope and (b) lets developers estimate more accurately — one of discovery's purposes. It is **not** the EARS/Gherkin, exhaustive-error, refinement-ready level (`ears-ac` / `gherkin-ac` are delivery-phase). Capture the shape of the feature and its load-bearing cases; leave fine-grained error copy, full scenario matrices, and field-level dictionaries to delivery.

One AC format per project — this bullet form. Do not mix in EARS or Gherkin.

## When to invoke

- After a story is decomposed (`disc-wbs-story-build`), to write its AC.
- When refining a presale WBS row whose raw AC needs to become discovery-clear.
- Before discovery handoff, to raise the AC quality of the scope baseline.

## Inputs

1. **The story/stories** — each with its `Topic` (title), `User Story` statement, and `Role`, read from the WBS per `disc-wbs-schema`, or provided directly.
2. **Product Context** — the authoritative source. Read the **current** document as a whole and reconcile **every** bullet against it — a rule, value, role, field, or state relevant to the story may live in any section, not only the one matching the epic. Do not rely on memory of an earlier version.
3. **Diagrams / designs** — where they exist, a source for behaviour and screen/field names; concrete values seen on a mockup are real input, not guesses. Discovery may have none — do not require them.
4. **WBS raw AC** — the row's existing dash-lines, used as a **seed** only.
5. **Roles** — provided by the BA; they vary per case. Use only the roles appropriate to the story, consistently.

## Source of truth — PC wins, PC silence is not permission

- The **Product Context is authoritative**. Where the PC and the WBS raw AC differ, the PC is the resolved state — **do not silently rewrite**; raise the divergence as an open question.
- Where the PC is **silent** on a point the bullet needs, raise an open question — do not invent a rule and do not accept a raw WBS AC as decided by default.
- This is exactly the presale case: a presale WBS may already carry rough bullets. Treat them as a seed to sharpen and reconcile, never as settled fact.

## The story block

- **Title** (WBS `Topic`) — verb + noun, sentence case, 2–5 words (e.g. "Reset password").
- **User story** (WBS `User Story`) — `As a [role], I want to [goal], so that [benefit].` The `[role]` matches the story's context and comes from the BA-provided set.
- **Acceptance criteria** (WBS `Acceptance Criteria`) — a flat list of **4–7 bullets**, sub-bullets (level 2) only where needed. Keep each bullet short, direct, consistent — grasped in one pass. Avoid super-long bullet text.

## Writing each bullet

- Describe the **business logic and expected system behaviour**.
- State the **filters, conditions, or restrictions** that apply (periods, courses, states, limits). **Be specific — never write "etc."**; if you list something, list it fully.
- Name the **fields involved** inline (e.g. email, title, date, description).
- **Atomic** — one checkable behaviour per bullet; no `or`/`and` hiding two requirements or two branches.
- Refer to the actor in AC as **"user"** — do not duplicate the specific role from the user story into the criteria.
- Maintain a consistent style; keep the focus on practical system behaviour.

## Coverage mindset — a check, not fixed sections

There are no mandatory Happy/Negative sections — the list stays flat. But before finalising, walk these and write a bullet for the ones that matter at discovery depth:

1. **Happy path + forward outcome** — the main success flow and what happens next (advance, save, route). The forward outcome is the single most-forgotten criterion.
2. **Alternative branches** — where one trigger splits by tier, role, or input state, surface each branch (own bullet or sub-bullet) — never blur them into one vague rule.
3. **Key negative / error states** — the load-bearing invalid/blocked cases (not every message; discovery captures the shape).
4. **Main edge cases** — the optional/empty path, an expired/used token, a capped/cooldown action — where relevant to this story.
5. **Role / permission** — only where a role actually gates the behaviour.

Completeness of behaviour, not bulk. Every bullet still earns its place; if the flow can never reach a state, do not write a bullet about it.

## Entity attributes / fields — where relevant

When a story centres on an entity or a form, list the fields that **drive this story's behaviour**:

- **Few (≤ 3–4)** → inline in a bullet.
- **Many** → a short **Attributes** mini-block: `field — type — required/optional` (a light list, not the heavy Data Dictionary of `ears-ac`).
- **Reconcile with PC §3 Domain model.** PC §3 holds the canonical entity; here list only the story-relevant fields and **do not duplicate the full model**. If the story and PC §3 disagree → open question, not a silent divergence.

## Labels & tags — keep it economical, never a Christmas tree

Tag only where it changes how the reader should treat the bullet. A bullet usually carries **at most one provenance label + at most one scope/confirm tag**; a directly-sourced, decided bullet stays **plain**.

- **Provenance (doctrine):** `[client decision]` · `[my inference]` · `[assumption to confirm]`. Inferences and assumptions **must** be marked — never present an inference as a stated decision.
- **`(optional)`** — a natural enhancement that is skippable (nice-to-have). Always also carries a provenance label; if it implies a client choice, add a matching open question.
- **`(proposed — confirm)`** — a behaviour/rule you inferred as **necessary** but no source states; needs a client yes/no → **always** paired with a matching open question. (Distinct from `(optional)`: proposed = probably needed; optional = nice-to-have.)
- **`` `TBD` ``** — an unknown value inside an otherwise-decided behaviour (a duration, a message, a limit) → paired with a matching open question.

## NFRs never live in AC

Performance, scalability, load/throughput, response time, availability — a "how fast / how many / under what load" concern is **never** a functional bullet. Do not write it here. Route it to `disc-nfr` and, so it is not lost, park it as an open question flagged NFR.

## INVEST & slicing

- **Combine** AC under one story when they serve one common use-case for the same user — a report's table + filter + search is **one** story, not three.
- **Split** a flow into stories by step, especially when different users perform different steps.
- **Priority when these tension:** slice by **actor change first**; within one actor, combine. An actor boundary forces a split even when the steps look combinable.

## Out of scope — optional trailing block

Only what is **deliberately** excluded (Future Phase / not collected). Short. Omit the block entirely when there is nothing to exclude.

## Open questions

Every `` `TBD` ``, every `(proposed — confirm)`, and every unresolved assumption produces a **matching open question**, routed **Client / Internal**. This skill **surfaces** them — it does not commit them. They flow to the WBS `Comments / Questions` cell and, on the BA's explicit request, to PC §9 via `disc-pc-update`.

## Output

**In chat — readable block per story:**

```
**Title:** <verb + noun, 2–5 words>
**User Story:** As a <role>, I want to <goal>, so that <benefit>.

**Acceptance Criteria**
- <bullet>  [label] (tag)
- <bullet>
  - <sub-bullet, only where needed>
- <bullet>

**Attributes**            (only when the story centres on an entity with many fields)
- <field> — <type> — required/optional

**Out of Scope**          (only when something is deliberately excluded)
- <excluded item> — Future Phase / not collected

**Open Questions**        (only when TBD / proposed / assumption exists)
- <question> — routing: Client / Internal
```

**Paste-ready:**

- When **updating an existing WBS row**: give the `Acceptance Criteria` cell (D) and, if any, the `Comments / Questions` cell (F) as paste-ready text — multi-line content wrapped so it lands in **one cell**.
- When **producing a new row** (e.g. called by `disc-wbs-story-build`): give the whole row — every column by name (`Epic` (A) · `Topic` (B) · `User Story` (C) · `Acceptance Criteria` (D) · `Integrations` (E) · `Comments / Questions` (F) · `Role` (G) · `Phase` (H)), tab-separated, multi-line cells quoted.
- **Never write to the Sheet.** The BA pastes manually.

## Final gate — run before delivering

1. 4–7 bullets (sub-bullets allowed); each atomic; each short.
2. Every inference/assumption labelled; no inference shown as a decision.
3. Every `` `TBD` `` and `(proposed — confirm)` has a matching open question; no orphan tag.
4. No NFR written as a functional bullet.
5. The actor in AC reads as "user", not the story's role name.
6. No "etc." — every list is specific.
7. Entity fields reconciled with PC §3, not duplicated.
8. AC reconciled against the current PC; any WBS-AC ↔ PC divergence raised as an open question, not silently resolved.
9. Paste-ready cell(s) named by column, with multi-line content quoted so each lands in one cell.

## What this skill does NOT do

- Does not decompose epics — `disc-wbs-story-build`.
- Does not write NFRs — `disc-nfr`.
- Does not write EARS or Gherkin — those are delivery-phase formats.
- Does not commit to the Product Context (`disc-pc-update`) and does not write to the Sheet.
- Does not produce dev-ready AC — that depth belongs to delivery.

## Style

Direct, no preamble. Markdown. Bullets are short full sentences, not fragments. English.
