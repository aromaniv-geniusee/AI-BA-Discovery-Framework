---
name: disc-tech-context
description: "Generate `03_tech_context.md` for a Discovery engagement from `01_project_context.md`, tech kickoff notes, stack descriptions, RFPs, API documentation, architecture notes, third-party service docs, or developer input. Use during discovery setup to capture the (often provisional) stack, candidate integrations, architecture notes, technical glossary, constraints, and open technical questions. Ask about domain-specific terms only when glossary language is missing, and never invent API fields, endpoints, or integration details."
---

# Discovery — Tech Context

Generate `03_tech_context.md` as the technical grounding file for the discovery engagement. It supports technically accurate requirements writing, integration scoping, and validation with the tech team, and it feeds the Solution Vision's technical section and `disc-nfr`. At discovery, much of this is provisional — a candidate stack and candidate integrations, not a committed architecture. Mark that state honestly rather than presenting proposals as decisions.

## Inputs

- Architectural or flow diagrams (PDF, screenshots — uploaded directly to Claude), Miro exports
- API specs or third-party service docs (OpenAPI, Postman, vendor pages)
- Tech stack description (free text from tech lead, PM, or RFP)
- Existing tech docs from Google Drive

## Handling diagrams

- Screenshots: upload directly to chat — Claude reads images natively.
- PDF: extract the key diagrams first; do NOT pass full multi-page PDFs.
- For each diagram, capture components, integration points, and data-flow direction. Reference the diagram by name in the output.

## Procedure

### 1. Extract technical context

Identify, if present:
- frontend and backend stack (mark `candidate` / `proposed` where not committed)
- database and infrastructure
- authentication approach
- PM and design tools when relevant for BA work
- external / third-party integrations and APIs
- key technical constraints

### 2. Build the technical glossary

Extract technical and integration terms from the input. For each term:
- write a concise 1–2 sentence definition
- add synonyms or abbreviations if provided
- flag whether clarification is needed

Keep this to technical/integration vocabulary. The product/domain glossary lives in the Product Context (§10) — do not duplicate it here; if a term is product-domain rather than technical, note that it belongs in the PC glossary.

If no glossary language is provided, ask:

> Are there any domain-specific technical terms, abbreviations, or services the team uses that I should know? This helps me write requirements using the correct language.

### 3. Document integrations

For each integration or external API, capture:
- purpose on this project
- data-flow direction: inbound, outbound, or bidirectional
- owner: client-side, third-party, or internal team
- status: confirmed / candidate (proposed but not decided)
- constraints or limitations if known

Discovery often evaluates candidate third-party services — record them as `candidate` and surface their key constraints, but do not present an unconfirmed choice as decided.

### 4. Flag gaps

After generating, explicitly flag:
- stack areas marked `TBD` or `candidate`
- integrations where flow, ownership, or status is unclear
- glossary terms that need confirmation

## Output template

```markdown
# Tech Context — [Project Name]
_Last updated: [date]_
_Discovery-stage — items marked `candidate` / `proposed` are not yet decided._

## Tech Stack

| Layer | Technology | Status | Notes |
|---|---|---|---|
| Frontend | [e.g. TypeScript / React] | confirmed / candidate / TBD | [Notes] |
| Backend | [e.g. Python] | | |
| Database | [TBD] | | |
| Infrastructure | [e.g. Cloudflare] | | |
| Auth | [e.g. OAuth / Cognito] | | |
| PM Tool | [e.g. Jira] | | |
| Design | [e.g. Figma] | | |

---

## External Integrations & APIs

| Integration | Purpose on This Project | Direction | Owner | Status | Constraints / Notes |
|---|---|---|---|---|---|
| [Integration] | [Purpose] | Inbound / Outbound / Bidirectional | [Owner] | confirmed / candidate | [Constraints] |

---

## Architecture Notes

[High-level description of system structure, component relationships, and data flow. Note where the architecture is still a proposal.]

---

## Technical Glossary

| Term | Definition | Synonyms / Abbreviations | Needs Clarification |
|---|---|---|---|
| [Term] | [Clear 1–2 sentence definition] | [Alt names] | Yes / No |

---

## Key Technical Constraints

- [Constraint]
- [Constraint]

---

## Open Technical Questions

- [ ] [Question about API, ownership, data flow, stack choice, or terminology]
```

## Rules

- Do not invent API field names, endpoint paths, payloads, or data types.
- Do not assume integration direction or ownership without evidence.
- Do not present a candidate stack or integration as decided — mark status.
- Do not define glossary terms from generic knowledge if the project may use them differently.
- If the stack is partially known, fill only confirmed layers and mark the rest as `TBD`.

## Output

Save as `03_tech_context.md`. The BA saves it into Project Knowledge manually — no external write.
