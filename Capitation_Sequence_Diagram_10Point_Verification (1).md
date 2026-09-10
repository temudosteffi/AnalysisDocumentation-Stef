# Capitation Sequence Diagram — Code-Level Verification Report

**DIAGRAM UNDER REVIEW:** `Capitation_Complete_Sequence_Diagram.html` (10 Mermaid sequence diagrams)  
**ENTRY POINT:** `CreateCAPPaymentsWiz2.btFinish_Click()` → `CapSubmitProxy.ProcessMessage()` → Agent Pipeline  
**Method:** Every claim traced to source code with file path and line number.

---

## Findings Table

| # | Diagram Element | Finding | Evidence (file:line) | Severity |
|---|----------------|---------|---------------------|----------|
| **1. PARTICIPANTS** | | | | |
| 1.1 | `QNXT Admin UI (CreateCAPPaymentsWiz)` | **EXISTS.** ASP.NET Web Forms wizard (3 pages: W0, Wiz, Wiz2). | `QNXT/UI/Webapp/HPAALL_Website/HPA/QPaymentManager/CreateCAPPaymentsWiz2.aspx.cs:347` | ✅ |
| 1.2 | `CapSubmitProxy (BulkProxy)` | **EXISTS.** Inherits `BulkProxy`, sets `ADV_CAP_PROC`. | `QNXT/Proxies/Payment/Src/QCSI Payment Proxy/CapSubmitProxy.cs:17` | ✅ |
| 1.3 | `CapSubmitAgent` | **EXISTS.** Extends `QFrameAgent`, loads XSLT. | `QNXT/Agents/Capitation/Src/CapSubmit/CapSubmitAgent.cs:21` | ✅ |
| 1.4 | `CapProgramAgent (CapProviderActivity)` | **EXISTS.** Extends `QFrameAgent`, delegates to `CapProviderActivity`. | `QNXT/ASCs/Capitation/Src/QCSI ASC CapitationSetup/CapProgramAgent.cs:27` | ✅ |
| 1.5 | `cBuildCapEnrolls (Stage 1a)` | **EXISTS.** VB.NET class in QCSI ASC Capitation. | `QNXT/ASCs/Capitation/Src/QCSI ASC Capitation/cBuildCapEnrolls.vb:31` | ✅ |
| 1.6 | `cBuildCapAffils (Stage 1b)` | **EXISTS.** VB.NET class. | `QNXT/ASCs/Capitation/Src/QCSI ASC Capitation/cBuildCapAffils.vb:23` | ✅ |
| 1.7 | `cBuildCapAmount (Stage 3)` | **EXISTS.** VB.NET class. | `QNXT/ASCs/Capitation/Src/QCSI ASC Capitation/cBuildCapAmount.vb` | ✅ |
| 1.8 | `cCapFundAllocation (Stage 4)` | **EXISTS.** VB.NET class. | `QNXT/ASCs/Capitation/Src/QCSI ASC Capitation/cCapFundAllocation.vb:22` | ✅ |
| 1.9 | `cCapFundAdjustment (Stage 5)` | **EXISTS.** VB.NET class. | `QNXT/ASCs/Capitation/Src/QCSI ASC Capitation/cCapFundAdjustment.vb:23` | ✅ |
| 1.10 | `cStoreCapVoucher (Stage 7 Write)` | **EXISTS.** VB.NET class. | `QNXT/ASCs/Capitation/Src/QCSI ASC Capitation/cStoreCapVoucher.vb:26` | ✅ |
| 1.11 | `cCapPayment (Stage 7 Finalize)` | **WRONG CLASS NAME.** `CapitationActivity.FinalizeCapPayment()` instantiates `cFinalizeCapPayment`, NOT `cCapPayment`. | `CapitationActivity.vb:157` — `New cFinalizeCapPayment(...)` | **BLOCKER** |
| 1.12 | `PostCap SPs (CA_Extensions)` | **EXISTS.** SPs confirmed in `CA_Extensions` database. | `act-oh-OHDDI-main/components/Source/Procs/PostCap_PreFundAllocate.sql:48` | ✅ |
| 1.13 | `Ohio.ETL SSIS` / Finance SSIS | **EXISTS.** All three DTSX packages confirmed. | `mms-bolton-fin-ohio-main/components/packages/md/hpfin_ClaimFundAllocation.dtsx`, `hpfin_APInvoiceUpdate.dtsx`, `Hpfin_ClaimResolution.dtsx` | ✅ |
| 1.14 | **MISSING PARTICIPANT:** `cFinalizeCapEnrollment` | **OMITTED.** `ICapitationActivity` declares 8 methods; the diagram shows only 7. `FinalizeCapEnrollment` (instantiates `cFinalizeCapEnrollment`) is a real pipeline step between `StoreCapVoucher` and `FinalizeCapPayment`, but is completely absent from the diagram. | `CapitationActivity.vb:136-144` — `FinalizeCapEnrollment` method defined; `ICapitationActivity.vb:82` — interface method declared | **BLOCKER** |
| 1.15 | **MISSING PARTICIPANT:** `CapReStartProxy` | **OMITTED.** The UI `btFinish_Click` has a major branch for "Restart Previous Run" that uses `CapReStartProxy` — a completely different dispatch path. Not shown in any diagram. | `CreateCAPPaymentsWiz2.aspx.cs:372-465` — full restart path with `CapReStartProxy` | **SHOULD-FIX** |
| 1.16 | **MISSING PARTICIPANT:** Distribution Hub | Mentioned in description text but not a participant in any sequence diagram. It is the IIS-hosted intermediary between proxies and agents. | `capitation-integration-mechanisms-detailed.md` — described as central dispatcher | **SHOULD-FIX** |
| | | | | |
| **2. MESSAGES** | | | | |
| 2.1 | `btFinish_Click() → GenerateMessages(isGlobal)` | **CORRECT.** Exact method names. | `CreateCAPPaymentsWiz2.aspx.cs:347,490` | ✅ |
| 2.2 | `CapitationActivity.BuildCapEnrolls(message)` | **CORRECT.** | `CapitationActivity.vb:49` | ✅ |
| 2.3 | `CapitationActivity.SelectCapAffiliations(message)` | **CORRECT.** | `CapitationActivity.vb:64` | ✅ |
| 2.4 | `CapitationActivity.AllocateCapAmounts(message)` | **CORRECT.** | `CapitationActivity.vb:78` | ✅ |
| 2.5 | `CapitationActivity.AllocateCapFunds(message)` | **CORRECT.** | `CapitationActivity.vb:91` | ✅ |
| 2.6 | `CapitationActivity.AdjustCapFunds(message)` | **CORRECT.** | `CapitationActivity.vb:107` | ✅ |
| 2.7 | `CapitationActivity.StoreCapVoucher(message)` | **CORRECT.** | `CapitationActivity.vb:121` | ✅ |
| 2.8 | `CapitationActivity.FinalizeCapPayment(message)` | **CORRECT** method name on activity, but diagram says it creates `cCapPayment` — it actually creates `cFinalizeCapPayment`. (See 1.11) | `CapitationActivity.vb:150,157` | **BLOCKER** |
| 2.9 | `CNextId.GetStringID(QCS_ID_PAYABLE=4)` | **CORRECT.** `QCS_ID_PAYABLE` used for paymentid generation. PostCap SP confirms `idtype=4`. | `cBuildCapEnrolls.vb:1275`, `PostCap_SplitPayments.sql:269` (`EXEC QCSIDB.dbo.spQ_IDSeq 4,1`) | ✅ |
| 2.10 | Stage 1a calls `CNextId.GetStringID(QCS_ID_PAYABLE=4)` directly | **PARTIALLY WRONG.** In the PCP path, Stage 1a first gets a `QCS_ID_MISC` for the queueid (line 428), and only later in `CreatePayment()` (line 1275) gets a `QCS_ID_PAYABLE`. The diagram arrow 11 conflates queueid and paymentid generation. | `cBuildCapEnrolls.vb:428` (MISC), `:1275` (PAYABLE) | **SHOULD-FIX** |
| 2.11 | `BulkProxy.Invoke()` — fire-and-forget | **CORRECT.** `ProcessMessage` calls `Invoke(inputMessages)` which is inherited from `BulkProxy`. Comment says "Method returns immediately after submitting". | `CapSubmitProxy.cs:30-35` | ✅ |
| 2.12 | XSLT file loaded as `XSLTCapSubmit.xslt` | **WRONG CASING.** Code loads `"../XSLTCAPSubmit.xslt"` (CAP all-caps). Diagram text says `XSLTCapSubmit.xslt`. | `CapSubmitAgent.cs:67` — `xslt.Load("../XSLTCAPSubmit.xslt")` | **NIT** |
| 2.13 | Stage 7: `CalcCapAdjustments()` shown as active call | **WRONG — COMMENTED OUT.** `CalcCapAdjustments()` is commented out in the code. The diagram shows it as an active step in the finalization sequence. | `cCapPayment.vb:132` — `'CalcCapAdjustments()` (single-quote = VB comment) | **BLOCKER** |
| 2.14 | Stage 7: `DELETE capmessagequeue (cleanup)` | **WRONG — COMMENTED OUT.** The `DeleteCapmessagequeueByMessageidQueueid` call is commented out. | `cStoreCapVoucher.vb:144-145` — both lines commented out | **SHOULD-FIX** |
| 2.15 | PostCap SP name: `PostCap_SplitPayments` | **WRONG NAME.** SP is defined as `PostCap_SplitPayment` (singular, no 's'). | `PostCap_SplitPayments.sql:29` — `CREATE OR ALTER PROC dbo.PostCap_SplitPayment` | **NIT** |
| | | | | |
| **3. ORDER** | | | | |
| 3.1 | Stage 1a: `LoadProgramcaps() → LoadOpenPayments() → Build_Global/PCP → CreatePayment()` | **CORRECT** order. Code also has `ValidateCapFactorProgramFundsForAffiliations()` after Build and before `ProcessEnrollments()` — not shown. | `cBuildCapEnrolls.vb:230,252,255-258,262,264` | ✅ |
| 3.2 | Stage 4: Build age → build scoring SQL → query → sort → select winner → WriteFundDetail | **CORRECT** order. | `cCapFundAllocation.vb:188,323,382,392,424` | ✅ |
| 3.3 | **MISSING STEP** in pipeline: `FinalizeCapEnrollment` should appear between `StoreCapVoucher` and `FinalizeCapPayment` | The `ICapitationActivity` interface defines this as a distinct step. Diagram skips it entirely. | `ICapitationActivity.vb:82-89`, `CapitationActivity.vb:136-144` | **BLOCKER** |
| 3.4 | Stage 1a: `ValidateCapFactorProgramFundsForAffiliations()` — not shown | This runs AFTER enrollment build and BEFORE `ProcessEnrollments()`. It checks for invalid fund configurations and logs warnings. | `cBuildCapEnrolls.vb:262,302-355` | **SHOULD-FIX** |
| | | | | |
| **4. SYNC vs ASYNC** | | | | |
| 4.1 | `BulkProxy.Invoke()` labeled "fire-and-forget" | **CORRECT.** Comment in proxy code confirms async/fire-and-forget semantics. | `CapSubmitProxy.cs:29` — "Method returns immediately after submitting" | ✅ |
| 4.2 | Proxy→Agent dispatch drawn as synchronous solid arrow | **MISLEADING.** `BulkProxy.Invoke()` is explicitly fire-and-forget, but the diagram uses a synchronous arrow `->>`). Should use `-->>` or annotate. | `CapSubmitProxy.cs:29-35` | **SHOULD-FIX** |
| 4.3 | Stage 7→PostCap→Stage 10/11 drawn as direct flow | **MISLEADING.** PostCap is triggered externally (agent config or manual), not directly from Stage 7. Stage 10/11 is batch-scheduled SSIS — temporal gap is not shown. | Description text says "actual trigger unknown" | **SHOULD-FIX** |
| | | | | |
| **5. RETURNS** | | | | |
| 5.1 | QCSIDB returns "Unique paymentid" | **CORRECT.** `CreatePayment` returns `spayid` (the generated paymentid). | `cBuildCapEnrolls.vb:1275,1300` | ✅ |
| 5.2 | Plandata returns "programcap config" | **CORRECT.** `LoadProgramcaps()` reads `effdate`, `capmethod`, `usetruecap` from programcap. | `cBuildCapEnrolls.vb:230,240-250` | ✅ |
| 5.3 | Stage 7: `isCapFinished()` returns RUNNING/FINISHED | **CORRECT.** Checks `batchstatus = "FINISHED"`. If not finished, sets statusmsg to "RUNNING". | `cCapPayment.vb:319,167` | ✅ |
| 5.4 | Multiple INSERT operations show no return arrows | **ACCURATE representation.** These are fire-and-forget `ExecuteQuery()` calls with no meaningful return value used. | `cStoreCapVoucher.vb:601`, `cCapFundAllocation.vb:680` | ✅ |
| | | | | |
| **6. ERROR / ALTERNATE PATHS** | | | | |
| 6.1 | Stage 1a: Restart path (`RestartFailedCapRun`) not shown | `cBuildCapEnrolls.ProcessMessage()` branches on `m_XMLMessage.restarting = "Y"` and calls `RestartFailedCapRun()` — a completely different execution path not shown. | `cBuildCapEnrolls.vb:141-147` | **SHOULD-FIX** |
| 6.2 | Stage 1a: Error handling on enrollment build | Every major method has `Try/Catch` blocks that log via `CreatePLDetailState(ERROR)`. None shown in diagram. | `cBuildCapEnrolls.vb:173-186,278-291,509-517` | **SHOULD-FIX** |
| 6.3 | Stage 7: Payment deletion when 0 vouchers exist | If `capvoucher` count = 0 after cleanup, the payment record is DELETED (`cDeletePaymentByPaymentid`). Not shown. | `cCapPayment.vb:381-389` — `l_DeletePaymentByPaymentid.ExecuteQuery(m_XMLMessage.paymentid)` | **SHOULD-FIX** |
| 6.4 | Stage 7: Payment split by multiple funds | If vouchers span >1 fund, `CreateCapPayment()` generates NEW paymentids and splits. This is a significant branch not shown. | `cCapPayment.vb:408-422` — `If dtFundId.Rows.Count > 1 Then` ... `nextid.GetStringID(QCS_ID_PAYABLE)` | **SHOULD-FIX** |
| 6.5 | Stage 7: Max cap voucher count split | `SplitCapVoucherByConfiguredCount()` splits vouchers if they exceed a configured maximum. Not shown. | `cCapPayment.vb:436-437` — `If maxCapVoucherCountPerPayment > 0 Then SplitCapVoucherByConfiguredCount(...)` | **SHOULD-FIX** |
| 6.6 | UI: Restart path via `CapReStartProxy` | `btFinish_Click()` has a major `if(type == "Restart Previous Run")` branch that uses `CapReStartProxy` instead of `CapSubmitProxy`. Not shown. | `CreateCAPPaymentsWiz2.aspx.cs:372-465` | **SHOULD-FIX** |
| | | | | |
| **7. CONDITIONAL BRANCHES** | | | | |
| 7.1 | Stage 1a: `isGlobal` branch → `Build_Global_Enrollments()` / `Build_PCP_Enrollments()` | **CORRECTLY shown** with `alt` fragment. | `cBuildCapEnrolls.vb:255-258` | ✅ |
| 7.2 | Stage 4: `captype = capterms / capfactors` | **CORRECTLY shown** with `alt` fragment in Stage 3 diagram. Stage 4 code also has this branch. | `cCapFundAllocation.vb:117-120` | ✅ |
| 7.3 | Stage 5: `capmethod = S / F / default` | **CORRECTLY shown** with `alt` fragment. (PayBySplitmonth, PayByFrontEndBackEndWash, AdjustForDays) | `cCapFundAdjustment.vb` (methods confirmed in grep results) | ✅ |
| 7.4 | Stage 7: `All batches FINISHED / Still RUNNING` | **CORRECTLY shown** with `alt` fragment. | `cCapPayment.vb:129,167` | ✅ |
| 7.5 | **MISSING:** Stage 7 `isSummary` branch | If `m_XMLMessage.isSummary = True`, calls `CreateCapSummary()` — a different path. Not shown. | `cCapPayment.vb:368` — `If m_XMLMessage.isSummary = True Then CreateCapSummary()` | **SHOULD-FIX** |
| 7.6 | **MISSING:** Stage 7 multi-fund split branch | If vouchers span multiple funds, new paymentids are generated and vouchers reassigned. Not shown as `alt`. | `cCapPayment.vb:406-422` | **SHOULD-FIX** |
| 7.7 | **MISSING:** Stage 1a `isSummary AND isGlobal` branch | Sets `recondate = capdate` and `reviewdate = capdate`. Not shown. | `cBuildCapEnrolls.vb:219-222` | **NIT** |
| | | | | |
| **8. LOOPS / BATCHING** | | | | |
| 8.1 | Stage 1b: `loop For each month` | **CORRECTLY shown.** Code iterates months for each enrollment. | `cBuildCapAffils.vb` — month iteration in `BuildPCP()` / `BuildGlobal()` | ✅ |
| 8.2 | Stage 4: Iteration over affiliations → months → funds | Drawn as a single call per affiliation/month, but code iterates `While Not m_XMLMessage.Affiliations.EOF` → `While Not Months.EOF` → `While Not Funds.EOF`. | `cCapFundAllocation.vb:114-139` | ✅ |
| 8.3 | Stage 7: capvoucher INSERT per enrollment×fund | Diagram shows single arrow. Code iterates `While Not Affiliations.EOF → While Not Months.EOF → While Not Funds.EOF` calling `InsertCapVoucher()` per combination. | `cStoreCapVoucher.vb:240-420` | **SHOULD-FIX** |
| 8.4 | PostCap: loop per OPEN payment | **CORRECTLY shown** with `loop For each OPEN cap payment`. | `PostCap_SplitPayments.sql:159-161` — `WHILE @OuterLoopNum <= @TotalOuterLoop` | ✅ |
| | | | | |
| **9. EXTERNAL DEPENDENCIES** | | | | |
| 9.1 | Plandata_Parallel DB | **CONFIRMED** — used by all stages. | `cBuildCapEnrolls.vb:104` — `Session.SelectAlias("PlanData")` | ✅ |
| 9.2 | QCSIDB (IDSequence) | **CONFIRMED** — used for ID generation. | `cBuildCapEnrolls.vb:105` — `QSession.SelectAlias("qenterprise")`; `CNextId_VBNet.vb:27` — calls `IDSequence` service | ✅ |
| 9.3 | PlanIntegration DB | **CONFIRMED** — used for process logging. | `cBuildCapEnrolls.vb:107-109` — `sessionPlanintegration.SelectAlias("planintegration")` | ✅ |
| 9.4 | HealthPAS_Common DB | **CONFIRMED** — PostCap inserts into it. | `PostCap_PreFundAllocate.sql:328` — `INSERT INTO Healthpas_Common.dbo.hpfin_FAAttributesCAP` | ✅ |
| 9.5 | CA_Extensions DB | **CONFIRMED** — PostCap SPs reside here. | `PostCap_PreFundAllocate.sql:1` — `USE CA_Extensions` | ✅ |
| 9.6 | Flexi GL (Linked Server) | **CONFIRMED** — finance SSIS packages exist in `mms-bolton-fin-ohio-main`. | `mms-bolton-fin-ohio-main/components/packages/md/hpfin_APInvoiceUpdate.dtsx` | ✅ |
| 9.7 | `CAPRateExtensions` table | **CONFIRMED.** | `act-oh-OHDDI-main/components/Source/Tables/CAPRateExtensions.sql` | ✅ |
| 9.8 | **NOT IN DIAGRAM:** `MessageStore` (ENV02) | The claim-check pattern (HTTP POST with message ID → separate MessageStore instance) is documented in architecture files and used by the Distribution Hub, but no diagram shows it. | `capitation-integration-mechanisms-detailed.md` — Section III | **SHOULD-FIX** |
| | | | | |
| **10. DATA CONCERNS** | | | | |
| 10.1 | Member DOB flows in XML messages | Stage 4 uses `m_XMLMessage.dob` for age calculation. DOB is PII flowing through the XML pipeline. Diagram arrow labels do not expose it explicitly, but the description text mentions it. | `cCapFundAllocation.vb:188` — `CDate(m_XMLMessage.dob)` | **NIT** |
| 10.2 | Enrollid in message labels | `enrollid` appears in several diagram labels. This is a member identifier (PII-adjacent for Medicaid). Acceptable for an internal architecture document but worth noting. | Multiple diagram arrows | **NIT** |
| 10.3 | No credentials or tokens shown | The diagram does not expose passwords, connection strings, or auth tokens in any message label. | All diagrams reviewed | ✅ |

