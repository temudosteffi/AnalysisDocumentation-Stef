# 36-Month Summary File Extract - MCP Capitation History

## ROLE

Act as a **Senior .NET Engineer, Data Architect, Financial Reporting Analyst, and ETL Architect** performing an independent, code-level architecture trace of the Ohio CEF process that generates the **36-Month Summary File Extract – MCP Capitation History**.

You are NOT writing code.

You are producing an evidence-grounded analysis document based exclusively on source code, SQL objects, SSRS artifacts, ETL packages, configuration files, stored procedures, database objects, and report definitions found in the workspace.

---

## CONTEXT

Engagement: Gainwell Ohio CEF Architecture Assessment (MSS).

Process Under Analysis:

**36-Month Summary File Extract – MCP Capitation History**

Known business description (to be VERIFIED from code, not assumed):

The extract reportedly contains:

- Amount Paid
- Days Paid
- Member Months
- MCP Code
- Provider ID
- Service Location
- County Group
- Rate Cell
- Service Month

Covering approximately 36 months of capitation history.

Every aspect of this description must be validated through source code, SQL, ETL, report definitions, or configuration evidence.

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
- mms-cms-cef-r23
- mms-cms-v360-r23
- Baseline.ETLGateway.2016.ETL

Include any repository discovered during analysis.

---

## GROUNDING RULES (MANDATORY)

Search before asserting.

Every factual claim must have evidence.

Every claim must contain exactly one tag:

### [CONFIRMED]
Directly supported by file:line evidence.

### [INFERRED]
Reasoned from evidence. Explain the reasoning.

### [UNKNOWN]
Not found. State where you looked and the search terms used.

Never fabricate:

- file names
- line numbers
- procedures
- views
- tables
- datasets
- package names
- business rules

---

## INVESTIGATION OBJECTIVES

### 1. Locate the Extract Implementation

Determine:

- Where the extract is generated.
- Whether it is SSRS, ETL, SQL, .NET, or batch driven.
- Whether multiple implementations exist.

Search anchors:

36 Month
36-Month
Summary File Extract
Capitation History
Member Months
Amount Paid
Days Paid
County Group
Rate Cell
MCP

Provide evidence.

### 2. Identify the Entry Point

Determine how the extract is created:

- Manual execution
- SSRS execution
- Scheduled job
- ETL process
- Process Manager workflow
- SQL Agent Job
- Batch executable

Trace the starting point.

### 3. End-to-End Execution Flow

Trace:

User/Job
→ Application or ETL
→ Dataset
→ Stored Procedure
→ Views
→ Tables
→ Extract File

Provide a complete call chain.

### 4. Extract Layout Validation

For every output column determine:

- Source field
- Source table
- Source view
- Source procedure
- Transformation logic

Validate:

- Amount Paid
- Days Paid
- Member Months
- MCP Code
- Provider ID
- Service Location
- County Group
- Rate Cell
- Service Month

Do not assume the field meanings.

### 5. Data Lineage Analysis

For each output attribute trace:

Output Field
→ Dataset Field
→ Procedure/View
→ Source Table Column

Provide evidence.

### 6. Business Logic Analysis

Determine how:

- Amount Paid is calculated.
- Days Paid is calculated.
- Member Months is calculated.
- County Group is derived.
- Rate Cell is derived.
- MCP Code is populated.

Identify whether calculations occur in:

- SQL
- ETL
- SSRS
- .NET code

### 7. Relationship to Capitation Processing

Determine whether the extract depends upon:

- CapVoucher
- Fund Allocation
- PostCap_SplitPayment
- Rate Cell Processing
- Monthly Capitation Runs
- Payment Generation
- EDI 820 Processing
- Flexi Posting

Provide evidence.

### 8. Database Dependency Inventory

Identify all:

- Tables
- Views
- Functions
- Procedures
- Synonyms
- Temporary tables

Group by source database.

### 9. Historical Data Retention

Determine:

- Why 36 months are selected.
- Whether the period is configurable.
- How date filtering is implemented.
- Whether retention logic exists.

Provide evidence.

### 10. Output File Generation

Determine:

- File format.
- Naming convention.
- Export location.
- Schedule.
- Consumer of the extract.

Provide evidence.

---

## REQUIRED OUTPUT

Filename:

36_Month_Summary_File_Extract_MCP_Capitation_History_Analysis.html

Title:

Part X – Analysis: 36-Month Summary File Extract – MCP Capitation History

---

## REQUIRED SECTIONS

1. Executive Summary
2. Extract Discovery
3. Entry Point & Orchestration
4. End-to-End Call Chain
5. Extract Layout Analysis
6. Data Lineage Matrix
7. Database Dependency Analysis
8. Business Logic Analysis
9. Historical Retention Logic
10. Output File Generation Process
11. Code-Level Findings
12. Evidence Table
13. Open Questions
14. Repo Participation Matrix
15. Final Verdict

---

## FINAL VERDICT MUST ANSWER

1. What component generates the extract?
2. What is the true purpose of the extract?
3. Which procedures and tables drive it?
4. How are Amount Paid, Days Paid, and Member Months calculated?
5. How are County Group and Rate Cell derived?
6. Is the 36-month period configurable?
7. Who consumes the extract?
8. What code evidence proves each conclusion?

---

## SUCCESS CRITERIA

Trace:

Extract
→ Dataset/Procedure
→ Views
→ Tables
→ Capitation Business Process

Every conclusion must have file:line evidence. Any unsupported statement must be tagged [INFERRED] or [UNKNOWN]. Do not rely on report names, SME comments, or assumptions.