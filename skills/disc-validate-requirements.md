---
name: disc-validate-requirements
description: "Independent, review-only depth validation of the Requirements Modeling output — the decomposed stories from disc-wbs-story-build and the bullet-AC from disc-bullet-ac — against the decided state. Scope selector: DECOMPOSITION (INVEST / vertical / actor-boundary / statement hygiene), AC (the disc-bullet-ac gate run as an independent pass, plus coverage depth), or BOTH (default — also runs the cross-level split-smell check the two-in-one skill exists for). PC is the canonical spine; 03_tech_context and nfrs are bound-checks (not authorities); meeting notes are a PC-currency signal, never an AC authority. Reports findings with severity and routes each fix to the owning skill (disc-bullet-ac, disc-wbs-story-build, disc-pc-update, disc-nfr); never edits, never resolves a conflict silently. Reads the WBS per disc-wbs-schema. Use to check whether stories/AC are actually right after they are generated — distinct from the per-story self-gate inside disc-bullet-ac and from the project-wide breadth scan of disc-gap-check."
---

# Discovery — Validate Requirements (depth validation of stories + AC)

Independent quality validation of the **Requirements Modeling** output: the decomposed stories (`disc-wbs-story-build`) and their bullet-AC (`disc-bullet-ac`). It answers one question — *is this output actually right?* — checking correctness, completeness, traceability, and INVEST/AC discipline against the decided state.

This is a **depth check on specific stories/AC**, and it is **review-only**: it reports findings with severity and routes each to the skill that owns the fix. It never edits an artifact and never silently resolves a conflict.

## Where it sits — three review tiers, rising altitude

- **Per-story self-gate** — the `disc-bullet-ac` "Final gate": the *same pass that wrote the AC* checks its own work, limited to that skill's own sources. Blind to its own errors by construction.
- **Per-epic depth validation — this skill:** an *independent* pass that can run on stories/AC it did **not** just generate (a presale row, AC from an earlier chat), against a wider source set, and that sees stories and their AC together.
- **Project-wide breadth scan** — `disc-gap-check`: coverage, orphans, duplicates across the whole PC/WBS/`nfrs`/SV map; checks that an AC cell is *populated*, not whether its contents are *correct*.

This skill is the depth tier: `disc-gap-check` asks "is there an AC?"; this asks "is the AC good, and was the story sliced right?".

## Scope selector

| Value | Validates | Use when |
|---|---|---|
| `DECOMPOSITION` | Story slicing only — INVEST, vertical value, actor boundary, statement hygiene, role logic, traceability to the parent WBS story | After `disc-wbs-story-build`, before AC is written |
| `AC` | Acceptance criteria only — the `disc-bullet-ac` gate as an independent pass, plus coverage depth and bound-checks | After `disc-bullet-ac`, on stories whose slicing is already trusted |
| `BOTH` *(default)* | Both catalogues **and** the cross-level check | The normal case — the two levels are coupled (see Step 3) |

**Ask on scope first.** When the BA has not named the scope, ask which of the three to run before loading anything — do not assume. Recommend `BOTH` as the sound default (a story-level defect often explains an AC defect — see Step 3) and say why, but wait for the BA's pick. Only proceed silently when the BA named the scope in the invocation.

## Sources (and their standing)

Getting the standing right is what keeps this validator from re-litigating settled decisions off a non-canonical source.

- **Product Context (`product_context.md`, §1–§13)** — the **canonical spine**. Every correctness and traceability check is against the *decided state*. PC silence is not a defect in the AC — it is an Open Question the AC should already carry.
- **`03_tech_context.md`** — a **bound-check, not an authority**. Does a bullet contradict a stated technical constraint? (This is a check `disc-bullet-ac` does not run — tech context is not in its source list.)
- **`nfrs` file** — a **bound-check, not an authority**. Has a how-fast / how-many / under-what-load concern been smuggled into a functional bullet? Route it to `disc-nfr`; do not accept it as AC.
- **Meeting notes / session output** — a **PC-currency signal, never an AC authority**. If AC diverges from raw notes but matches the PC, the AC is not wrong — the real question is whether the PC captured the session correctly. Route "check PC currency" to `disc-pc-update`; never flag the AC as defective on the strength of notes alone.
- **WBS** — read per **`disc-wbs-schema`** (Path B, Step-0 preflight, header-by-name). Source of the stories/AC under review and of the raw WBS AC (seed only). A raw WBS AC never overrides a PC decision — a divergence is a finding to route, not a defect to score against the AC.

Precedence for every finding: the **PC is canonical**; `03_tech_context` and `nfrs` bound what is realistic/appropriate; notes and raw WBS AC are signals, not authorities.

## When to invoke

- After `disc-wbs-story-build` and/or `disc-bullet-ac`, before the output is trusted as the scope baseline.
- On a presale/imported WBS row whose stories and AC were not generated in this framework and need an independent read.
- As a reviewable step in the Requirements Modeling chain, or as a direct skill call on named stories.