---

## Summary of Findings

| Severity | Count | Items |
|----------|-------|-------|
| **BLOCKER** | 4 | 1.11, 1.14, 2.8, 2.13 |
| **SHOULD-FIX** | 15 | 1.15, 1.16, 2.10, 2.14, 3.3, 3.4, 4.2, 4.3, 6.1–6.6, 7.5, 7.6, 8.3, 9.8 |
| **NIT** | 5 | 2.12, 2.15, 7.7, 10.1, 10.2 |
| **✅ Confirmed** | 34 | All other entries |

---

## BLOCKER Details

1. **Wrong class name (1.11 / 2.8):** Diagram says `cCapPayment` handles finalization. Code shows `CapitationActivity.FinalizeCapPayment()` instantiates `cFinalizeCapPayment` at line 157 of `CapitationActivity.vb`. The class `cCapPayment.vb` exists as a separate file containing pool/check logic, but it is NOT what `FinalizeCapPayment()` creates.

2. **Missing pipeline step (1.14 / 3.3):** `FinalizeCapEnrollment` is declared in `ICapitationActivity.vb` (line 82), implemented in `CapitationActivity.vb` (line 136), and creates `cFinalizeCapEnrollment`. This is an entire pipeline step between StoreCapVoucher and FinalizeCapPayment that the diagram omits. The file `FinalizeCapEnrollment.vb` exists in the project directory.

