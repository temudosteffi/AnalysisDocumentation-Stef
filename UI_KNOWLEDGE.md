UI KNOWLEDGE — what to look for, by question
Use only the parts the user's question needs; a question about navigation does not need
data-binding lineage. First identify the UI technology from the file extension, because the
concepts below map differently per stack: WebForms (.aspx/.ascx), ASP.NET MVC Razor (.cshtml),
VB6 forms (.frm), WPF (.xaml), SSRS report layout (.rdl).

- Screen identity & placement: the screen's technology (extension); the repo and business
  module it lives under (e.g. QClaim / QMember / Authorization / Finance); whether it is a full
  page, a reusable control/partial (.ascx, Razor partial, WPF UserControl), a layout/master, or
  a popup/dialog; parallel/branch copies of the same screen (same name in a second website
  folder) — treat them as one logical screen unless asked to compare.

- Layout & composition: master page / _Layout / frameset / WPF Window that hosts the screen;
  nested user controls or partials and the order they render; tabs, panels, framesets and popup
  containers; which control renders which region. Name a shared control only when the include /
  RenderPartial / control registration that pulls it in is cited.

- Controls & events (WebForms/VB6): server controls (GridView, DropDownList, TextBox, Button,
  etc.) and their IDs; the code-behind events they fire (Page_Load, button Click, SelectedIndex
  Changed, grid RowCommand) and what those handlers do; IsPostBack branches; ViewState / Session
  keys read or written; validators. For VB6 .frm: form controls, their event subs (Click,
  Load, Timer) and the module procedures they call.

- Model & binding (MVC Razor): the @model type; Html/Tag helpers and the model properties they
  bind; the controller + action that returns the view and the action that receives the post;
  ViewBag/ViewData keys; client validation attributes. Trace a field back to its model property,
  not to a hard-coded string.

- Navigation & flow: how the user arrives (menu item, link, redirect, dispatcher/router page)
  and where each action sends them (Response.Redirect, RedirectToAction, form action, WPF
  navigation); wizard step order and the next/back transitions; modal vs full-page; login /
  session-timeout / unauthorized / error-page redirects. Cite the redirect or route that
  implements the hop.

- Data source behind the screen: the service, API, repository, or stored procedure the screen
  calls to load and to save; the parameters passed and the result shown; grids bound to a
  dataset vs a service call. Name the call site (code-behind method, controller action, or
  binding) — never assume a table without the query or proc that touches it.

- Auth, security & access: login/registration/change-password screens; role or permission
  checks that show, hide, or disable controls; "grant user access" / user-group / permission
  screens; unauthorized and no-access pages. Never copy a connection string, password, token,
  or user id found in markup or code-behind.

- Reports (.rdl): dataset queries (command text / stored proc) and their parameters; report
  parameters and available-value sources; the tablix/matrix/chart regions and the dataset
  fields they show; grouping, filters and expressions; sub-reports and drill-through targets.
  Treat .rdl as report layout, not interactive UI.

- Estate conventions you may recognise only from evidence, never assume: a QNXT/HealthPAS "Q"
  module screen set (QClaim, QMember, QContract…) with a shared master and help (.htm) pages; a
  VUE360 MVC feature area (Views/<Area> + matching controller); a Process Manager wizard chain
  (…Wizard step pages sharing a code-behind base); a shared header/nav user control included
  across many pages; a viewer/dispatcher/error-router trio for document and image display. Name
  the pattern only when the pages, controls, or routes that implement it are cited.
