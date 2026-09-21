---
name: disc-wbs-story-build
description: "Decompose a discovery WBS epic into smaller, independent, vertical user stories (INVEST), each written as a full 'As a role, I want, so that' statement grouped under its parent WBS story, and emit them as paste-ready WBS rows (the whole row A–H, each column named). Reads the WBS per disc-wbs-schema and the Product Context; names the splitting pattern; surfaces split decisions and WBS-vs-PC gaps for BA review. Story statements only — acceptance criteria are added afterwards by disc-bullet-ac. Read-only on the Sheet; the BA pastes rows manually."
---

# Discovery — WBS Story Build (epic → stories)

Turn one WBS epic into smaller, valuable **vertical** user stories — INVEST-compliant — each traceable to the parent WBS story it came from, and hand them back as **paste-ready WBS rows** the BA applies manually. Story statements only; acceptance criteria are added afterwards by `disc-bullet-ac`.

## When to invoke

When the BA wants to break a discovery WBS epic into stories, before writing AC. Works on any epic in the WBS.

## Inputs

The BA names the epic. Read these automatically.

1. **WBS** — read per **`disc-wbs-schema`** (the single source for the layout, access method, and read contract — columns, header row by name, epic detection, the Path-B MCP read, and the Step-0 preflight). Do not hardcode column positions or the read method here; follow the schema file.
   - In short (see `disc-wbs-schema` for the authoritative version): run the **Step-0 preflight** (Code Execution + Drive; stop and ask if missing — never fall back silently). Locate the header by matching column names; **an epic = all rows sharing the named `Epic` value**; each such row is a parent story (`Topic` = short title, `User Story` = full statement, `Acceptance Criteria` = raw AC seed, `Role`, `Phase`).
   - **Echo-confirm (before decomposing):** per the schema, print for BA review — the epic matched, the count of parent-story rows found, the Phase breakdown, and the first 2–3 `Topic` + `User Story` values read verbatim. The BA confirms the right rows were picked up before the skill continues. If the epic value matches no rows, or the header cannot be located, **stop and say so** — never guess a shifted layout.
2. **Product Context** — the authoritative source for content. Read it as a whole: for every story search the entire document — a rule relevant to the story may live in any section, not only the one matching the epic's name. Where the Product Context and the WBS raw AC conflict, the **Product Context wins** (surface the divergence; do not silently rewrite).

## Process

For the named epic, take each parent WBS story (each row with that `Epic` value) in turn:

- If it is already an atomic, INVEST-compliant vertical slice, keep it as **one story** and say so in a line.
- Otherwise decompose it into child stories, each a full statement: `As a <role>, I want <action>, so that <benefit>.`

Group every resulting story under its parent WBS story title (`Topic`) — this grouping is the traceability link to the parent. Then add **Split flags** and **Gaps & conflicts**, and finally emit the **paste-ready rows**.

## Decomposition guidelines

- Split **vertically**: each story delivers one complete, user-visible piece of value end-to-end — the UI, the logic behind it, and the data it needs, together — rather than a single technical layer on its own.
- Apply **INVEST** (Independent, Negotiable, Valuable, Estimable, Small, Testable).
- Name the splitting pattern used. Common ones:
  1. **Workflow Steps** — sequential stages of one flow (e.g. provide details → verify → agree to terms).
  2. **CRUD Operations**.
  3. **Business Rule Variations** — different rules or branches (often a role-based split, see Role logic).
  4. **Data Variations**.
  5. **Data Entry Methods (UI)**.
- When a candidate child is trivial or naturally belongs inside another, raise it as a **Split flag** for the BA to decide (e.g. "auto-redirect to welcome page could fold into the first story").
- **Create is not View.** Keep a create/add action and a view/list action as separate vertical slices — never fold one into the other, even when they share a screen.
- **Branch complexity is a split smell.** If a single `want` pulls in many conditions, exceptions, or role/branch variations, the split is probably too coarse — re-split, so each story stays one clean intention. (These conditions become AC downstream in `disc-bullet-ac`; catching them here keeps AC short.)
- **Shared steps → one canonical story.** When the same behaviour recurs across several flows in the epic (e.g. consent to Terms, email verification), create **one** canonical story for it and have the other flows reference it as an AC step — never duplicate it as a child in each flow.
- **Actor boundary forces a split.** Consistent with `disc-bullet-ac`: where different users perform different steps of one flow, slice by actor change first, even when the steps otherwise look combinable.

## Story statement rules

The `want` must be clean — detail lives in the AC, not in the statement.

