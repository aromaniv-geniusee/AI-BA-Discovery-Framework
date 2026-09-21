---
name: disc-nfr
description: "Generate or validate discovery Non-Functional Requirements using ISO 25010 quality attributes and Utility Tree thinking, grounded in the project's NFRs tab (the Geniusee elicitation template: Category / Definition / Question / Geniusee default commitment / Client's answer / Additional info), the Product Context, and 03_tech_context. Produces structured, measurable NFR scenarios into a separate paste-ready `nfrs` file — not a PC section. Reads the NFRs tab per disc-wbs-schema. Use when defining or reviewing NFRs for a feature, epic, or system during discovery."
---

# Discovery — NFR (quality attributes)

Generate and validate Non-Functional Requirements for a feature, epic, or system using the Utility Tree approach (ISO 25010 quality attributes), grounded in the project's **NFRs tab** — the Geniusee elicitation template already filled (in part) with the client. Produces structured NFR scenarios with **measurable** response measures — not vague statements like "the system must be fast."

NFRs live in a **separate `nfrs` file**, owned by this skill — **never** a Product Context section. The PC references it; do not duplicate NFR content into the PC.

## When to invoke

Starting or scoping an epic, preparing for an architecture/tech review, or when a functional story surfaces a "how fast / how many / under what load" concern that `disc-bullet-ac` correctly refused to write as AC.

## Input

- Feature / epic / system scope (named by the BA, or "project-wide").
- Optional: known constraints (SLA, user volume, regulatory requirements, tech-stack limits).
- Optional: mode — `GENERATE` (write NFRs) or `VALIDATE` (review existing NFRs). Default `GENERATE`.

## Sources (read automatically)

- **NFRs tab** — read per **`disc-wbs-schema`** (Path B, Step-0 preflight, forward-fill on `Category`/`Definition`). This is the primary seed: each row is a `Question` under a `Category`/`Definition`, with the **`Geniusee default commitment`**, the **`Client's answer`** (may be empty), and **`Additional info`** (often a quality-attribute scenario). Use the client's answer where present; fall back to the Geniusee default where not; never invent beyond either.
- **Product Context (`product_context.md`)** — business goals (§1), functional domains (§4), roles (§2). Each NFR links to a business goal or user need from here.
- **`03_tech_context.md`** — stack, candidate integrations, and technical constraints that bound what is realistic. Mark values `candidate`/`TBD` honestly, as tech context does.

No Confluence/Jira MCP in discovery.

## Process

### GENERATE mode

**Step 1 — Identify relevant quality attributes.** Start from the `Category` values present in the NFRs tab (e.g. Availability, Compatibility, Performance Efficiency) and map them to ISO 25010; add an attribute only if the feature clearly needs one the tab omits. Select only relevant attributes — not all apply to every scope.

| Category | Attributes to consider |
|---|---|
| Performance efficiency | Time behaviour, resource utilisation, capacity |
| Reliability | Fault tolerance, recoverability, availability |
| Security | Confidentiality, integrity, authentication, authorisation |
| Usability | Learnability, operability, accessibility |
| Maintainability | Modularity, testability, modifiability |
| Portability | Adaptability, installability |
| Compatibility | Interoperability, co-existence |

**Step 2 — Build the Utility Tree.** For each selected attribute, turn the tab's `Question` + answer into a scenario:

| Field | Source |
|---|---|
| Quality attribute | ISO 25010 name (mapped from the tab `Category`) |
| Rationale | Why it matters here — from PC business goal / the tab `Definition` |
| Business goal | The PC §1 goal it supports |
| Source of stimulus | Who/what triggers it |
| Stimulus | The specific triggering event |
| Environment | Conditions (e.g. peak load) |
| Artifact | The affected part of the system |
| Response | What the system does |
| Response measure | Measurable threshold — from `Client's answer` if given, else the `Geniusee default commitment`, else `TBD` (Goal / Stretch / Wish where useful) |

