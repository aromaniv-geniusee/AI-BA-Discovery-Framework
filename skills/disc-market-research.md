---
name: disc-market-research
description: "Run external web research and return paste-ready Market Analysis content for Solution Vision §2 — Customer Segments (2.1), Market Size & Dynamics (2.2, optional), and Alternatives & Competition (2.3). Plan-first: seeds parameters from Project Knowledge, proposes a research plan, waits for the BA's approval, then researches and returns a copy-paste `.md` structured to drop into the SV §2. This is the one discovery skill whose source is the open web, not the Product Context — so the discipline is source-every-claim, label-every-estimate, flag-stale-data, confidence per finding; never fabricate a figure, competitor, segment, or trend. Feeds SV §2 but is not part of SV assembly; `disc-solution-vision` proposes adding it and never blocks on it. Use when presales/discovery needs §2 market content that is not already captured. Read-only external research; no Drive/tool write; the BA pastes manually and edits before the content enters the SV."
---

# Discovery — Market Research (external, for SV §2)

Produce **paste-ready Market Analysis content for Solution Vision §2** from external web research, on a direction the BA confirms first. The skill seeds its parameters from Project Knowledge, **proposes a research plan, waits for "ok"**, then researches and returns one `.md` file structured to drop straight into SV §2 (2.1 Customer Segments · 2.2 Market Size & Dynamics · 2.3 Alternatives & Competition). It sets the direction and does the legwork; the BA verifies and edits the result before it enters the client document.

## Nature of this skill — read first

- This is the **one discovery skill whose source is the external web**, not the Product Context. Every other skill in the set is PC-first and never leaves it; here the deliverable is deliberately outside-in — market, segment, and competitor intelligence the PC does not contain and should not.
- Because the source is the open web, the anti-invention rule **reshapes rather than relaxes**: never fabricate a figure, competitor, segment, or trend. Every material claim carries a real, dated source; market-size and growth numbers are labelled as estimates; stale data is flagged; each finding gets a confidence tag (high/medium/low).
- The output is a **proposal to paste and edit — not finished client copy.** The BA confirms the numbers and framing before §2 goes to the client.
- Market intelligence is **not a product decision.** This skill never edits the Product Context, WBS, or `nfrs`; findings inform the SV narrative, not the decided scope.

## When to use

- Presales/discovery needs SV §2 content and the market picture is not already supplied by the client or captured elsewhere.
- **Direct skill call — no mode.** Situational: skip where the client already provided market/competitor analysis, or where §2 is out of scope for this engagement.

## Inputs — seed from context, then confirm direction

Resourceful-first: read what already exists before asking.

- **Seed from Project Knowledge where present** — pre-fill parameters, don't ask for what is already there:
  - `01_project_context.md` — domain, business model, geography, MVP boundary.
  - `product_context.md` — §1 overview, §2 users/roles (segment seeds), §7 out of scope.
  - `03_tech_context.md` — product form (web / mobile / API), which shapes the competitor set.
- **Then confirm/collect the research parameters** (ask only the genuine gaps, indexed, max 5):
  1. Domain / industry.
  2. Market geography — regions/countries in scope.
  3. Target customer segments — as known (seed from PC §2 / any personas).
  4. Known competitors — a seed list, if the BA or client has one.
  5. Depth — quick scan (headline picture) or deep (corroborated figures, wider competitor set).
  6. Subsections needed — 2.1 / 2.2 (optional) / 2.3, in any combination.

## Procedure

### Step 1 — Propose a research plan, then wait for "ok"

Present, in text and **before any research**:
- the confirmed parameters (and which were seeded from context vs supplied);
- for each requested subsection, **what will be investigated and the angle** — e.g. 2.3 → the competitor set to cover and the comparison axes (offering / pricing / strengths / weaknesses / substitutes); 2.1 → which segments and which attributes (profile, behaviour, needs, decision-making, purchasing power);
- **source strategy** — recent-first (~last 2–3 years); source-type priority: industry/analyst reports, official and company sources, and reputable publications over aggregators and forums;
- a **rough source budget** for the chosen depth, and **known limits** — where public data will likely be thin, name it now as a probable estimate/TBD rather than a promise.

Wait for the BA's "ok" or adjustments. **Do not research before approval.**