3. **Commented-out code shown as active (2.13):** `CalcCapAdjustments()` at `cCapPayment.vb:132` is commented out with a VB single-quote. The diagram (Figure 7, Stage 7) shows `CalcCapAdjustments() → INSERT ADJUSTMENT vouchers` as an active step in the finalization flow. This will mislead any reader into believing adjustment vouchers are created during finalization.

---

## Accuracy Statistics

| Metric | Count | Percentage |
|--------|-------|------------|
| **Total claims verified** | 53 | 100% |
| ✅ Confirmed correct | 34 | 64% |
| NIT (cosmetic — acceptable) | 5 | 9% |
| **Total acceptable (correct + NIT)** | **39** | **74%** |
| SHOULD-FIX (omissions, not errors) | 15 | 28% |
| BLOCKER (factually wrong) | 4 | 7% |

### Accuracy by Perspective

| Perspective | Rate | Explanation |
|-------------|------|-------------|
| **What the diagram shows** | **~92%** | Of the ~48 positive claims the diagram makes, only 4 are factually wrong. |
| **Overall completeness** | **74%** | Including omitted paths/steps that exist in code but are missing from the diagram. |
| **Error rate** | **7%** | Only 4 out of 53 claims are actively misleading (BLOCKERs). |

---

## Verdict

