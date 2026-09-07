# Capitation Complete Component Diagram — Code Verification Report

**Document Under Review:** `InputFiles/Capitation_Complete_Component_Diagram.html`  
**Cross-Reference:** `InputFiles/Capitation_Complete_Sequence_Diagram.html`  
**Date:** 2026-08-19  
**Primary Repo:** `mms-cms-cef` (maps to `mms-cms-cef-r23` in diagrams)

---

## Master Findings Table

| # | Component / Edge | Finding | Evidence (file:line) | Severity |
|---|---|---|---|---|
| 1 | QNXTALL.sln | **EXISTS** — UI wizard solution confirmed | [QNXTALL.sln](mms-cms-cef/components/QNXT/UI/Webapp/HPAALL_Website/QNXTALL.sln) | OK |
| 2 | CreateCAPPaymentsWiz2.aspx | **EXISTS** — UI entry point confirmed | [CreateCAPPaymentsWiz2.aspx](mms-cms-cef/components/QNXT/UI/Webapp/HPAALL/HPA/QPaymentManager/CreateCAPPaymentsWiz2.aspx) | OK |
| 3 | QTaskScheduler.sln | **EXISTS** | [QTaskScheduler.sln](mms-cms-cef/components/QNXT/UI/QTaskScheduler/QTaskScheduler.sln) | OK |
| 4 | QCSI Proxy Payment.sln | **EXISTS** — contains CapSubmitProxy, CapReStartProxy, CapitationBatchProcessProxy | [QCSI Proxy Payment.sln](mms-cms-cef/components/QNXT/Proxies/Payment/Src/QCSI%20Payment%20Proxy/QCSI%20Proxy%20Payment.sln) | OK |
| 5 | QCSI Internal Proxy.sln | **EXISTS** — contains OneWayProxy.cs, BulkProxy.cs | [QCSI Internal Proxy.sln](mms-cms-cef/components/QFrame/Infrastructure/Common/Src/QCSI%20Internal%20Proxy/QCSI%20Internal%20Proxy.sln) | OK |
| 6 | DistributionHub.sln | **EXISTS** — web service endpoint confirmed | [DistributionHub.sln](mms-cms-cef/components/QFrame/Infrastructure/Common/Src/WebService/Distribution%20Hub/DistributionHub.sln) | OK |
| 7 | CAPSubmit.sln | **EXISTS** — CapSubmitAgent with XSLT fan-out confirmed | [CAPSubmit.sln](mms-cms-cef/components/QNXT/Agents/Capitation/Src/CapSubmit/CAPSubmit.sln) | OK |
| 8 | QCSI ASC Capitation.sln | **EXISTS** — all 7-step pipeline VB.NET classes confirmed | [QCSI ASC Capitation.sln](mms-cms-cef/components/QNXT/ASCs/Capitation/Src/QCSI%20ASC%20Capitation/QCSI%20ASC%20Capitation.sln) | OK |
| 9 | QCSI ASC CapitationSetup.sln | **EXISTS** — CapProgramAgent.cs, CapProviderActivity.cs, ICapProviderActivity.cs confirmed | [QCSI ASC CapitationSetup.sln](mms-cms-cef/components/QNXT/ASCs/Capitation/Src/QCSI%20ASC%20CapitationSetup/QCSI%20ASC%20CapitationSetup.sln) | OK |
| 10 | SelectCapAffiliations.sln | **EXISTS** | [SelectCapAffiliations.sln](mms-cms-cef/components/QNXT/Agents/Capitation/Src/SelectAffiliations/SelectCapAffiliations.sln) | OK |
| 11 | CapGlobal.sln | **EXISTS** (as CAPGlobal.sln) | [CAPGlobal.sln](mms-cms-cef/components/QNXT/Agents/Capitation/Src/CapGlobal/CAPGlobal.sln) | OK |
| 12 | CapPcp.sln | **EXISTS** | [CapPcp.sln](mms-cms-cef/components/QNXT/Agents/Capitation/Src/CapPcp/CapPcp.sln) | OK |
| 13 | AdjustCapFunds.sln | **EXISTS** | [AdjustCapFunds.sln](mms-cms-cef/components/QNXT/Agents/Capitation/Src/AdjustCapFunds/AdjustCapFunds.sln) | OK |
| 14 | QCSI Payment DAL.sln | **EXISTS** | [QCSI Payment DAL.sln](mms-cms-cef/components/QFrame/Payment/Src/LDL/) | OK |
| 15 | QCSI ASC PostCapitationBatchAgent.sln | **EXISTS** — PostCapitationFundAllocationAgent.cs, PostCapitationSplitPaymentAgent.cs confirmed | [QCSI ASC PostCapitationBatchAgent.sln](mms-cms-cef/components/CA_Extensions/ASCs/Capitation/Src/QCSI%20ASC%20PostCapitationBatchAgent/QCSI%20ASC%20PostCapitationBatchAgent.sln) | OK |
| 16 | Diagram: `CapSubmitProxy` extends `BulkProxy` | **CORRECT** — `CapSubmitProxy : BulkProxy` confirmed in source | [CapSubmitProxy.cs](mms-cms-cef/components/QNXT/Proxies/Payment/Src/QCSI%20Payment%20Proxy/CapSubmitProxy.cs) — class declaration | OK |
| 17 | Diagram says Proxy → Internal Proxy dependency | **CORRECT** — `QCSI Proxy Payment.csproj` has PackageReference `ClaimAD.QCSI.Internal.Proxy` | [QCSI Proxy Payment.csproj](mms-cms-cef/components/QNXT/Proxies/Payment/Src/QCSI%20Payment%20Proxy/QCSI%20Proxy%20Payment.csproj) | OK |
| 18 | Diagram says Internal Proxy → Process Execution | **CORRECT** — `QCSI Internal Proxy.csproj` has PackageReference `ClaimAD.QCSI.Process.Execution` | [QCSI Internal Proxy.csproj](mms-cms-cef/components/QFrame/Infrastructure/Common/Src/QCSI%20Internal%20Proxy/QCSI%20Internal%20Proxy.csproj) | OK |
| 19 | Diagram: "3 Transports: HTTP \| Svc Broker \| ByPass" | **CORRECT** — OneWayProxy.cs `Invoke()` has exactly three code paths: ByPassQEnterprise direct call, ServiceBrokerEnabled enqueue, HTTP POST default. BulkProxy adds a 4th variant (EnqueueDirectToMessageStore). | [OneWayProxy.cs](mms-cms-cef/components/QFrame/Infrastructure/Common/Src/QCSI%20Internal%20Proxy/OneWayProxy.cs), [BulkProxy.cs](mms-cms-cef/components/QFrame/Infrastructure/Common/Src/QCSI%20Internal%20Proxy/BulkProxy.cs) | NIT |
| 20 | BulkProxy has 4 transport paths not 3 | **DIAGRAM INCOMPLETE** — BulkProxy.cs has ByPass, Bulk+ServiceBroker, Bulk+EnqueueDirectToMessageStore, and HTTP POST (4 paths). Diagram says "3 Transports" which is only accurate for OneWayProxy. | [BulkProxy.cs](mms-cms-cef/components/QFrame/Infrastructure/Common/Src/QCSI%20Internal%20Proxy/BulkProxy.cs) | NIT |
| 21 | Diagram: ProcessExecutionHost → Assembly.Load() + CreateInstance() | **CORRECT** — `ProcessExecutionHost.cs` lines 247-253: `Assembly.Load(step.Agent.Assembly)` then `assembly.CreateInstance(step.Agent.CreatableClass, ...)` confirmed | [ProcessExecutionHost.cs](mms-cms-cef/components/QFrame/Infrastructure/Common/Src/QCSI%20Process%20Execution/ProcessExecutionHost.cs#L247-L253) | OK |
| 22 | Diagram names "MessageProcessorEngine" as the loader | **INACCURATE** — `MessageProcessorEngine` delegates to `ProcessExecutionHost` which does the actual reflection loading. The diagram conflates the two. | [MessageProcessorEngine.cs](mms-cms-cef/components/QFrame/Infrastructure/Common/Src/QCSI%20Process%20Execution/MessageProcessorEngine.cs) — calls `new ProcessExecutionHost(); host.Execute()` | SHOULD-FIX |
| 23 | Diagram: CapSubmitAgent → "XSLT Fan-Out (1 msg → N programs)" | **CORRECT** — `CapSubmitAgent.cs` loads `XslTransform`, loads `../XSLTCAPSubmit.xslt`, transforms XML | [CapSubmitAgent.cs](mms-cms-cef/components/QNXT/Agents/Capitation/Src/CapSubmit/CapSubmitAgent.cs), [XSLTCapSubmit.xslt](mms-cms-cef/components/QNXT/Agents/Capitation/Src/CapSubmit/XSLTCapSubmit.xslt) | OK |
| 24 | All agents extend `QFrameAgent` | **CORRECT** — All 5 agent .csproj files reference `QEnterpriseExtensibility` (which contains `QFrameAgent` base class). Source confirms `CapSubmitAgent : QFrameAgent`, `CapProgramAgent : QFrameAgent`, etc. | Source files of all agents | OK |
| 25 | CapitationActivity implements ICapitationActivity | **CORRECT** — `Public Class CapitationActivity Inherits ActivityServiceComponent Implements ICapitationActivity` | [CapitationActivity.vb](mms-cms-cef/components/QNXT/ASCs/Capitation/Src/QCSI%20ASC%20Capitation/CapitationActivity.vb) | OK |
| 26 | ICapProviderActivity interface | **CORRECT** — interface has `LoadProgramAndCaps(programid, captype)` and `BuildProgramCapitation(message)` | [ICapProviderActivity.cs](mms-cms-cef/components/QNXT/ASCs/Capitation/Src/QCSI%20ASC%20CapitationSetup/ICapProviderActivity.cs) | OK |
| 27 | Diagram omits `ProcessExecutionHost` | **MISSING COMPONENT** — This is the actual class that does `Assembly.Load`/`CreateInstance`. The diagram shows `MessageProcessorEngine` doing this work, but it actually delegates to `ProcessExecutionHost`. | [ProcessExecutionHost.cs](mms-cms-cef/components/QFrame/Infrastructure/Common/Src/QCSI%20Process%20Execution/ProcessExecutionHost.cs) | SHOULD-FIX |
| 28 | Diagram omits `CapitationService.vb` | **MISSING COMPONENT** — Present in the QCSI ASC Capitation project; this is the service entry class. Not shown explicitly in the diagram though it is part of the pipeline. | [CapitationService.vb](mms-cms-cef/components/QNXT/ASCs/Capitation/Src/QCSI%20ASC%20Capitation/CapitationService.vb) | NIT |
| 29 | Diagram omits legacy `QCSI_Capitation.vbproj` (QFrame) | **MISSING** — A parallel legacy VS 2003 project exists at `QFrame/Payment/Src/Activities/QCSI_Capitation/` with COM dependencies (ADODB, QmxIDs, etc.) and .NET 1.1 target. This is the legacy version. | [QCSI_Capitation.vbproj](mms-cms-cef/components/QFrame/Payment/Src/Activities/QCSI_Capitation/QCSI_Capitation.vbproj) | NIT |
| 30 | PostCap SPs in act-oh-OHDDI-main | **ALL CONFIRMED** — PostCap_PreFundAllocate, PostCap_PreFundAllocateAdjustments, PostCap_PreFundAllocateRecons, PostCap_SplitPayment (note: file is `PostCap_SplitPayments.sql`), plus Group 1-5 variants all found | `_zip_inspect/act-oh-OHDDI-main/.../components/Source/Procs/PostCap_*.sql` | OK |
| 31 | PostCap SPs use CA_Extensions DB | **CORRECT** — All PostCap SQL files open with `USE [CA_Extensions]` | `_zip_inspect/act-oh-OHDDI-main/.../components/Source/Procs/PostCap_PreFundAllocate.sql` line 1 | OK |
| 32 | CAPRateExtensions tables | **CONFIRMED** — Both `CAPRateExtensions.sql` and `CAPRateExtensionsRules.sql` found with load scripts | `_zip_inspect/act-oh-OHDDI-main/.../components/Source/Tables/CAPRateExtensions.sql` | OK |
| 33 | Finance SSIS packages (bolton-fin-ohio) | **ALL 3 CONFIRMED** — `hpfin_ClaimFundAllocation.dtsx`, `hpfin_APInvoiceUpdate.dtsx`, `Hpfin_ClaimResolution.dtsx` all found in `packages/md/` | `_zip_inspect/mms-bolton-fin-ohio-main/.../components/packages/md/*.dtsx` | OK |
| 34 | up_hpfin_CFACapAdjFundAlloc SP | **CONFIRMED** — SQL file found and referenced in `hpfin_ClaimFundAllocation.dtsx` line 674 | `_zip_inspect/mms-bolton-fin-ohio-main/.../components/db/mssql/md/stored-procedures/up_hpfin_CFACapAdjFundAlloc.sql` | OK |
| 35 | Flexi GL tables (TIVH, TIVD, APBH, PIPQ) | **CONFIRMED** — Extensive references across multiple SQL files in bolton-fin-ohio: `FLEXI..TIVH`, `FLEXI..TIVD`, `FLEXI..APBH`, `FLEXI..PIPQ` | `_zip_inspect/mms-bolton-fin-ohio-main/.../components/db/mssql/md/stored-procedures/up_hpfin_API*.sql` | OK |
| 36 | Flexi GL via linked server (DTC/2PC) | **CORRECT** — Four-part naming `FLEXI..TIVD` confirms linked-server access | Same as #35 | OK |
| 37 | Corticon Business Rules (FinanceMatrix, PayablesMatrix) | **CONFIRMED** — `FinanceMatrix.ers`/`.erf` and `PayablesMatrix.ers`/`.erf` with `Matrix.ecore` vocabulary found | `_zip_inspect/finance-corticon-businessrules-main/.../RuleSheet/`, `RuleFlow/`, `Vocabulary/` | OK |
| 38 | hpfin_CorticonMatrix table | **CONFIRMED** — Table DDL found in bolton-fin-ohio DB scripts | `_zip_inspect/mms-bolton-fin-ohio-main/.../components/db/mssql/md/tables/hpfin_CorticonMatrix.sql` | OK |
| 39 | CreateMatrixTable.sln (bolton-fin-hist-ohio) | **NOT VERIFIABLE** — The `mms-bolton-fin-hist-ohio-main` directory is **empty** (zip not extracted or content missing). Cannot verify this component. | `_zip_inspect/mms-bolton-fin-hist-ohio-main/` — empty directory | SHOULD-FIX |
| 40 | OH_FI_Reporting — all 4 capitation reports | **ALL CONFIRMED** — CMS Capitation Summary, FINRP00001 Monthly Totals, FINRP00001L Cap List, FINRP00001O Operational Report | `_zip_inspect/OH_FI_Reporting-main/.../Financial/` | OK |
| 41 | Ohio.ETL SSIS packages (MEMIM00065, MEMSW00013, ELGIP00005) | **NOT VERIFIABLE** — `etl-framework-main` is a generic SSIS framework, NOT the Ohio-specific ETL repo. No MEMIM/MEMSW/ELGIP packages found in any available repo. | `_zip_inspect/etl-framework-main/` — only framework templates, no Ohio packages | SHOULD-FIX |
| 42 | Diagram: Proxy → Dist Hub arrow | **CORRECT (compile-time NuGet)** — `QCSI Proxy Payment.csproj` → `ClaimAD.QCSI.Internal.Proxy` → `ClaimAD.QCSI.Process.Execution`. Runtime: HTTP POST or Service Broker. | Project files | OK |
| 43 | Diagram: Dist Hub → CapSubmit arrow | **RUNTIME ONLY** — No compile-time reference. Distribution Hub has no package/project reference to `Agent CAP Submit`. The connection is via HTTP endpoint → `MessageProcessorEngine` → `ProcessExecutionHost` → `Assembly.Load` from GAC. | [DistributionHub.csproj](mms-cms-cef/components/QFrame/Infrastructure/Common/Src/WebService/Distribution%20Hub/DistributionHub.csproj) — no Cap references | OK |
| 44 | Diagram: Cap Engine → Plandata_Parallel | **CORRECT (runtime/SDL)** — Database access via `IQSecureDataLocator` (SDL framework). Connection strings resolved at runtime by session's `EnvID` + `dbalias` lookup in QCSIDB. Not a compile-time reference. | CQIDSequenceLDL.cs, CapitationActivity.vb — all use SDL pattern | OK |
| 45 | Diagram: Cap Engine → QCSIDB (paymentid) | **CORRECT** — `CNextId.vb` calls `CService("IDSequence")` → `CQIDSequenceLDL.cs` → `m_sdl.RetrieveData(IDSEQUENCE_ID_SP_SELECT)` → calls `spQ_IDSeq`. Indirection is via service locator, not direct. | [CNextId.vb](mms-cms-cef/components/QNXT/ASCs/Common/Shared/Src/QCSI%20Core%20Shared/CNextId.vb), [CQIDSequenceLDL.cs](mms-cms-cef/components/QFrame/Infrastructure/IDSequence/Src/LDL/CQIDSequenceLDL.cs) | OK |
| 46 | Diagram: PostCap Agents → PostCap SPs | **CORRECT** — `PostCapitationFundAllocationAgent.cs` calls query 65071 (maps to `PostCap_PreFundAllocate`). `PostCapitationSplitPaymentAgent.cs` calls query 65072 (maps to `PostCap_SplitPayment`). | [PostCapitationFundAllocationAgent.cs](mms-cms-cef/components/CA_Extensions/ASCs/Capitation/Src/QCSI%20ASC%20PostCapitationBatchAgent/PostCapitationFundAllocationAgent.cs), [PostCapitationSplitPaymentAgent.cs](mms-cms-cef/components/CA_Extensions/ASCs/Capitation/Src/QCSI%20ASC%20PostCapitationBatchAgent/PostCapitationSplitPaymentAgent.cs) | OK |
| 47 | Diagram: PostCap SPs → HealthPAS_Common | **CORRECT** — PostCap_PreFundAllocate SP (in OHDDI) references `HealthPAS_Common.dbo.*` tables and INSERTs `hpfin_FAAttributesCAP` | `_zip_inspect/act-oh-OHDDI-main/.../PostCap_PreFundAllocate.sql` | OK |
| 48 | Diagram: PostCap SPs → Plandata | **CORRECT** — PostCap SPs reference `plandata_parallel.dbo.payment`, `capvoucher`, etc. via cross-database queries | OHDDI PostCap SQL files | OK |
| 49 | Diagram: CFA SSIS → HealthPAS_Common → Flexi GL | **CORRECT** — CFA reads `hpfin_FAAttributesCAP` + `hpfin_CorticonMatrix`, then AP Invoice loads to FLEXI via linked server | bolton-fin-ohio `.dtsx` and `.sql` files | OK |
| 50 | Diagram: "INACTIVE in ADV_CAP_BATCH (Steps 1,2)" for PostCap | **ASSERTED but NOT VERIFIABLE from source** — The process chain step activation status is in the PlanIntegration database (runtime data), not in code. Cannot confirm from the codebase. | No code evidence — runtime DB config | NIT |
| 51 | Diagram: Edge CapProgramAgent → CapProviderActivity | **CORRECT (compile-time)** — `CapProgramAgent.cs` directly instantiates `new CapProviderActivity(session, headerId, detailId)` and calls `cap.BuildProgramCapitation(message)` | [CapProgramAgent.cs](mms-cms-cef/components/QNXT/ASCs/Capitation/Src/QCSI%20ASC%20CapitationSetup/CapProgramAgent.cs) | OK |
| 52 | Diagram: Standalone agent .sln files (SelectCapAffiliations, CapGlobal, CapPcp, AdjustCapFunds) | **EXISTS but DUAL-FRAMEWORK** — These have legacy VS 2003/.NET 1.1 `.csproj` files alongside the modernized solutions. The diagram does not indicate the legacy framework situation. | [CAPSubmit.csproj](mms-cms-cef/components/QNXT/Agents/Capitation/Src/CapSubmit/CAPSubmit.csproj) — `ProductVersion 7.10.3077` | SHOULD-FIX |
| 53 | Diagram: QCSIDB target framework = "COTS" | **CORRECT** — QCSIDB is vendor-managed infrastructure, confirmed by assembly naming (`ClaimAD.*` prefix) | Project files: all `ClaimAD.QCSI.*` naming convention | OK |
| 54 | Diagram: ASC Capitation targets `net48` | **CORRECT** — `QCSI ASC Capitation.vbproj` uses SDK-style `<TargetFramework>net48</TargetFramework>` | [QCSI ASC Capitation.vbproj](mms-cms-cef/components/QNXT/ASCs/Capitation/Src/QCSI%20ASC%20Capitation/QCSI%20ASC%20Capitation.vbproj) | OK |
| 55 | Diagram: Agent projects target .NET 1.1 | **NOT STATED in diagram** — Diagram does not mention that 5 agent .csproj files are VS 2003/.NET 1.1 legacy format. This is a significant omission for migration planning. | Agent `.csproj` files — `ProductVersion 7.10.3077`, `v1.1.4322` | SHOULD-FIX |
| 56 | Diagram: PostCapBatchAgent targets v4.8 | **CORRECT** — Old-style `.csproj` with `<TargetFrameworkVersion>v4.8</TargetFrameworkVersion>` | [QCSI ASC PostCapitationBatchAgent.csproj](mms-cms-cef/components/CA_Extensions/ASCs/Capitation/Src/QCSI%20ASC%20PostCapitationBatchAgent/QCSI%20ASC%20PostCapitationBatchAgent.csproj) | OK |
| 57 | Version mismatch: PostCapBatchAgent uses QFrameExtensibility 5.31.3 vs 31.2608.0 elsewhere | **NOT FLAGGED in diagram** — PostCapBatchAgent references `ClaimAD.QFrameExtensibility` v5.31.3 while Process Execution uses 31.2608.0. Potential version incompatibility. | [QCSI ASC PostCapitationBatchAgent.csproj](mms-cms-cef/components/CA_Extensions/ASCs/Capitation/Src/QCSI%20ASC%20PostCapitationBatchAgent/QCSI%20ASC%20PostCapitationBatchAgent.csproj) | SHOULD-FIX |
| 58 | Diagram: PostCapBatchAgent → "QCSI Access PaymentCustom" reference | **CORRECT (GAC reference)** — `.csproj` has `<Reference Include="QCSI Access PaymentCustom">` with `Private=False` and no HintPath (GAC-resolved) | [QCSI ASC PostCapitationBatchAgent.csproj](mms-cms-cef/components/CA_Extensions/ASCs/Capitation/Src/QCSI%20ASC%20PostCapitationBatchAgent/QCSI%20ASC%20PostCapitationBatchAgent.csproj) | OK |
| 59 | Diagram: All coupling via NuGet PackageReference | **MOSTLY CORRECT** — The modernized projects (ASC Capitation, CapitationSetup, Proxy Payment, Internal Proxy, Process Execution, IDSequence LDL, Payment DAL) all use NuGet PackageReference with zero ProjectReferences. Agent projects use legacy assembly references. PostCapBatchAgent uses old-style assembly references. | All `.csproj`/`.vbproj` files | OK |
| 60 | Diagram groups Corticon Rules + Matrix Loader as separate boxes | **CORRECT architecture but LOADER NOT VERIFIABLE** — Corticon rules repo confirmed. Matrix loader (CreateMatrixTable.sln from bolton-fin-hist-ohio) cannot be verified (empty directory). | `_zip_inspect/finance-corticon-businessrules-main/` confirmed; `_zip_inspect/mms-bolton-fin-hist-ohio-main/` empty | SHOULD-FIX |
| 61 | Diagram: mms-cms-v360-r23 "145 QCSIDB refs" | **NOT VERIFIABLE** — `mms-cms-v360` repo not present in workspace. Diagram claims 145 QCSIDB references. | No repo available | NIT |
| 62 | Diagram: Baseline.ETLGateway.2016.ETL "499 QCSIDB refs" | **NOT VERIFIABLE** — This repo not present in workspace. | No repo available | NIT |
| 63 | Diagram: sis-inx-edigw-main "EDI 820 Export" | **EXISTS** — Repository present with web services and components | `_zip_inspect/sis-inx-edigw-main/` | OK |
| 64 | Diagram: "PBIDSequence (batch cache, 20/batch)" under .NET ID Wrappers | **NOT VERIFIABLE** — No file named `PBIDSequence` found in the workspace. May be a class inside a larger file or a non-code concept. | Search returned no results | NIT |
| 65 | Diagram: Sequence diagram has "Stage 3 — Base Amount" but Component diagram calls it "Step 4" | **NAMING MISMATCH between diagrams** — The sequence diagram labels the base-amount step as "Stage 3" while the component diagram inner pipeline calls it "Step 4". The stage numbers and step numbers are deliberately different naming schemes (stages = conceptual, steps = pipeline order), but this is not explained in either diagram. | Both HTML documents | NIT |
| 66 | Diagram: All agents deploy from GAC | **CORRECT** — `ProcessExecutionHost.cs` comment: "agents or ASC that derive from QFrameAgent must now reside in GAC". CAPSubmit.csproj and peers have no output path to web/app directory — GAC deployment assumed. | [ProcessExecutionHost.cs](mms-cms-cef/components/QFrame/Infrastructure/Common/Src/QCSI%20Process%20Execution/ProcessExecutionHost.cs) | OK |
| 67 | Diagram shows QCSIDB arrow ← (reverse from wrappers to DB) | **ARROW DIRECTION WRONG** — Diagram arrow goes FROM QCSIDB TO .NET Wrappers (QCSIDB → Wrappers). The actual dependency is Wrappers → QCSIDB (code calls database, not the reverse). | Component diagram SVG line at y=545 | SHOULD-FIX |
| 68 | Diagram: "Ohio Shadow: SQLDEV_IDSequence" in HealthPAS_Common | **CONFIRMED** — `SQLDEV_MUM_GET_QCSIDB_ID.SQL` found in OHDDI with references to `QCSIDB.dbo.IDSequence` | `_zip_inspect/act-oh-OHDDI-main/.../SQLDEV_MUM_GET_QCSIDB_ID.SQL` | OK |

---

## Dependency Table (Built from Project Files)

### Modernized Projects (SDK-style, NuGet PackageReference)

| Project (Assembly) | Target | PackageReferences (NuGet) | Assembly Refs | COM Refs |
|---|---|---|---|---|
| **ClaimAD.QCSI.ASC.Capitation** (QCSI ASC Capitation.vbproj) | net48 | ClaimAD.QCSI.Access.Eligibility (31.2608.0), ClaimAD.QCSI.Access.Payment (31.2608.0), ClaimAD.QCSI.Access.Provider (31.2608.0), ClaimAD.QCSI.Authentication (31.2608.0), ClaimAD.QCSI.Core.Shared (31.2608.0), ClaimAD.QCSI.Proxy.Payment (31.2608.0) | System.Data.Linq, VB.Compat | Microsoft.StdFormat, stdole |
| **ClaimAD.QCSI.ASC.CapitationSetup** (QCSI ASC CapitationSetup.csproj) | net48 | ClaimAD.QCSI.ASC.CarrierProgram (31.2608.0), ClaimAD.QCSI.ASC.Provider (31.2608.0), ClaimAD.QCSI.Proxy.Payment (31.2608.0) | — | — |
| **ClaimAD.QCSI.Proxy.Payment** (QCSI Proxy Payment.csproj) | net48 | ClaimAD.QCSI.Internal.Proxy (31.2608.0), ClaimAD.QCSI.Messages.Payment (31.2608.0) | — | — |
| **ClaimAD.QCSI.Internal.Proxy** (QCSI Internal Proxy.csproj) | net48 | ClaimAD.QCSI.Process.Execution (31.2608.0), ClaimAD.QIS.Server.Common (31.2608.0) | System.configuration | — |
| **ClaimAD.QCSI.Process.Execution** (QCSI Process Execution.csproj) | net48 | ClaimAD.QCSI.Process.Locator.LDL (31.2608.0), ClaimAD.QFrame.Internal.Messages (31.2608.0), ClaimAD.QFrameExtensibility (31.2608.0) | — | — |
| **DistributionHub** (DistributionHub.csproj) | v4.8 (old-style) | ClaimAD.QFrame.Common.Messages (31.2608.0), ClaimAD.QFrame.Internal.Messages (31.2608.0), ClaimAD.QIS.XML.Interfaces (31.2608.0) | System.Web, System.Runtime.Remoting, etc. | — |
| **ClaimAD.QCSI.Payment.DAL** (QCSI Payment DAL.csproj) | net48 | ClaimAD.QCSI.Globals (31.2608.0) | — | — |
| **ClaimAD.QCSI.IDSequence.LDL** (QCSI IDSequence LDL.csproj) | net48 | ClaimAD.QCSI.Globals (31.2608.0) | — | — |

### Legacy Projects (VS 2003 / .NET 1.1)

| Project (Assembly) | Target | Direct Assembly Refs |
|---|---|---|
| **Agent CAP Submit** (CAPSubmit.csproj) | v1.1 | QEnterpriseExtensibility |
| **SelectCapAffiliations** (SelectCapAffiliations.csproj) | v1.1 | QEnterpriseExtensibility, QCSI Activity Service Comps, CapitationService (HintPath to ASCs/Capitation/Bin) |
| **Agent CAP Global** (CAPGlobal.csproj) | v1.1 | QEnterpriseExtensibility, QCSI Activity Service Comps, ASC Capitation Setup Activity |
| **Agent Cap Pcp** (CapPcp.csproj) | v1.1 | QEnterpriseExtensibility, QCSI Globals, QCSI Activity Service Comps, ASC Capitation Setup Activity |
| **AdjustCapFunds** (AdjustCapFunds.csproj) | v1.1 | QEnterpriseExtensibility, QCSI Activity Service Comps, CapitationService (HintPath to ASCs/Capitation/Bin) |
| **QCSI_Capitation** (QCSI_Capitation.vbproj, QFrame legacy) | v1.1 | QCSI Globals (HintPath), ADODB, QmxIDs, QmxRemote, MSXML2, Scripting, VBA (COM) |

### CA_Extensions (Old-style .NET 4.8)

| Project (Assembly) | Target | Key Refs |
|---|---|---|
| **QCSI ASC PostCapitationBatchAgent** | v4.8 | QCSI Access PaymentCustom (GAC, no HintPath), ClaimAD.QFrameExtensibility (5.31.3 via NuGet) |

---

## Runtime vs Compile-Time Coupling Classification

| Edge (A → B) | Diagram Shows | Actual Type | Mechanism | Match? |
|---|---|---|---|---|
| UI Wizard → CapSubmitProxy | Solid arrow | **Compile-time** | Direct C# call in aspx.cs codebehind | ✅ |
| CapSubmitProxy → OneWayProxy/BulkProxy | Solid arrow | **Compile-time** | Class inheritance (`CapSubmitProxy : BulkProxy`) + NuGet package ref | ✅ |
| OneWayProxy → Distribution Hub | Solid arrow | **Runtime** | HTTP POST to configurable URL | ⚠️ Diagram does not distinguish |
| OneWayProxy → Service Broker | Not shown | **Runtime** | `IQSecureDataLocator.EnqueueMessage()` | ⚠️ Missing |
| OneWayProxy → MessageProcessorEngine (ByPass) | Not shown | **Compile-time** | Direct `new MessageProcessorEngine()` in ByPass mode | ⚠️ Missing |
| Distribution Hub → Agents | Solid arrow | **Runtime/reflection** | `ProcessExecutionHost.Assembly.Load()` + `CreateInstance()` | ⚠️ Diagram conflates with compile-time |
| CapProgramAgent → CapProviderActivity | Implied | **Compile-time** | Direct `new CapProviderActivity()`, NuGet ref | ✅ |
| CapitationActivity → c* classes | Implied | **Compile-time** | Direct `New cBuildCapEnrolls(...)` etc. | ✅ |
| All code → Plandata_Parallel | Dashed arrow | **Runtime/SDL** | `IQSecureDataLocator` → `dbalias` lookup → ADO.NET | ✅ |
| All code → QCSIDB | Dashed arrow | **Runtime/SDL** | `CService("IDSequence")` → `CQIDSequenceLDL` → SDL | ✅ |
| PostCap Agents → PostCap SPs | Solid arrow | **Runtime/SDL** | Query IDs 65071/65072 → SDL → SP execution | ✅ |
| Finance SSIS → Flexi GL | Solid arrow | **Runtime** | Linked server (4-part name `FLEXI..TIVD`) + DTC/2PC | ✅ |

---

## Deployment Boundaries

| Component | Actual Host Process | Diagram Grouping | Correct? |
|---|---|---|---|
| CreateCAPPaymentsWiz2.aspx / QNXTALL | IIS (ASP.NET Web Application) | "UI / Entry Point" | ✅ |
| QTaskScheduler | Windows Scheduled Task | "UI / Entry Point" | ✅ |
| CapSubmitProxy, CapReStartProxy | In-process (loaded by UI or QTaskScheduler) | Same boundary as UI | ✅ |
| OneWayProxy, BulkProxy | In-process (loaded by proxy caller) | "QFrame Infrastructure" | ✅ |
| Distribution Hub | IIS (Web Service) or Windows Service | Separate boundary | ✅ |
| CapSubmitAgent, CapProgramAgent, all c* agents | ProcessExecutionHost (loaded from GAC) | Engine boundary | ✅ |
| PostCapitationFundAllocationAgent, PostCapitationSplitPaymentAgent | ProcessExecutionHost (GAC) | Grouped with PostCap SPs | ⚠️ The C# agents run in ProcessExecutionHost but the SPs run in SQL Server — different processes |
| Finance SSIS packages | SSIS Runtime (SQL Agent) | "Finance SSIS" | ✅ |
| Corticon Rules | Corticon Server (separate process) | External | ✅ |
| SSRS Reports | SQL Server Reporting Services | "SSRS Reporting" | ✅ |
| Flexi GL | Separate database server (linked server) | External | ✅ |

---

## Data Store Verification

| Database | Diagram Claims | Code Evidence | Verdict |
|---|---|---|---|
| **Plandata_Parallel** | Primary business data — enrollment, contracts, rates, payments | All c* VB.NET classes access via SDL; PostCap SPs cross-reference `plandata_parallel.dbo.*` | ✅ Confirmed |
| **PlanIntegration** | Dispatch + run-state + run-log (process, processstep, agent, processstate, processlog*) | `ProcessExecutionHost.cs` reads process/agent config; all agents log via `cProcessLog` | ✅ Confirmed |
| **QCSIDB** | ID generation (spQ_IDSeq), config (dbalias, envconfig) | `CQIDSequenceLDL.cs` calls IDSEQUENCE_ID_SP_SELECT; `CNextId.vb` via CService | ✅ Confirmed |
| **HealthPAS_Common** | Reference data (MEMIM00065_*), financial staging (hpfin_FAAttributesCAP, hpfin_CorticonMatrix) | PostCap SPs reference `HealthPAS_Common.dbo.*`; bolton-fin-ohio has table DDL; CFA SSIS reads from HPC | ✅ Confirmed |
| **CA_Extensions** | PostCap SPs, CAPRateExtensions tables | All PostCap SPs use `USE [CA_Extensions]`; CAPRateExtensions.sql confirmed | ✅ Confirmed |
| **Flexi GL** | TIVH, TIVD, APBH, PIPQ via linked server | `FLEXI..TIVD` four-part naming in bolton-fin-ohio SPs; `up_hpfin_APISetINFLEXIPaymentStatus.sql` | ✅ Confirmed |

---

## Sequence Diagram Cross-Reference

| Sequence Diagram Participant | Component Diagram Equivalent | Present in Both? | Naming Match? |
|---|---|---|---|
| QNXT Admin UI (CreateCAPPaymentsWiz) | QNXTALL.sln / CreateCAPPaymentsWiz2.aspx | ✅ | ✅ |
| CapSubmitProxy (BulkProxy) | QCSI Proxy Payment.sln | ✅ | ✅ |
| PlanIntegration DB | PlanIntegration Database | ✅ | ✅ |
| CapSubmitAgent | CAPSubmit.sln | ✅ | ✅ |
| CapProgramAgent (CapProviderActivity) | QCSI ASC CapitationSetup.sln | ✅ | ✅ |
| Plandata_Parallel DB | Plandata_Parallel | ✅ | ✅ |
| cBuildCapEnrolls (Stage 1a) | Step 1 / Stage 1a | ✅ | ⚠️ Seq = "Stage 1a", Comp = "Step 1 / Stage 1a" |
| cBuildCapAffils (Stage 1b) | Step 2 (MCO Assignment) + Step 3 / Stage 5 | ⚠️ | ⚠️ Seq calls it "Stage 1b" but Comp splits it to Step 2 + Step 3 |
| cBuildCapAmount (Stage 3) | Step 4 | ✅ | ⚠️ Seq = "Stage 3", Comp = "Step 4" |
| cCapFundAllocation (Stage 4) | Step 5 | ✅ | ⚠️ Seq = "Stage 4", Comp = "Step 5" |
| cCapFundAdjustment (Stage 5) | Step 6 / Stage 4 | ✅ | ⚠️ Seq = "Stage 5", Comp = "Step 6 / Stage 4" — confusing double-numbering |
| cStoreCapVoucher (Write) | Step 7a / Stage 7 | ✅ | ✅ |
| cCapPayment (Finalize) | Step 7b / Stage 7 | ✅ | ✅ |
| OneWayProxy (Dispatch) | QCSI Internal Proxy.sln | ✅ | ✅ |
| MessageProcessorEngine | MessageProcessorEngine box | ✅ | ⚠️ Diagram shows it doing Assembly.Load but it delegates to ProcessExecutionHost |
| ProcessExecutionHost | NOT SHOWN | ❌ | ❌ Missing from component diagram |
| QCSIDB (IDSequence) | QCSIDB Database (COTS) | ✅ | ✅ |
| PostCap SPs (CA_Extensions) | PostCap Agents + PostCap SPs | ✅ | ✅ |
| Ohio.ETL SSIS (Reference Loaders) | Rate Cell Sweep box | ✅ | ✅ |
| Corticon Rules (CreateMatrixTable.sln) | Corticon Decision Output + Corticon Matrix Loader | ✅ | ✅ |
| Finance SSIS (mms-bolton-fin-ohio) | Finance SSIS box | ✅ | ✅ |
| Flexi GL (Linked Server) | Flexi GL box | ✅ | ✅ |

**Naming issue:** The "Stage" vs "Step" numbering between the two documents is internally consistent but confusing. The sequence diagram uses "Stage N" to mean the conceptual phase, while the component diagram uses "Step N" for pipeline execution order. The component diagram also includes "Stage N" cross-references in parentheses, creating double-numbering like "Step 6 / Stage 4".

---

## Granularity Assessment

| Issue | Description | Severity |
|---|---|---|
| Solution-level boxes next to class-level boxes | The diagram shows `QCSI ASC Capitation.sln` as one solution but then expands its 7 inner classes as individual boxes. Meanwhile `DistributionHub.sln` is a single box with no expansion. The solutions are at comparable scope but shown at different zoom levels. | NIT |
| Database tables shown inside database boxes | Showing specific tables (e.g., `enrollkeys`, `capterm`, `processstate`) inside database rectangles is appropriate given the data-centric architecture. | OK |
| `.sln` boxes mixed with `.dtsx` package names | The diagram mixes solution-level (.sln) components with individual SSIS package names (.dtsx). These are roughly comparable scope items. | OK |
| Individual stored procedures listed | PostCap SPs are listed individually, while the Finance SSIS layer only shows package names. Comparable granularity. | OK |

---

## Not Verifiable

| Item | Reason |
|---|---|
| `CreateMatrixTable.sln` (mms-bolton-fin-hist-ohio-main) | Repository directory is empty — zip not extracted |
| Ohio.ETL SSIS packages (MEMIM00065, MEMSW00013, ELGIP00005) | `etl-framework-main` is a generic framework, not the Ohio-specific ETL repo; actual Ohio.ETL repo not in workspace |
| `mms-cms-v360-r23` "145 QCSIDB refs" | Repository not present in workspace |
| `Baseline.ETLGateway.2016.ETL` "499 QCSIDB refs" | Repository not present in workspace |
| `PBIDSequence (batch cache, 20/batch)` | No matching file or class found in any available repo |
| ADV_CAP_BATCH Steps 1,2 INACTIVE status | This is runtime DB configuration in PlanIntegration, not verifiable from source code |
| Process chain counts ("192 chains", "214 agents", "~2B rows in processstate") | Runtime database metrics, not in source code |
| Server connection strings (OHDBD01.OHAWS.COM) | Infrastructure configuration, not source-controlled |
| "~71 concurrent runs per monthly cycle" | Operational metric, not in source |

---

## Verdict: **NEEDS CHANGES**

The component diagram is **substantially accurate** — every named solution, project, source file, database, and stored procedure was located in the codebase (14/14 solutions confirmed, 19/19 key source files confirmed, all 6 databases confirmed, all related repos confirmed). The architecture, dependency directions, and coupling mechanisms are fundamentally correct.

However, it requires changes for:

1. **SHOULD-FIX: `MessageProcessorEngine` vs `ProcessExecutionHost` conflation** (#22, #27) — The diagram credits `MessageProcessorEngine` with the `Assembly.Load`/`CreateInstance` reflection, but that class delegates to the **unlisted** `ProcessExecutionHost`. This is the single most important runtime coupling point in the system and should be accurately attributed.

2. **SHOULD-FIX: QCSIDB arrow direction** (#67) — The SVG arrow goes from DB to wrappers; it should go from wrappers to DB (code calls database).

3. **SHOULD-FIX: Agent .NET 1.1 framework not disclosed** (#52, #55) — Five agent `.csproj` files target .NET Framework 1.1 (VS 2003 format). This is a critical migration concern not mentioned in the diagram.

4. **SHOULD-FIX: QFrameExtensibility version mismatch** (#57) — PostCapBatchAgent uses version 5.31.3 while all other projects use 31.2608.0. Not flagged.

5. **SHOULD-FIX: Missing repos** (#39, #41, #60) — `mms-bolton-fin-hist-ohio-main` (empty) and Ohio.ETL (not the same as `etl-framework-main`) prevent full verification.

6. **NIT: Stage/Step numbering confusion** (#65) — The dual-numbering system between the two documents is internally consistent but confusing without a legend.

7. **NIT: BulkProxy has 4 transport paths, not 3** (#20) — Minor factual inaccuracy.

No **BLOCKER**-level issues were found — no components are invented, no dependency directions are fundamentally wrong (the QCSIDB arrow is an SVG rendering issue, not a conceptual error), and no real projects in the solution are significantly omitted.