## Input

- **Scope selector** — `DECOMPOSITION` / `AC` / `BOTH`. Confirmed with the BA before validating; if not named in the invocation, ask first (recommending `BOTH`).
- **Target** — an epic name, a set of story titles, or the stories/AC already in-thread (just generated by the chain).
- Reads the PC, `03_tech_context`, and `nfrs` from Project Knowledge; the WBS per `disc-wbs-schema`; any meeting notes the BA points to.

If the target is in-thread output, no WBS read is required; if it is rows on the Sheet, read them per `disc-wbs-schema` and echo-confirm before validating.

## Process

### Step 0 — confirm scope, then preflight & load

First, confirm the scope selector (`DECOMPOSITION` / `AC` / `BOTH`) with the BA if it was not named in the invocation — recommend `BOTH` and say why, but wait for the pick before loading. Then: if reading the WBS from the Sheet, run the `disc-wbs-schema` Step-0 preflight (Code Execution + Drive; stop and ask if missing — never fall back silently) and echo-confirm the read (epic, story count, first `Topic`/`User Story` values). Load the PC in full, plus `03_tech_context`, `nfrs`, and any named notes. If a source returns partial data, **flag it explicitly** and validate what is present — do not fill gaps with assumptions.

### Step 1 — Decomposition checks *(scope DECOMPOSITION or BOTH)*

Per story, against `disc-wbs-story-build` doctrine:

- **INVEST** — Independent, Negotiable, Valuable, Estimable, Small, Testable. Flag the failing letter(s).
- **Vertical slice** — one complete user-visible piece of value, not a single technical layer.
- **Actor boundary** — different users performing different steps of one flow must be separate stories; a mixed-actor story is a split miss.
- **Create ≠ View** — a create/add and a view/list action folded into one story is a defect.
- **Branch-complexity split smell** — a single `want` pulling in many conditions/exceptions/role-branches signals a too-coarse split (this is also the cross-level trigger in Step 3).
- **Shared step duplicated** — a recurring behaviour (consent, verification) written as a child in several flows instead of one canonical story referenced by the rest.
- **Statement hygiene** — one `want` = one intention; active voice; the genuine goal, not a mechanical sub-action; no conditions / optionality / options / field lists / UI detail in the statement; no automatic system outcome written as a story of its own.
- **Role logic** — behaviour differs by role → split; same across roles → one story naming them together; same action → same actor (a different actor for the same action is a business-rule variation to split or an inconsistency to surface, never a silent mismatch).
- **Traceability** — every story groups under a parent WBS story (its link to the scope baseline); an ungrouped story is orphaned.

### Step 2 — AC checks *(scope AC or BOTH)*

Per story's AC, run the `disc-bullet-ac` final gate as an **independent** pass, then the additional depth and bound-checks:

Gate criteria (independently verified, not self-reported):
- 4–7 bullets, sub-bullets only where needed; each **atomic** (no `or`/`and` hiding two behaviours or two branches); each short.
- Every inference/assumption **labelled** (`[my inference]` / `[assumption to confirm]`); **no inference presented as a decision**.
- Every `` `TBD` `` and every `(proposed — confirm)` has a **matching open question** — no orphan tag.
- The actor reads as **"user"**, not the story's role name.
- **No "etc."** — every list is specific and complete.
- Entity fields **reconciled with PC §3**, not duplicated as a full model.

