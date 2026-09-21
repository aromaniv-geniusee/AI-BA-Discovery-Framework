---
name: disc-validate-notes
type: prompt-template
description: "Lightweight prompt template (not a full skill). Turn raw, messy session notes or a rough transcript into clean, structured notes — fixing attribution, de-duplicating, and flagging unclear passages — as a pre-step before disc-meeting-to-req extracts decisions and requirements. Structuring only; does not extract requirements or make decisions."
---

# Discovery — Validate Notes (prompt template)

A cleanup pass, not an interpretation pass. It takes raw session material and returns tidy, faithful, structured notes the BA can trust before requirements are extracted. It stays close to the source: it reorganizes and clarifies, it does not add, decide, or infer.

## When to use

- Right after a call, when the notes/transcript are messy, out of order, or have garbled attribution.
- Before `disc-meeting-to-req` — clean input makes the extraction reliable.
- Skip it when the notes are already clean and well-structured; go straight to `disc-meeting-to-req`.

## Input

Raw notes or transcript, any format (typed notes, pasted chat, auto-transcript export, voice-memo summary). Optionally `02_stakeholders.md` for speaker identification.

## What to produce

1. **Cleaned notes**, grouped by topic in the order they were discussed, with clear speaker attribution where it can be established from the source or `02_stakeholders.md`.
2. **Fixed attribution** — where a transcript mislabels or leaves a speaker blank and the source makes it recoverable, correct it; where it cannot be recovered, mark `[speaker unclear]`.
3. **De-duplication** — collapse repeated or restated points into one, preserving the fullest version.
4. **Flags** — mark passages that are ambiguous, contradictory, inaudible, or cut off as `[unclear — …]` rather than guessing what was meant.

## Rules

- Structuring only. Do not extract decisions, action items, requirements, or open questions — that is `disc-meeting-to-req`.
- Do not add anything not in the source. If something is implied but not said, leave it out or flag it; never fill the gap.
- Do not resolve a contradiction — surface both sides with `[unclear — conflict]`.
- Preserve domain and glossary terms exactly as spoken.
- Neutral, factual tone; no paraphrasing that shifts meaning.

## Output

Structured markdown returned in-chat. The BA then feeds it into `disc-meeting-to-req`.