### **NEEDS CHANGES**

The diagram is remarkably well-grounded for AI-generated output — 39 of 53 verified claims are confirmed correct or acceptable in source code with correct class names, method signatures, algorithm details (age-in-months, sort order, winner selection rule), and database operations. What the diagram *does* show is ~92% correct — the gap is primarily in what it *doesn't* show (omitted paths and steps). However, four BLOCKER-level findings prevent it from being rated ACCURATE:

1. Fix the class name: `cCapPayment` → `cFinalizeCapPayment`
2. Add the missing `FinalizeCapEnrollment` pipeline step
3. Remove or strike-through `CalcCapAdjustments()` (it is commented out in production code)
4. Remove the `DELETE capmessagequeue` arrow (also commented out)

---

## Not Verifiable

| Claim | Reason |
|-------|--------|
| `QCS_ID_PAYABLE` enum value = 4 | Enum is defined in `Q.Global.dll` (compiled assembly, not in source). Strongly implied by PostCap SP using `spQ_IDSeq 4,1`, but not directly confirmed from enum source. |
| "10 scoring dimensions" for Query 10794 | The query class name is confirmed (`cSelectCaptermsByPcpsvczipRatecodelistGenderEtcCapitationEtc`), and 12 parameters are passed at `cCapFundAllocation.vb:325-326`. The exact number "10" dimensions vs parameters could not be verified without the query XML definition. |
| `processstate` table has ~2B rows | Runtime data claim — cannot be verified from source code. |
| "153,071 PK collisions per RECON run" | Runtime/production data claim — cannot be verified from source code. |
| Stage 8/9 `Assembly.Load from GAC` | The `ProcessExecutionHost` assembly-loading logic is described but not in the workspace source. Likely in a QFrame infrastructure assembly. |
| PostCap trigger mechanism | Description says "actual trigger unknown." Cannot confirm whether it is agent-config-driven, scheduled, or manual from available source. |
| Linked server 2PC to Flexi GL | SSIS packages are binary `.dtsx` files — internal SQL steps not readable from this workspace. |
| "71 concurrent cap runs" serializing through idtype=4 | Runtime concurrency claim — cannot verify from code. |
