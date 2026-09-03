# EDI 820 Generation Analysis Prompt

# ROLE

Act as a **Senior .NET Engineer and Integration Architect** performing an independent, code-level trace of the **EDI 820 Payment Order/Remittance generation process**.

You are NOT writing or modifying code.

You are producing an **evidence-grounded architecture analysis document** based entirely on source code, SQL artifacts, SSIS packages, configurations, stored procedures, interfaces, and workflow implementations found in the workspace.

---

# CONTEXT

Engagement: Gainwell Ohio CEF Architecture Assessment (MSS).

The workspace contains the Ohio R23 repositories, including QNXT/HealthPAS integrations, financial processing, Edifecs EDI transformations, SSIS packages, Process Manager components, and supporting databases.

Business Process Under Analysis:

**EDI 820 Generation after payment reaches Paid status.**

Current understanding (to be VERIFIED, not assumed):

1. Payment status is updated to **Paid** in QNXT.
2. An operations user manually navigates to QNXT.
3. User clicks a button/process to generate outbound payment files.
4. QNXT generates an XML payload for each MCO/provider.
5. XML is handed off to Edifecs.
6. Edifecs converts XML to X12 820.
7. 820 is transmitted externally.

This entire flow must be validated with actual code evidence.

If any automated, scheduled, event-driven, batch-driven, API-driven, stored-procedure-driven, Process Manager-driven, ETL-driven, or Edifecs-driven mechanism exists, identify it and prove it with evidence.

---

# REPOSITORIES TO EVALUATE

Verify actual workspace contents using code search before making conclusions.

- mms-cms-cef-r23
- act-oh-OHDDI-main
- mms-bolton-fin-ohio-main
- Ohio.ETL
- sis-inx-edigw-main
- ProcessManager-PMCustom-main
- OH_FI_Reporting-main
- act-OH-FIWebSvc-main
- finance-corticon-businessrules-main
- mms-bolton-distmgr-main
- LetterManager-main
- mms-cms-v360-r23
- mms-bolton-fin-hist-ohio-main
- etl-framework-main
- ProcessManager-Common-main
- ProcessManager-*Dev2022-main
- Baseline.ETLGateway.2016.ETL

---

# INVESTIGATION OBJECTIVES

## 1. What event initiates EDI 820 generation?

Investigate whether generation begins from:

- Payment Status = Paid
- CapVoucher creation
- Check generation
- EFT generation
- Financial posting completion
- Manual QNXT action
- Scheduled batch
- SQL Agent Job
- Process Manager workflow
- SSIS Package
- Edifecs polling process
- File drop trigger
- Service bus/message queue
- Custom application

Do not assume.

Provide evidence.

## 2. Is manual intervention required?

Trace all UI flows.

Identify:

- Screens
- Menu options
- Buttons
- Controller actions
- Stored procedures
- Services called

Questions to answer:

- Does a user manually generate 820s?
- Does a user initiate a batch which later creates 820s?
- Can 820s be generated automatically without user intervention?
- Are there multiple generation paths?

## 3. Trace the End-to-End Call Chain

Starting from:

Payment Status = Paid

Trace every step until:

Final X12 820 file is produced and transmitted.

Document:

- Application components
- Services
- APIs
- Stored procedures
- SSIS packages
- Batch jobs
- File drops
- Interfaces
- Edifecs transformations

Include actual execution sequence.

## 4. Identify the XML Generation Logic

Investigate:

- XML creation code
- XML schemas
- XML templates
- XML serialization classes
- Stored procedures producing XML
- File export processes

Search terms include:

- XmlWriter
- XDocument
- XmlSerializer
- Create820
- PaymentXml
- RemittanceXml
- ExportXML
- GenerateXML
- EDIXML
- EFT XML
- FOR XML
- 820 XML

Determine:

- Which component owns XML generation.
- Where generated XML files are written.
- File naming conventions.
- Trigger mechanism.

## 5. Identify Edifecs Participation

Investigate:

- Edifecs interfaces
- Gateway components
- sis-inx-edigw repositories
- Maps
- Transformations
- Routing configurations
- File pickup locations

Determine:

- Does Edifecs receive XML?
- Does Edifecs receive database records?
- Does Edifecs create the X12 820?
- Is the transformation performed elsewhere?

## 6. Determine Automation vs Manual Processing

For every step classify as:

- MANUAL
- AUTOMATED
- SCHEDULED
- EVENT DRIVEN
- UNKNOWN

Provide evidence.

## 7. Financial Processing Dependencies

Investigate whether 820 generation depends on:

- CapVoucher records
- Voucher status
- EFT status
- Check status
- Payment batches
- Accounts payable processing
- Flexi integration
- Financial exports

Document all dependencies.

## 8. Database Investigation

Identify all tables involved.

Potential search anchors:

- 820
- Remittance
- EFT
- Voucher
- CapVoucher
- PaymentStatus
- PaymentBatch
- Paid
- Generate820
- EDI
- TradingPartner
- OutboundFile

For each table provide:

- Purpose
- Read/Write behavior
- Producing process
- Consuming process

## 9. Scheduling Investigation

Determine whether 820 generation is initiated through:

- SQL Agent Jobs
- Windows Services
- Scheduled Tasks
- Process Manager
- ETL Framework
- SSIS Packages
- Batch Executables

If not found, explicitly state where you searched.

---

# GROUNDING RULES (MANDATORY)

- Search before asserting.
- Every factual claim must have evidence.
- Every claim must contain exactly one tag:

### [CONFIRMED]

Directly supported by:

- file:line reference
- config reference
- SQL reference

### [INFERRED]

Reasoned from multiple pieces of evidence but not explicitly stated.

State reasoning.

### [UNKNOWN]

Expected but not found.

State:

- repositories searched
- search terms used

Never fabricate evidence.

---

# REQUIRED OUTPUT

Generate a single self-contained HTML report:

**Filename:** EDI820_Generation_Analysis.html

**Title:** EDI 820 Generation Analysis – Payment Status to X12 Transmission

Sections:

1. Executive Summary
2. Trigger Analysis
3. UI and Manual Operations Analysis
4. End-to-End Call Chain (with SVG diagram)
5. XML Generation Evidence
6. Edifecs Transformation Evidence
7. Database Objects
8. Automated Jobs and Batch Processes
9. Evidence Table
10. Alternative Flows
11. Open Questions for SMEs
12. Final Verdict

---

# SEARCH ANCHORS

820, EDI820, X12, Remittance, Payment Order, Trading Partner, Outbound, Generate820, Create820, Export820, CapVoucher, Voucher, EFT, CheckRun, Paid, PaymentStatus, Edifecs, sis-inx-edigw, XML, XmlWriter, XmlSerializer, FOR XML, ProcessManager, SSIS, SQL Agent, TradingPartner, Remittance Advice, Payment Export, Batch Export, Outbound File, FTA, Gateway, Translation Map

---

# SUCCESS CRITERIA

Determine whether the following statement is true, false, or partially true:

> After payment reaches Paid status, a user manually enters QNXT, triggers 820 generation, QNXT creates XML, and Edifecs converts that XML into an X12 820.

Every conclusion must be supported by code-level evidence with file:line references.
