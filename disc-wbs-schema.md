# Discovery — Sheet Schema (`disc-wbs-schema`)

Single source of the discovery Sheet layout **and** the read contract every WBS/NFRs-reading skill follows. Skills reference this file instead of hardcoding columns, header rows, or the read method — so a layout or access change is edited **here once**. Reading skills that depend on it: `disc-wbs-story-build`, `disc-bullet-ac`, `disc-gap-check`, `disc-nfr`, `disc-solution-vision`.

This is a reference file, not an invokable `/skill`.

## The file being read

- A **2-tab copy** of the project WBS, kept in the corporate Google Drive folder(s). The copy contains only the two tabs skills read: **`WBS & Development Efforts`** and **`NFRs`**. Trimming the other tabs is deliberate — the MCP export pulls the whole workbook, so a 2-tab file keeps the parse clean and echo-confirm unambiguous.
- **Read-only.** No skill writes to the Sheet. All skill output is **paste-ready** for the BA to apply manually.
- **Sync caveat:** the copy is a second source of truth. If the original is edited but the copy is not, skills will faithfully read a **stale copy**. Mitigation: treat the copy as the working discovery WBS and edit *it* directly, or keep it strictly derived and re-copy after original changes. `[assumption to confirm]` — confirm the BA's chosen sync model per project.

## Read contract (every reading skill runs this)

### Step 0 — preflight (mandatory, before any read)

Verify both are available in the current chat:
1. **Code Execution** (needed to parse the exported `.xlsx`).
2. **Google Drive** connector (needed to fetch the Sheet).

Probe cheaply (a trivial code run + a Drive metadata read). If either is missing → **STOP and ask the BA** to enable Code Execution (Settings → Feature previews) and/or reconnect Google Drive, then resume. **Do not fall back silently** and do not fabricate data. Only if the BA cannot enable Code Execution does the CSV fallback below become the path.

### Default read — Path B (MCP-download → parse)

1. `Google Drive:download_file_content` with `exportMimeType = application/vnd.openxmlformats-officedocument.spreadsheetml.sheet` (xlsx). The whole workbook exports; the copy has only the two tabs.
2. Parse with `openpyxl` (`data_only=True`); select the named tab.
3. **Locate the header by name-match** (see synonyms), not by a fixed offset. In the reference copy the header sits on row 3, but do not assume it — match the names. **Fail loud** if the header cannot be matched; never guess a shifted layout.
4. Build a `canonical-name → column-index` map from the header using the synonyms table.
5. Scope columns: on **`WBS & Development Efforts`** read only the mapped columns **A–H** and ignore everything from column I onward. On **`NFRs`** read A–F.
6. **Forward-fill scoped:** `NFRs` has vertical merges in `Category` / `Definition` → forward-fill continuation rows from the merge anchor. `WBS & Development Efforts` A–H has **no** vertical merges (Epic is repeated literally per row) → **do not** forward-fill.
7. **Echo-confirm + spot-check** (below) before using the read for anything.

Do NOT use `Google Drive:read_file_content` as the extraction path: it flattens all tabs into one undelimited markdown blob (no per-tab boundary) and bleeds `Phase` values across tabs — validated to over-count. It is acceptable only for a quick human glance, never for structured extraction.

### Fallback — CSV in Project Knowledge

A manual CSV export of the tab, placed in Project Knowledge, read as text. Used **only** when Step 0 shows Code Execution or Drive is unavailable. State out loud when this path is active. Limitation: CSV export carries one tab, drops merges (NFRs `Category`/`Definition` continuation cells come back blank — forward-fill by position from the last non-empty value and flag it), and reflects only the last manual export (staleness risk).

### Caching within a session

Download **once per session**; subsequent reads use the local parsed copy. Re-download only when the BA says the sheet changed ("я оновив аркуш"), on a new chat, or after a container reset.

## Tab 1 — `WBS & Development Efforts`

Header located by name (reference copy: row 3, two preamble rows above). Columns A–H, left to right:

| Col | Canonical name | Meaning |
|---|---|---|
| A | `Epic` | Which epic the row belongs to (e.g. "Events"). **This is how epics are grouped** — see below. |
| B | `Topic` | Short story title (e.g. "Reset password"). |
| C | `User Story` | Full story statement ("As a role, I want …, so that …"). |
| D | `Acceptance Criteria` | Raw AC for the row (dash-prefixed lines, may be multi-line in one cell). |
| E | `Integrations` | Integration notes for the row (may be empty). |
| F | `Comments / Questions` | Free-text notes / open questions (may be empty). |
| G | `Role` | Role(s) for the story (e.g. "Collector, EventHost"). May seed role resolution; the Product Context still governs on conflict. |
| H | `Phase` | Delivery phase — `MVP`, `Phase 2`, or `Future Phases`. |

Columns **from I onward are ignored** (effort/estimation and other delivery-only columns). Column **letters are not fixed** and must not be hardcoded — read by header name.

**Epic detection:** an epic = all rows sharing the same `Epic` value (e.g. every row with `Epic = "Events"`). No vertical merge is used — the `Epic` value is repeated on each of its rows. Each row within an epic is one **parent story**: `Topic` = short title, `User Story` = full statement, `Acceptance Criteria` = raw AC, `Role`/`Phase` as above. Process only the rows of the named epic; never reach into other epics.

**Phase filter:** `MVP` = in scope for current discovery focus. `Phase 2` and `Future Phases` = out of the immediate MVP slice; include only when the BA explicitly asks.

## Tab 2 — `NFRs`

Header located by name (reference copy: row 3). Read A–F. Owned/read by `disc-nfr`.

| Col | Canonical name | Meaning |
|---|---|---|
| A | `Category` | NFR category (e.g. Availability, Compatibility, Performance Efficiency). **Vertically merged** across its question rows. |
| B | `Definition` | Category definition. **Vertically merged**, mirrors `Category`. |
| C | `Question` | The elicitation question under the category. |
| D | `Geniusee default commitment` | Geniusee's default position/answer. |
| E | `Client's answer` | The client's answer (may be empty during discovery). |
| F | `Additional info` | Notes, quality-attribute scenarios, extra context. |

**Forward-fill required** on A and B: continuation question rows share the merged `Category`/`Definition` above them — carry the anchor value down so each question row is self-contained.

## Column-name matching + synonyms (edit here on rename)

Skills match header cells against the **canonical name or any listed synonym** (case-insensitive, trimmed). To adapt to a client's renamed column, **add one synonym line** — no skill edits.

**`WBS & Development Efforts`:**
- `Epic` — (syn: Epic Name)
- `Topic` — (syn: Story Title, Title)
- `User Story` — (syn: Story, User story)
- `Acceptance Criteria` — (syn: AC, Acceptance criteria)
- `Integrations` — (syn: Integration, Integration notes)
- `Comments / Questions` — (syn: Comments, Questions, Notes)
- `Role` — (syn: Roles)
- `Phase` — (syn: Stage, Delivery Phase)

`Phase` values — (`MVP`) · (`Phase 2`, syn: Phase2) · (`Future Phases`, syn: Future)

**`NFRs`:**
- `Category` — (syn: NFR Category)
- `Definition` — (syn: Description)
- `Question` — (syn: Questions)
- `Geniusee default commitment` — (syn: Default commitment, Default, Geniusee default)
- `Client's answer` — (syn: Client answer, Answer)
- `Additional info` — (syn: Info, Notes)

**Fail-loud rules:**
- An **expected canonical column is not matched** (no name or synonym found) → STOP, report which, ask the BA to confirm the rename or point to the right header. Never guess a position.
- An **unknown extra column** appears → report it and ignore it (do not fail on additions).

## Echo-confirm (mandatory before a read is used)

Before decomposing, writing AC, checking gaps, or extracting NFRs, the skill states and the BA confirms:
- tab read + header row located;
- the `name → column` map, incl. any **expected-not-found** and any **unknown-extra** columns;
- for WBS: the epic list with parent-row counts (and Phase breakdown when relevant);
- a spot-check: the first 2–3 `Topic` / `User Story` values (WBS) or `Category` / `Question` values (NFRs), verbatim, so a mis-read is caught before it propagates.

## Change discipline

When the layout, tab names, header row, access method, or synonyms change → **edit this file only**. Skills reference it; they must not re-declare columns, header row, epic logic, or the read contract inline.