### Step 2 — Research

On approval, run the web research per the plan:
- **Recent-first**; prefer primary/authoritative sources; **corroborate load-bearing figures across ≥2 independent sources** where possible.
- Capture, per finding: the claim (**paraphrased, never copied**), the source, its date, and a confidence tag.
- Where public data is thin or sources conflict, **say so** — an honest gap or a ranged estimate beats a confident fabrication. Note conflicts rather than silently picking one number.

### Step 3 — Assemble the paste-ready file (§2 shape)

Produce one `.md` file structured to drop into SV §2, subsection by subsection, per the format below — prose plus the tables the SV expects, every material claim cited to the Sources appendix, estimates and stale data labelled. Present the file; precede it with a one-line coverage note (which subsections are populated, where data was thin). The BA saves it to the Drive working folder and pastes into the SV, editing as needed.

## Output format

A single `.md` file (e.g. `market_research_<domain>.md`), copy-paste-ready into SV §2. Inline markers `[S1]`, `[S2]…` reference the Sources appendix; each material claim ends with a confidence tag. Only include the subsections the BA requested.

```markdown
# Market Analysis — <domain> (draft for SV §2 — verify before use)

_Coverage: 2.1 ✓ · 2.2 ✓/omitted · 2.3 ✓ · Research date: <YYYY-MM-DD>_

## 2.1 Customer Segments

<1–2 sentence intro framing the segments for this product.>

| Segment | Profile / Demographics | Behaviour & Needs | Decision-making / Purchasing power |
|---|---|---|---|
| <segment> | <who they are> [S1] (high) | <what they do / need> [S2] (medium) | <who decides / spend> [S3] (low — estimate) |

- Persona / Problem Statement canvas: <link — placeholder for BA to insert, if conducted>

## 2.2 Market Size and Dynamics   _(optional)_

- **Market size:** <figure or range> — **estimate**, per <body> [S4] (medium).
- **Growth / CAGR:** <figure> over <period>, per <body> [S5] (medium).
- **Key trends:**
  - <trend> [S6] (high)
  - <trend> [S7] (medium)
- **Strategic angle (where answerable):** reputation in this market / desired position / how this product supports it — <short, only if grounded; else omit>.

## 2.3 Alternatives and Competition

<1–2 sentence intro on the competitive landscape.>

| Competitor | Offering | Pricing | Strengths | Weaknesses |
|---|---|---|---|---|
| <name> | <what it does> [S8] (high) | <model / price if public; else "not public — TBD"> | <strength> [S8] (medium) | <weakness> [S9] (low) |

- **Substitutes / indirect competition:** <bullet> [S10] (medium)
- **Positioning takeaway:** <one neutral sentence on the gap this product could occupy — my inference, confirm>.

## Sources

- **[S1]** <Title> — <Publisher> — <YYYY-MM-DD> — <URL> — confidence: high
- **[S2]** <Title> — <Publisher> — <YYYY-MM-DD> — <URL> — confidence: medium
- …
```

## Research & sourcing rules

- **Recency:** prioritise ~last 2–3 years; date-stamp findings; flag anything older as potentially stale.
- **Attribution:** every material claim → a real source. No source → drop it or mark `[assumption to confirm]`. Never invent competitors, figures, or segments.
- **Estimates:** market-size and growth numbers are almost always estimates — label them, give ranges over false precision, and name the estimating body.
- **Quotes:** paraphrase; if a short exact quote is unavoidable, keep it minimal and attributed; never reproduce large blocks or lift a source's structure.
- **Confidence per finding** (high/medium/low), especially on figures — a lone uncorroborated number is medium at best.
- **Neutral framing** on named competitors; no unverifiable or disparaging claims. Report what public sources state.

## What this skill does NOT do

- **Not part of SV assembly** — it feeds SV §2; `disc-solution-vision` proposes adding its output but never blocks on it, and §2 stays a skeleton pointer if research was not run.
- Does not use web findings to alter the **Product Context, WBS, or `nfrs`** — market intel is not a product decision.
- Does not produce **final client-ready copy** — output is a verified-then-pasted proposal; the BA owns the last edit.
- **No tool/Drive write** — read-only external research; the BA saves and pastes manually.
