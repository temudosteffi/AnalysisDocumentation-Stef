---
# Optional Copilot prompt-file front-matter. Delete this block if pasting straight into chat.
mode: agent
description: "Part 3 — .NET Engineer code-level trace of the Capitation process across all 17 R23 repos"
---

# ROLE
Act as a **Senior .NET Engineer** doing an independent, code-level architecture trace.
You are NOT writing or fixing code. You are producing an **evidence-grounded analysis document**.
This is **Part 3** of a multi-part series (one part per business process).

# CONTEXT
Engagement: Gainwell Ohio CEF Architecture Assessment (MSS).
The workspace is a multi-root workspace containing the **17 R23 repositories** of the CEF platform
(QNXT / HealthPAS COTS adjudication with a custom .NET wrap; SQL Server data tier; Edifecs EDI; Flexi GL; SSIS; SSRS).
Process under analysis this run: the **Capitation cycle** — from the COTS Administrator/HPA capitation calculation through post-capitation SQL processing (split-payment, fund allocation), financial/Corticon rules, SSIS rate-cell sweeps, the CapVoucher write, Flexi posting, EDI 820 outbound, and penalty invoicing.

The 17 repos to account for (verify against the actual workspace with `#codebase`; do not assume):
mms-cms-cef-r23, act-oh-OHDDI-main, mms-bolton-fin-ohio-main, Ohio.ETL, sis-inx-edigw-main,
ProcessManager-PMCustom-main, OH_FI_Reporting-main, act-OH-FIWebSvc-main,
finance-corticon-businessrules-main, mms-bolton-distmgr-main, LetterManager-main,
mms-cms-v360-r23, mms-bolton-fin-hist-ohio-main, etl-framework-main,
ProcessManager-Common-main, ProcessManager-*Dev2022-main, Baseline.ETLGateway.2016.ETL.

# GROUNDING RULES (non-negotiable)
1. Use `#codebase` semantic search + grep + file reads to find evidence. Search before you assert.
   Anchor searches on real code terms (proc names, table names, file names, X12 IDs), not paraphrases.
   The search anchors below are **hypotheses to verify**, not confirmed facts.
2. **Every factual claim must carry evidence** — a `file:line` reference for code/config, or a named
   non-repo source — and exactly one tag.
3. Tag **every** claim with exactly one of:
   - `[CONFIRMED]` — directly evidenced at a cited file:line.
   - `[INFERRED]` — reasoned from evidence but not explicitly stated in code; say what it's inferred from.
   - `[UNKNOWN]` — expected but not found in any repo. State where you looked. Do NOT fabricate a source.
4. Absence is a finding, not a blank. If something expected (orchestration, SQL Agent jobs, schedules,
   external exports, host data) is not present, say so explicitly and list it as an Open Question.
5. Never invent file paths, line numbers, proc names, table names, or hosts. If unsure, tag `[UNKNOWN]`.
6. Distinguish COTS runtime (QNXT/HPA/Administrator in mms-cms-cef) from Ohio custom code.

# TASK
Trace the execution path end-to-end across the repos and produce the document below. Identify entry points, the call chain, the data model touched, per-repo components, error-handling robustness, and code-level findings (with severity). Give a participation verdict for **all 17 repos** — including the ones with no role in this process.

Flag the non-idempotent CapVoucher INSERT (no upsert/`MERGE` guard) and missing transaction wrapping as HIGH; note the APM blind spot (capitation has no Dynatrace service entity).

Anchor your searches on terms like (verify, don't assert): `InsertCapVoucher`, `CapVoucher`, `PostCap_SplitPayment`, `FAAttributesCAP`, `820`, `rate cell`, `Corticon`, `getCapitation`, `Flexi`, `MERGE`, `command timeout`, `BEGIN TRAN`.

# REQUIRED OUTPUT — single self-contained `.html` file
Filename: `03_DotNetEngineer_Capitation_Analysis.html`  ·  `<h1>`: "Part 3 — .NET Engineer Analysis: Capitation (All 17 Repos)"
Produce these sections, in this order:

- **Header meta block**: Engagement, Role, Scope, Date, Grounding statement (claims evidenced to file:line; tagged).
- **Executive Summary** — prose: which repos own which part of the cycle, in one paragraph.
- **1. Entry Points & Orchestration** — where the run is triggered. If no .NET `Main()`/Windows Service entry
  point triggers it, say so and mark the orchestration `[INFERRED]`/`[UNKNOWN]` with evidence.
- **2. Call Chain** — the evidenced execution sequence. Include **Figure 1: a hand-authored inline `<svg>`
  call-chain diagram** (see DIAGRAMS).
- **3/4. Data Model** — tables and cross-DB relationships touched (group by database: Plandata_Parallel,
  HealthPAS_Common, CA_Extensions, QCSIDB, ETLStageDB). Include a **second hand-authored inline `<svg>`** ER
  diagram, DBs color-coded, with FK / reads edges and a legend.
- **5. Key Types & Stored Procedures Per Repo** — one `<h3>` + table per participating repo
  (proc/component | purpose | database | evidence file:line).
- **6. Robustness & Error Handling** — table: pattern | evidence | assessment (call out HIGH RISK items).
- **7. Evidence Table** — table: claim | file:line | tag. This is the master citation list.
- **8. Open Questions (Require SME)** — ordered list of `[UNKNOWN]`/`[INFERRED]` items needing SME input.
- **9. Code-Level Findings** — table: finding | severity (HIGH/MEDIUM/LOW) | evidence. Focus on transaction
  safety, idempotency / re-run risk, hardcoded dates/magic numbers, fragile config parsing, missing SSIS error
  handlers, duplicate packages, batch-vs-OLTP write contention.
- **10. Repo Participation Matrix** — table: repo | role in this process | artifact type. List **all 17**;
  highlight participating repos; mark non-participating ones with their role as "—".
- Footer line: "Generated by GitHub Copilot — Gainwell Ohio CEF Architecture Assessment — <date>".

# DIAGRAMS
- Use **raw inline `<svg>`**, hand-authored. **Do NOT use Mermaid** (it truncates past the node cap).
- Boxes = components, colored by tier; diamonds = decisions; dashed boxes = loops/boundaries; labeled arrows = flow.
- Each diagram gets a `<div class="diagram-container">` with a `<div class="diagram-title">` and a legend.
- Colors map tiers consistently: COTS engine, post-cap/custom SQL procs, data inserts, SSIS/ETL, Corticon rules, Flexi integration, EDI gateway — distinct fills, restated in the legend.

# STYLING (embed in `<style>`)
Segoe UI/Arial body, 11pt. Indigo headings: h1 `#1a237e` (3px bottom border), h2 `#283593`, h3 `#3949ab`.
Tables: collapsed borders, header row `#e8eaf6`, zebra even rows `#f5f5f5`. Monospace `code`/`pre` on `#f5f5f5`.
Tag pill classes: `.tag-confirmed` green `#2e7d32`, `.tag-inferred` blue `#1565c0`, `.tag-unknown` orange `#e65100`.
Severity classes: `.severity-high` red `#c62828`, `.severity-medium` `#e65100`, `.severity-low` `#2e7d32`.
Include `@media print` rules to avoid page breaks inside tables/diagrams.

# SELF-CHECK BEFORE FINISHING
- Does every claim have evidence (file:line or named source) and a tag? Any untagged sentence is a defect — fix it.
- Are all 17 repos in the participation matrix?
- Are `[UNKNOWN]`s honest (you actually searched) rather than lazy?
- Is the HTML a single self-contained file that renders offline (no external CSS/JS/fonts required)?