- **One `want` = one user intention.** No compound actions ("open the link **and** reach the page", "activate **and** set a password"). Split them or pick the single real intention.
- **State the genuine goal, in active voice.** Name what the user wants to achieve (`create an account`), not the mechanical sub-action (`provide details`), and never passive/system-driven phrasing (`be taken to`, `be routed to`).
- **Keep conditions, optionality, options, field lists, and UI details out of the statement** — they are Acceptance Criteria. So: not `(optionally)`, not `(with a "Remember me" option)`, not `when the org has subgroups`, not `with a code`.
- **Automatic system outcomes are not stories.** Auto-redirects, default-role assignment, and similar happen without the user acting — they are AC of the triggering story, not stories of their own.

## Role logic

WBS stories are often written generically as `As a User`. A `Role` column value (e.g. "Collector, EventHost") may seed the actor, but resolve the real role against the Product Context role list:

- If behaviour differs by role, split into separate stories per role (Business Rule Variation).
- If behaviour is the same across roles, write **one** story naming the roles together: `As a System Admin and Admin, I want …`.
- One role-context per story otherwise.
- **Same action — same actor.** An identical action takes the same role(s) wherever it appears; a different actor for the same action is either a genuine business-rule variation (split by role) or an inconsistency to resolve — not a silent mismatch.
- If the correct role cannot be determined from the WBS `Role` column and the Product Context, write the role as `TBD` and list it under Gaps.

## Gaps & conflicts

Surface these for BA review rather than resolving them silently:

- **WBS↔PC divergence** — where a parent's raw WBS AC differs from the Product Context (field optionality, defaults, added or removed steps, scope), note it; the Product Context is the resolved state. Flag a divergence **only when it changes a field, rule, or scope decision**. Treat pure **renames** (e.g. a role label changed) as silent mapping — map to the current term and move on, no Gaps line.
- **Dependency / duplicate** — where a child slice duplicates or depends on another parent WBS story in the same epic (e.g. a verification step that also exists as its own WBS story), flag it instead of creating a duplicate story.
- **Role TBD** — any unresolved role.
- **Missing** — anything the Product Context is silent on; write `TBD`.

## Anti-hallucination

- Use only the epic's WBS rows and the Product Context. Introduce no roles, entities, flows, or fields that are not there.
- Anything unknown or unclear → `TBD`, listed under Gaps.
- Use only Product Context glossary terminology.

## Output format

First the readable decomposition, then the paste-ready rows.

```
# Decomposition — <Epic name>

<echo-confirm summary: epic matched · parent rows found · Phase breakdown · first 2–3 Topic/User Story values>

## <Parent WBS story title>
Pattern: <pattern(s)>
1. As a <role>, I want <action>, so that <benefit>.
2. As a <role>, I want <action>, so that <benefit>.

## <Parent WBS story title — kept whole>
Already an atomic slice; kept as one story.
As a <role>, I want <action>, so that <benefit>.

## Split flags
- <merge / boundary decisions for the BA>
- <how children relate to the parent row: replace the parent row, or append beneath it — the BA decides>

## Gaps & conflicts
- WBS↔PC: <divergence; PC is the resolved state>
- Dependency/duplicate: <…>
- Role TBD: <…>
- Missing: <… TBD>

## Paste-ready rows  (whole row A–H — each column named; multi-line cells quoted so each lands in one cell)
<one tab-separated row per resulting story, one column per name:
 Epic (A) · Topic (B, story title) · User Story (C) · Acceptance Criteria (D, empty — added by disc-bullet-ac) ·
 Integrations (E, seed if any) · Comments / Questions (F, seed / open questions if any) ·
 Role (G, resolved) · Phase (H, inherited from parent unless the BA changes it)
 — multi-line cells quoted so each lands in one cell>
```

Return as markdown. **Read-only on the Sheet** — never write; the BA pastes the rows manually.

### Notes on the paste-ready rows

- **`Acceptance Criteria` (D) is left empty here** — it is filled next by `disc-bullet-ac`, which owns AC. Do not write AC in this skill.
- **`Phase` (H)** inherits the parent's value; flag in a Split-flag line if a child plausibly belongs to a different phase, and let the BA decide.
- **Parent vs children** — whether the children replace the parent row or are appended beneath it is a BA decision (raised as a Split flag), not resolved silently.
- **`Topic` (B)** of a child is its own short title (verb + noun, sentence case, 2–5 words), consistent with `disc-bullet-ac` title rules.

## Relation to other skills

Reads via `disc-wbs-schema`. Feeds `disc-bullet-ac` (AC per story). Requirements that arrive from a session enter through `disc-meeting-to-req` first. Story statements only — no AC, no commits, no Sheet writes here.