Where the client has not answered and no default applies, write `TBD` and open a matching NFR question — do **not** invent a numeric value.

**Step 3 — Coverage check.** After generating, mark ❌ where a load-bearing area is uncovered: performance under peak load; error/failure recovery; data security and access control; availability/uptime; compatibility (browsers/devices/resolutions); logging and auditability (if a regulated domain). Recommend the scenario to add.

**Step 4 — Deliver the `nfrs` file** (paste-ready). Also, where useful, emit paste-ready cells for the NFRs tab — each named by its column (`Geniusee default commitment` (D) / `Additional info` (F)) and given as the content of one cell, multi-line content quoted so it lands in one cell; the BA applies them manually.

```
# NFRs — <project / scope>
_Last updated: <date> · Source: NFRs tab + PC + 03_tech_context_

## <Quality attribute — e.g. Performance efficiency · Time behaviour>
**Rationale:** <why it matters here>
**Business goal:** <PC §1 goal>

| Field | Value |
|---|---|
| Source of stimulus | <…> |
| Stimulus | <…> |
| Environment | <…> |
| Artifact | <…> |
| Response | <…> |
| Response measure | Goal: <…> / Stretch: <…> / Wish: <…>  (or `TBD`) |

---
[next attribute…]

## Coverage check
✅ Performance — covered
❌ Recoverability — not covered — recommended: add offline/retry scenario

## Open questions
- <NFR question> — routing: Client / Internal — status: Open
```

### VALIDATE mode

**Step 1 — Read existing NFRs** (the `nfrs` file, or the NFRs tab answers).

**Step 2 — Check each NFR:**

| Criterion | Check |
|---|---|
| Measurable | A specific threshold, not "fast" or "secure" |
| Testable | Verifiable in a test scenario |
| Contextual | Tied to a specific feature/component, not generic |
| Realistic | Achievable given `03_tech_context` stack/constraints |
| Traceable | Linked to a PC business goal or user need |

**Step 3 — Return the report:**

```
## NFR Validation Report — <scope>

| NFR | Measurable | Testable | Contextual | Issue |
|---|---|---|---|---|
| "System must be fast" | ❌ | ❌ | ❌ | No threshold, not testable, not scoped |
| "Response ≤2s under 10k users" | ✅ | ✅ | ✅ | — |

### Recommended fixes
- NFR 1: rewrite as "API response time ≤ [X]ms for [Y]% of requests under [Z] concurrent users"
```

## Rules

- Do not invent technical values (throughput, latency, uptime) — use `TBD` where real data is unknown and open a question for the BA to confirm with the tech lead/client.
- Every NFR links to a PC business goal or user need — standalone technical NFRs are not acceptable.
- Do not include categories not relevant to the scope.
- Prefer the `Client's answer` over the `Geniusee default commitment`; use the default only as a fallback, and mark it as a default, not a client decision.
- NFRs stay in the `nfrs` file, never in the PC. Load-bearing NFR unknowns that block scope may also be routed to PC §9 via `disc-pc-update` on the BA's request; the NFR detail itself stays in `nfrs`.

## MCP-unavailable fallback

If the NFRs tab cannot be read via MCP (preflight fails and Code Execution cannot be enabled):
1. Use the CSV export in Project Knowledge per `disc-wbs-schema`, or ask the BA to paste the NFRs-tab content and any known constraints.
2. Generate/validate from the provided content.
3. Mark all numeric thresholds `TBD` unless the BA provides them, and note "Manual input — verify against live data."

## Output

Save as `nfrs` (the discovery NFR file). The BA saves it into Project Knowledge manually — no external write. `nfrs` feeds the Solution Vision's technical/quality section via `disc-solution-vision`.

## What this skill does NOT do

- Does not write functional AC — that is `disc-bullet-ac`.
- Does not create an NFR section inside the Product Context — NFRs live only in `nfrs`.
- Does not write to the Sheet — output is paste-ready.
