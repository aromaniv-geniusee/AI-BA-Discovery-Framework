---
name: disc-stakeholders
description: "Generate `02_stakeholders.md` from `01_project_context.md`, kickoff notes, PM messages, org charts, or free-text stakeholder descriptions for a Discovery engagement. Use after disc-project-context or when onboarding onto a discovery project to map internal and client stakeholders, decision authority, responsibilities, communication context, and a BA-focused RACI over discovery deliverables. Create the internal version first, keep Power/Interest for internal use only, and ask whether a client-safe version is also needed."
---

# Discovery — Stakeholder Analysis

Generate `02_stakeholders.md` as the structured stakeholder map for the discovery engagement. It supports elicitation planning, approvals, escalation, and BA alignment. Decision authority is load-bearing here: the people who can make binding client decisions are the ones whose calls get committed to the Product Context.

## Input

Accept any of the following:
- `01_project_context.md`
- Kickoff notes, RFP, PM message with stakeholder mentions
- Existing stakeholder list or org chart
- Free-text description of team members and client contacts

If the project has separate streams, map stakeholders per stream where relevant.

## Procedure

### 1. Extract stakeholders

Identify all relevant people and groups:
- Internal team: BA, PM, tech lead / architect, design, delivery leads
- Client-side: PO, founder/CEO, CTO, PM, SME, approvers, end-user reps
- Vendors or third parties when relevant

For each stakeholder, capture what is known:
- Name
- Role
- Organization
- Contact channel or email if provided
- Time zone if provided
- Responsibilities
- Decision-making authority (what they can bindingly decide)
- Communication preferences
- Concerns, work style, or special notes if explicitly known

Group-level entries are acceptable when individuals are not known yet.

### 2. Classify influence and interest

For internal BA use only, assign:
- Power: High / Low
- Interest: High / Low

If classification is unclear, mark as `TBD` rather than guessing.

### 3. Build a BA-focused RACI

Cover these discovery deliverables:
- Elicitation & requirements gathering
- Requirements / WBS elaboration
- Solution Vision document
- Diagrams / prototype review
- Open-questions resolution & client decisions
- Discovery handoff / sign-off

Generate a sensible default matrix based on known roles, then flag it for review.

### 4. Flag missing stakeholder data

After generation, list stakeholders where critical information is missing. Use a short follow-up note such as:

> The following stakeholders need more information before the next client interaction: [list]. Suggest clarifying this on the next call or async.

Give special weight to any stakeholder whose **decision authority** is unclear — an ambiguous approver stalls every decision that must land in the PC.

## Output template

```markdown
# Stakeholder Map — [Project Name]
_Last updated: [date]_
_⚠ Internal document — Power/Interest columns for the internal team only_

## Stakeholder Register

| Name | Role | Organization | Contact | Time Zone | Power | Interest | Decision Authority | Responsibilities | Concerns / Notes |
|---|---|---|---|---|---|---|---|---|---|
| [Name] | [Role title] | [Client / Internal / Vendor] | [Slack / email] | [GMT+x] | H / L | H / L | [what they can bindingly decide] | [what they own] | [working style, watch-outs] |

## RACI Matrix

| Deliverable | BA | PO / Client | PM | Tech Lead | Designer |
|---|---|---|---|---|---|
| Elicitation & requirements gathering | R | C | I | C | I |
| Requirements / WBS elaboration | R | C | I | C | I |
| Solution Vision document | R | A | C | C | C |
| Diagrams / prototype review | R | C | I | C | C |
| Open-questions resolution & client decisions | C | A | C | C | I |
| Discovery handoff / sign-off | R | A | C | C | I |

> Adjust roles and approvers based on actual project structure.
> R = Responsible, A = Accountable, C = Consulted, I = Informed

## Influence / Interest Notes

- **High Power / High Interest** → manage closely
- **High Power / Low Interest** → keep satisfied
- **Low Power / High Interest** → keep informed
- **Low Power / Low Interest** → monitor with minimal effort

## Open Questions

- [ ] [Name] — missing: [contact / time zone / decision authority]
```

## Rules

- Do not invent names, contacts, time zones, or decision authority.
- Do not assume Power/Interest when the basis is weak — use `TBD`.
- Keep internal political notes out of any client-facing version.
- Generate the internal version first.
- If the BA also needs a client-safe version, remove Power/Interest and internal concerns notes.

## Output

Save as `02_stakeholders.md`. The BA saves it into Project Knowledge manually — no external write.
