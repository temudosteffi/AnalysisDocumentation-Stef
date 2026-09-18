# UI KNOWLEDGE — what to look for, by question

Use only the parts the user's question needs; a question about navigation does not need
control-level data binding.

- Screen type & technology: identify the UI kind before anything else — ASP.NET WebForms
  (`.aspx` pages, `.ascx` user controls, `.master` layouts), ASP.NET MVC/Razor (`.cshtml`
  views + controller/action), classic ASP (`.asp`), VB6 desktop forms (`.frm`), WPF
  (`.xaml` + code-behind), DHTML behaviors (`.htc`). The stack dictates where markup, layout,
  events and data binding live (WebForms code-behind vs MVC controller vs VB6 event subs).

- Page structure & layout: for WebForms the `MasterPageFile` and the `.master` shell that
  frames the page; ContentPlaceHolder regions; for Razor the `_Layout`, `_ViewStart` and
  partials/`Shared` views; framesets and framing pages (classic `.asp` viewer/launch frames);
  popup/modal containers, intermediary/redirect pages, toolbars and nav dispatchers. Name the
  reusable shell (Site.Master, Site.Mobile.Master) a screen sits in, not just the leaf page.

- Navigation & flow: how one screen reaches another — postback + `Response.Redirect`/
  `Server.Transfer`, MVC `RedirectToAction`/route, wizard step sequence (Add/Edit/Main step
  pages), selector/picker pages that return a value to a caller, and popup/dialog return paths.
  For wizards, cite the ordered step pages and the control that advances them.

- Controls & data binding: input controls and grids (TextBox, DropDown, ListBox, CheckBox/
  RadioButton groups, GridView, repeaters, tree views); the data source each is bound to and
  where it is populated (Page_Load / `!IsPostBack`, controller model, form load); lookup/
  reference pickers and the code set they read; for form-builder screens (ControlWizard) the
  control type being generated.

- Events & code-behind: server-side event handlers (Button_Click, SelectedIndexChanged,
  ItemCommand, Page_Load/Init), MVC action methods (GET vs POST), VB6 form event subs
  (Form_Load, control_Click); ViewState / session values a screen reads or writes; what each
  event does (save, search, route, launch a viewer) — name the handler and its target.

- Validation & error handling: client vs server validation (validator controls, ModelState,
  jQuery/script checks); required-field and format rules; dedicated error/access pages
  (ErrorPage, GenericErr, NoAccess, UnAuthorisedUser, NotImplemented) and where failures
  redirect; timeout/session-expiry handling.

- Security & access: authentication pages (Login, Logoff, ChangePassword, Register), session
  timeout/warning screens, role/permission checks that gate a page or control (GrantUserAccess,
  user/group wizards), and any per-screen access flag. Never copy connection strings, passwords,
  or user IDs from config.

- Service / backend calls: how a screen gets or sends data — WCF service references (`.svc`),
  legacy SOAP web services (`.asmx`), or the DHTML `webservice.htc` helper; name the service and
  operation the screen calls (e.g. VUE360QWCFService, MMSDocumentWCF_P8, FinancialService), and
  whether it reads (search/lookup) or writes (save/commit). Treat `.svcmap`/`.wsdl`/`.disco`
  under Service References as generated proxy metadata, not endpoints.

- Reports & documents: embedded SSRS reports (`.rdl`) a screen launches and the parameters it
  passes; document/image viewers (DisplayDoc, ViewImage, DocumentViewer, TiffToPDF) and the
  FileNet P8 / imaging service behind them; letter-generation screens and their templates.

- Module / area grouping: place a screen in its business area — QNXT/HealthPAS modules
  (QClaim, QMember, QProvider, QContract, QBenefit, UM, Finance, EDI, Security), VUE360 MVC
  areas (Claims, Member, Provider, Authorization, Finance, Case, Configuration), Process Manager
  areas (queues, work/doc classes, workflow routes, action server, document explorer), EDI
  Gateway areas (claim scanning, 277U/835, scheduler, auto-adjust). Group by folder, not by
  guessing.

- Estate conventions you may recognise only from evidence, never assume: parallel branch/
  website copies of the same screen set (count the distinct name, not every file); a `*QNXT`/
  `*QFrame` `.asmx` service tier with one service per business module behind the pages; client
  DHTML behaviors (TabStrip, treeview, toolbar) reused across QNXT screens; classic-ASP framing
  pages wrapping newer `.aspx` UI; VB6 admin/monitoring client fronting a workflow server. Name
  the pattern only when the pages, controls and services that implement it are cited.
