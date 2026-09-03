# Capitation_SSRS_Reporting_DeepDive_Prompt

## ROLE

Act as a **Senior .NET Engineer, SSRS Architect, Data Architect, and Financial Reporting Analyst** performing an independent, code-level architecture trace of the Ohio CEF Capitation Reporting solution.

You are NOT writing code.

You are producing an evidence-grounded analysis document based exclusively on source code, SSRS artifacts, SQL objects, ETL components, configurations, and database objects discovered in the workspace.

This is a reporting-focused architecture analysis.

---

## CONTEXT

Engagement: Gainwell Ohio CEF Architecture Assessment (MSS).

Analyze the Capitation SSRS reporting suite, including:

- FINRP00001 – Capitation Cycle Totals
- FINRP00001L – Capitation List
- FINRP00001O – Operational Capitation Report (reported to supplement EDI 820 processing; verify from code)
- FINRP00020 – Monthly Counts & Detail

Also identify and analyze any additional Capitation-related reports discovered during investigation.

Environment may include:

- QNXT / HealthPAS
- SSRS
- SQL Server
- SSIS
- ETL Framework
- Flexi Financial Integration
- Edifecs EDI Integration
- Corticon Rules
- Ohio custom .NET applications

Do not assume any business purpose, data source, calculation, or dependency. Prove everything from code and report artifacts.

---

## REPOSITORIES TO ANALYZE

Verify actual workspace contents before making conclusions.

Potential repositories include:

- OH_FI_Reporting-main
- mms-bolton-fin-ohio-main
- mms-bolton-fin-hist-ohio-main
- Ohio.ETL
- etl-framework-main
- act-OH-FIWebSvc-main
- ProcessManager-PMCustom-main
- ProcessManager-Common-main
- finance-corticon-businessrules-main
- mms-cms-cef-r23
- mms-cms-v360-r23

Include additional repositories discovered during analysis.

---

## GROUNDING RULES (MANDATORY)

- Search before asserting.
- Every factual claim must have evidence.
- Every claim must contain exactly one tag.

### [CONFIRMED]

Directly supported by:
- RDL file
- Shared dataset
- Shared data source
- Stored procedure
- View
- Function
- Configuration
- SSIS package

Must include file:line evidence.

### [INFERRED]

Supported by evidence but not explicitly documented.
State reasoning.

### [UNKNOWN]

Expected but not found.
List:
- repositories searched
- search terms used
- artifacts reviewed

Never fabricate:
- report definitions
- datasets
- stored procedures
- views
- tables
- file names
- line numbers
- business logic

---

## INVESTIGATION OBJECTIVES

### 1. Report Discovery

Locate:
- FINRP00001
- FINRP00001L
- FINRP00001O
- FINRP00020
- Additional Capitation reports

Provide:
- Repository
- RDL path
- Folder location
- Shared datasets
- Shared data sources
- Evidence

### 2. Business Purpose Validation

Determine actual business purpose from report definitions and source code.

For each report identify:
- Intended audience
- Business process supported
- Financial significance
- Operational significance
- Relationship to Capitation lifecycle

### 3. Report Execution Architecture

Trace:

User
→ SSRS Report
→ Dataset
→ Stored Procedure
→ View
→ Table
→ Source Capitation Process

Document:
- Parameters
- Cascading parameters
- Default values
- Dataset mappings
- Execution dependencies

### 4. Dataset Analysis

For every dataset provide:
- Dataset name
- Report
- Query type
- Embedded/shared
- Stored procedure or query used
- Parameter mappings
- Evidence

### 5. Stored Procedure Trace

For each procedure document:
- Purpose
- Inputs
- Outputs
- Tables read
- Tables written
- Procedure call chain
- Business function

### 6. Data Lineage Analysis

Trace report fields to source columns.

Examples:
- Cycle Amount
- Total Capitation
- Total Payments
- Adjustment Amounts
- Provider Totals
- MCO Totals
- Member Counts
- Monthly Counts
- Remittance Values

Create lineage:

Report Field
→ Dataset Field
→ Procedure Column
→ View Column
→ Table Column

### 7. Relationship to Capitation Processing

Determine whether reports consume data from:
- CapVoucher
- PostCap_SplitPayment
- Fund Allocation
- FAAttributesCAP
- Rate Cell Processing
- Financial Posting
- Payment Generation
- Flexi Posting
- EDI 820 Generation

For every dependency prove:
- Direct use
- Indirect use
- Not used

### 8. Relationship to EDI 820 Generation

Special focus on FINRP00001O.

Determine:
- Does it supplement EDI 820 processing?
- Does it reconcile against 820 output?
- Does it use remittance data?
- Does it read EDI staging tables?
- Does it use trading partner information?

Provide evidence.

### 9. Database Dependency Inventory

Identify:
- Tables
- Views
- Functions
- Procedures
- Synonyms
- Temporary tables

Group by database.

### 10. SSRS Expression Analysis

Inspect:
- Expressions
- Running totals
- Visibility rules
- Conditional formatting
- Aggregates
- Custom code

Determine which calculations occur in SSRS versus SQL.

### 11. Security Analysis

Investigate:
- Folder security
- Report security
- Roles
- Permissions
- Parameter filtering

### 12. Performance Analysis

Evaluate:
- Large scans
- Heavy aggregations
- Temp table usage
- Nested procedure calls
- Repeated dataset execution
- Missing filters

---

## REQUIRED OUTPUT

Filename:

Capitation_SSRS_Reporting_DeepDive.html

Title:

Part X – SSRS Reporting Analysis: Capitation Reporting Suite
(FINRP00001, FINRP00001L, FINRP00001O, FINRP00020)

---

## REQUIRED SECTIONS

1. Executive Summary
2. Report Inventory
3. Report Architecture
4. FINRP00001 Analysis
5. FINRP00001L Analysis
6. FINRP00001O Analysis
7. FINRP00020 Analysis
8. Dataset Matrix
9. Data Lineage Matrix
10. Database Dependency Diagram (SVG)
11. Report-to-Capitation Process Mapping
12. Code-Level Findings
13. Evidence Table
14. Open Questions
15. Repo Participation Matrix
16. Final Verdict

---

## FINAL VERDICT MUST ANSWER

1. What does FINRP00001 actually report?
2. What does FINRP00001L actually report?
3. What does FINRP00001O actually report?
4. Does FINRP00001O support EDI 820 reconciliation?
5. What does FINRP00020 actually report?
6. Which stored procedures drive each report?
7. Which tables are authoritative sources?
8. Which reports use CapVoucher data?
9. Which reports use payment/remittance data?
10. What code evidence proves each conclusion?

---

## SUCCESS CRITERIA

Trace:

Report
→ Dataset
→ Stored Procedure
→ View
→ Table
→ Capitation Business Process

Every conclusion must contain file:line evidence. Any unsupported statement must be tagged [INFERRED] or [UNKNOWN]. Do not rely on report names, SME statements, or assumptions.