Coverage depth (the `disc-bullet-ac` coverage mindset — a genuine miss is a finding, not bulk-for-bulk's-sake):
- **Happy path + forward outcome** — the main flow and what happens next (advance/save/route). The forward outcome is the single most-forgotten criterion — check it explicitly.
- **Alternative branches** — each tier/role/input-state branch surfaced, not blurred into one vague rule.
- **Key negative / error states** — the load-bearing invalid/blocked cases (shape, not every message).
- **Main edge cases** — optional/empty path, expired/used token, capped/cooldown action, where relevant.
- **Role / permission** — present only where a role actually gates the behaviour.

Traceability + bound-checks (the value this pass adds over the self-gate):
- **PC traceability** — every load-bearing bullet traces to a PC statement, or is a labelled inference, or carries an open question. A bullet stating a rule the PC does not state and without a label/OQ is an **invention** — High severity.
- **PC divergence** — an AC that contradicts a PC decision (field optionality, defaults, added/removed steps, scope). PC is the resolved state → route to `disc-pc-update`; do not score it as an AC style defect.
- **Tech-context bound-check** — a bullet contradicting a stated `03_tech_context` constraint → surface with its constraint reference.
- **NFR-in-AC** — any how-fast / how-many / under-what-load concern written as a functional bullet → route to `disc-nfr`; it is never AC.
- **Notes-currency signal** — AC diverges from meeting notes but matches the PC → **not an AC defect**; route "confirm the PC captured the session correctly" to `disc-pc-update`.

### Step 3 — Cross-level check *(scope BOTH only — the reason this is one skill)*

The two levels are coupled; a defect at one level often *is* a defect at the other:

- **Bloated AC ⇒ coarse slice.** A story whose AC runs well past 7 bullets or carries many branches is a signal the decomposition was too coarse — recommend a re-split, not just an AC trim.
- **Hidden branch/actor in AC ⇒ non-atomic story.** AC that reveals a second actor or a distinct sub-flow means the story was not atomic — recommend the split the AC exposed.
- **Clean slice, unwritable AC.** A story that looks INVEST-fine but whose AC cannot be written cleanly against the PC is a slicing or a PC-gap problem — name which.

Report each cross-level finding once, at the level where the fix belongs, and say which other level it explains.

### Step 4 — Deliver report

```
## Requirements Validation Report — [epic / stories] · scope: [DECOMPOSITION / AC / BOTH]
Validation date: [date]
Sources: Product Context (canonical) · 03_tech_context (bound) · nfrs (bound) · notes (currency signal) · WBS (disc-wbs-schema)

### Summary
| Level | Stories checked | Findings | High severity |
|---|---|---|---|
| Decomposition | N | N | N |
| Acceptance Criteria | N | N | N |
| Cross-level | — | N | N |

### Decomposition findings
- [Story]: [defect — INVEST letter / vertical / actor / statement] — Severity: High/Med/Low — Fix via: disc-wbs-story-build

### AC findings
- [Story]: [defect — missing forward outcome / non-atomic bullet / unlabelled inference / orphan TBD / invention] — Severity: High/Med/Low — Fix via: disc-bullet-ac
- [Story]: AC contradicts PC [§n] ([decided X] vs [AC Y]) — Action: align AC to PC / raise OQ — Fix via: disc-pc-update
- [Story]: NFR concern in functional bullet ([bullet]) — Fix via: disc-nfr
- [Story]: AC diverges from notes but matches PC — Action: confirm PC currency — Fix via: disc-pc-update

### Cross-level findings
- [Story]: AC bloated ([n] bullets, [m] branches) ⇒ decomposition too coarse — Recommended: re-split — Fix via: disc-wbs-story-build

### Recommended next steps
1. [Priority action] — owner skill: [disc-…]
2. [Priority action] — owner skill: [disc-…]
```

## Routing fixes (review-only — this skill never edits)

Every finding names the skill that owns the fix; the fix happens there, on the BA's explicit command:
- Story slicing / structure / role split → **`disc-wbs-story-build`**.
- AC content, labels, coverage, bullets → **`disc-bullet-ac`**.
- A PC divergence, or a notes-vs-PC currency question → **`disc-pc-update`**.
- An NFR concern lifted out of AC → **`disc-nfr`**.

**Never resolve a real conflict silently.** Surface it, stop, route it to the BA (and onward to the client where the decision is theirs).

## Rules

- **Read-only.** Do not modify stories, AC, the PC, `03_tech_context`, or `nfrs` during validation. Output is a report.
- **PC is canonical.** `03_tech_context` and `nfrs` bound what is appropriate; notes and raw WBS AC are signals. A non-canonical source never overrides a recorded PC decision — surface the divergence, do not score the AC against the non-canonical source.
- **Do not invent requirements** to fill a gap. PC silence is an Open Question the AC should carry — flag its absence, do not supply the rule.
- **Depth, not breadth.** Story/AC correctness only; coverage/orphans/duplicates across the whole map are `disc-gap-check`.
- Severity: **High** = an invention, a PC contradiction, a missed forward outcome, or a mis-sliced actor boundary (blocks reliable scope/estimate) · **Medium** = a coverage or atomicity defect to fix before handoff · **Low** = style/label cleanup.
- Pure **renames** (a role/label changed) are silent mapping, not a finding — map to the current term and move on.

## MCP-unavailable fallback

If the WBS cannot be read via MCP (preflight fails and the BA cannot enable Code Execution):
1. Use the CSV export in Project Knowledge per `disc-wbs-schema`, or validate the stories/AC the BA pastes in.
2. Run the validation from the provided content.
3. Note in the report: "Manual input — MCP not used. Verify against live data."

## What this skill does NOT do

- Does not edit any artifact — findings only. AC fixes → `disc-bullet-ac`; story fixes → `disc-wbs-story-build`; PC edits → `disc-pc-update`; NFR moves → `disc-nfr`.
- Does not decompose epics or write stories (`disc-wbs-story-build`) or write AC (`disc-bullet-ac`) — it validates their output.
- Does not run the project-wide breadth scan — coverage, orphans, and duplicates across the map are `disc-gap-check`.
- Does not commit anything to the Product Context — that is `disc-pc-update`.
- Does not treat `03_tech_context`, `nfrs`, meeting notes, or a raw WBS AC as authority over a PC decision.
- Does not validate a prompt — that is `prompt-validator`; this validates the *output*.
