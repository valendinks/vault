# Reports Foundation Alignment

- Board: https://miro.com/app/board/uXjVGhcZSbs=/
- Exported: 2026-06-02T16:25:45Z
- Scope: Canonical export. Includes the main numbered narrative, canonical Alignment 1-6 docs, shared vocabulary, report classification table, form-format comparison table, and key open decisions/gaps. Retired or duplicate working records are omitted except as supporting-note references.

# Board Overview

This board documents the foundational redesign of Plans' property diagnostic reports system. The goal is to make new regulated and customer-specific reports primarily content-driven, with bounded engineering glue, instead of repeating bespoke implementation work across iOS, backend, and web.

The architecture separates report work into four domains:

- **cases**: orchestration of inspection jobs, required reports, assignees, and delivery lifecycle.
- **homes**: reusable property state such as spatial model, materials, technical installations, and calculation inputs.
- **captures**: report-scoped evidence and facts, including typed JSONB data and photos.
- **installers**: service-provider domain, intentionally unchanged.

The board explicitly separates three decisions:

- **Data model and write paths**: decided by the numbered architecture frames.
- **Form-definition format**: separate RFC; current direction is FHIR Questionnaire + ValueSet + FHIRPath, with SpeziQuestionnaire on iOS.
- **Authoring and per-report UX**: later work, not part of the foundation decision.

The core invariant is: **captures may reference Home objects but never mutate them**. Shared property state lives in `homes`; report-scoped evidence and conclusions live in `captures`.

## Numbered Narrative

### 1. Why We Are Doing This

Current state:

- Every captured field requires hardcoded changes across three codebases: iOS, Calor/Go, and web-main.
- Adding one field or creating one report requires coordinated platform changes and deployments.
- Each report becomes a multi-month, multi-codebase project.

Goal:

- Ship all 8 regulated reports by the end of 2026 without compounding implementation cost.
- Turn report creation into mostly content work: questions, rules, validation, and deliverables.
- Keep engineering involved for bounded glue, review, release control, integrations, and platform gaps.

Scope boundary:

- **In**: foundation primitives, data model, write paths, and form-format direction.
- **Out for later**: task-based UX flows, admin authoring tooling, completion semantics, customer-editable templating, and full migration sequencing.

### 1.5. Form-Definition Format — Separate Decision

This board decides how form data is captured and stored:

- Envelope shape.
- Four-domain split.
- JSONB for `CaptureType` data.
- Dual write paths with audit trails.
- Reports as intent with lifecycle.

The separate form-format RFC decides:

- Form schema declaration.
- Field labels and codelists.
- Validation rules.
- Conditional field logic.
- Cross-platform rendering on iOS and web.
- Versioning and internationalization.

Current form-format direction:

- FHIR Questionnaire + ValueSet + FHIRPath.
- SpeziQuestionnaire for iOS.
- Native versioning via `Questionnaire.url` + `version`.

Important stability note: frames 1-10 remain stable regardless of the final form-definition format.

### 2. The New Model — 4 Domains

The new model separates:

- **cases**: currently "leads"; orchestrates inspection jobs, required reports, and delivery lifecycle.
- **homes**: property state; spatial chain, materials, EPC technical indicators, calculation engines.
- **captures**: new domain for inspection evidence and report-scoped facts; capture envelope, typed JSONB, photos.
- **installers**: unchanged service-provider domain.

Relationships:

- `cases` reads from `installers` and `homes`.
- `captures` anchor to `cases`.
- `captures` may reference `homes`.
- `captures` do not mutate `homes`.

Boundary test:

- If a fact affects the reusable Home model for downstream calculations, put it in `homes`.
- If a fact affects the evidence or conclusion of a specific diagnostic report, put it in `captures`.

### 3. State vs Capture — Same Wall, Many Facts

One physical wall can produce many facts:

- **homes** facts:
  - Wall geometry for spatial modeling.
  - Segment material for EPC heat-loss calculations.
- **captures** facts:
  - Asbestos finding.
  - Lead measurement.
  - Termite finding.
  - Crack in wall.

Rule of thumb:

- Persistent, reusable property model data belongs in `homes`.
- Case/report-scoped evidence and diagnostic context belongs in `captures`.
- Some composite workflows create both: for example, a surveyor anchors a heat pump with photo evidence and updates Home state in the same transaction.

### 4. Two Write Paths

The system supports two write paths.

Capture write:

- Used for field operations requiring evidence: photos, forms, measurements, samples, structured forms, or bulk imports.
- Audited through capture rows, photos, form data, and related metadata.
- Example: a surveyor anchors a heat pump with photo and form data.

Direct state write:

- Used for simple administrative or state edits without evidence requirements.
- Audited through row-level columns such as `created_by`, `created_at`, `updated_by`, and `updated_at`.
- Example: an office user toggles room heating status.

Composite write:

- Some operations create both state records and captures in one transaction.
- This supports evidence-backed changes to reusable Home state.

### 5. Adding a Field — Today vs Tomorrow

Today:

- Change iOS Swift types and UI.
- Change Calor Go types and storage.
- Add database migrations.
- Update OpenAPI and generated types.
- Change web-main.
- Coordinate deployment.

Impact: multi-day per field, multi-month per report.

Tomorrow:

- Update `CaptureType` configuration.
- iOS renders from config.
- JSONB stores typed capture data.
- Backend and web read unified config/state.

Impact: minutes per field, config-only deploy for many changes.

This is the clearest measure of whether the redesign earns its keep.

### 6. Where Evidence Lives — Today vs Tomorrow

Today, evidence is scattered across tables such as:

- `home_images`
- `technical_installation_images`
- `room_by_room_note_images`
- `room_by_room_captured_azimuths`
- `room_by_room_notes`

These are foreign-keyed to snapshot-internal IDs. Edits clone data and create accidental over-versioning, including unchanged evidence.

Tomorrow:

- Evidence lives in a unified `captures` domain.
- Stable ObjectIDs anchor captures.
- Capture rows represent universal envelopes.
- Typed JSONB stores capture data.
- Photos connect through `Photo` and `CapturePhoto`.
- The model is append-only/event-oriented instead of clone-on-edit.

### 7. Report Classification — Where Each Report Lands

Reports are classified by whether their primary data impact is `homes`, `captures`, or mixed.

Homes-driven reports:

- FR Boutin
- FR Carrez
- FR EPC
- DK EPC

Capture-driven reports:

- FR Termite
- FR Asbestos
- FR Lead/CREP
- FR Electricity
- FR Gaz
- FR Admin
- DK Electrical
- DK Condition

EPC has two capture flavors:

- Composite writes that update Home state.
- Capture-only supplementary report comments that do not affect Home state.

### 8. Execution Plan (8 Phases)

The execution philosophy is incremental: no phase blocks the next, and no phase breaks production.

1. Add foundation layer: additive capture schema, domain models, and Provider interfaces alongside legacy systems.
2. Prove JSONB end-to-end with admin `owner_name`: config, form handling, upload, storage, and readback.
3. Build a hard-envelope slice: anchored capture with photo and `report_id`, using a minimal gas-appliance example.
4. Migrate legacy image/evidence data: Home/TI/note images and captured azimuths into captures.
5. Migrate room-by-room notes C-portion into captures and deprecate B-portion.
6. Add audit infrastructure: `created_by`, `created_at`, `updated_by`, `updated_at` on regulated-report state tables.
7. Integrate reports into cases: reports table, activate `Capture.report_id`, computed views via Providers.
8. Opportunistic improvements: materials split, proposal ownership, and snapshot rationalization as the foundation proves itself.

### 9. What Won't Change — This Is Not a Rewrite

The following remain stable initially:

- Spatial hierarchy: Home -> Building -> Storey -> Room -> BuildingElement -> Segment.
- EPC input structure: materials and EPC technical installations remain typed inside `homes`.
- Calculation engines: French area calculator, EPC, and similar engines remain in place and are accessed through Providers.
- Materials and technical installations remain typed by design; JSONB is for new `CaptureType` data.
- iOS upload mechanism: single end-of-case zip via `/upload-notification` continues initially.
- RbR API response shape: preserved initially through a new composer.
- Installers domain: untouched.
- `home_snapshots` versioning: persists, but evidence leaves the snapshot FK graph to reduce cloning.

### 10. Known Gaps — Out of Scope for the Foundation

These are real concerns, but not preconditions for the foundation:

- Offline conflict resolution for multi-device room/capture editing.
- Audit retention and soft-delete, including GDPR and regulatory history beyond timestamps.
- Identity modeling for user, surveyor, and system actors in audit fields.
- Backend-served configuration, including admin authoring, cache invalidation, offline staleness, and version compatibility.
- Performance and report completion semantics, including load testing and per-bundle state machines.
- Task-based workflow, including TaskGroup model, surveyor wizard flow, and completion progress tracking.

Reference docs listed on the board:

- `plans.md`
- `foundation-restructured.md`
- `form-definition-format.md`
- `french-diagnostics-system-design.md`

### 11. Per-Platform Footprint — What Changes Per Codebase

Backend / Calor / Go:

- New captures domain architecture.
- Composite-write handlers.
- Cross-domain composer for RbR views.
- Audit columns.
- Legacy migration.
- Reports integration.

iOS:

- Local data store refactoring.
- Dynamic form rendering via `CaptureType` config.
- Composite-write UX.
- Photo handling improvements.
- Upload payload restructuring.
- Potential FHIR/SpeziQuestionnaire integration.

web-main / Kelvin:

- Read from the new composer for joined state and captures.
- Preserve direct state writes for office edits.
- Future admin-authoring UI for `CaptureType` configs.
- Eventual material/TI description editor.

## Tables

### Report Classification

| Report | Country | Primary | State | Capture | Notes |
|---|---|---|---|---|---|
| FR Boutin | FR | homes | spatial + measurements (per Loi Boutin) | minimal — surveyor signature/metadata | Surface area declaration per Loi Boutin (furnished rentals). Homes-driven, light capture — same shape as FR Carrez. |
| FR Carrez | FR | homes | spatial + measurements (per Loi Carrez) | minimal — surveyor signature/metadata | Surface area certification per Loi Carrez (property sales). Similar shape to EPC: homes-driven, light capture. |
| FR Termite | FR | captures | none | discovery findings, anchored | Light schema |
| FR Asbestos | FR | captures | none | element + nested samples | Uses parent_capture_id |
| DK Electrical | DK | captures | none | checklist + defects | Similar to FR Electricity |
| FR Gaz | FR | captures | none | gas appliances + checklist | Appliance per-Gaz unless promoted to TI |
| FR Admin | FR | captures | small (room creation only) | owner data, admin records | Mostly case-scoped (no report_id) |
| FR EPC | FR | homes | spatial + materials + EPC TIs | (1) scan discoveries -> composite write; (2) supplementary report comments -> capture-only | Two flavors of capture: composite (updates home state) + capture-only (report text, no home-state effect) |
| FR Lead/CREP | FR | captures | none | samples, device calibrations | Multi-phase: onsite + lab |
| DK Condition | DK | captures | none | defect observations | Light state, heavy capture |
| FR Electricity | FR | captures | none | checklist answers, defects | Form-heavy, conditional logic |
| DK EPC | DK | homes | spatial + materials + EPC TIs | same as FR EPC: scan discoveries (composite) + supplementary report comments (capture-only) | Same dual pattern as FR EPC. Real ticket: "tap-your-way to EPC supplementary comments" (Linear) |

### Form-Definition Format Comparison

| Dimension | Typed (today) | FHIR + codegen | SDUI bundle | Hybrid (FHIR forms + SDUI orchestration) |
|---|---|---|---|---|
| Projection: capture -> calculator input | Hand-written Go | Hand-written Go (FHIR doesn't help) | TS in bundle | TS in bundle |
| Projection: capture -> XML structure | Hand-written Go templates | Hand-written Go templates | TS in bundle + Go XML serializer | TS in bundle + Go XML serializer |
| Calor runtime requirement | Native Go | Go + FHIR mapper (codegen) | Go + embedded JS (V8 or goja) — NEW operational concern | Go + FHIR mapper + embedded JS |
| Task list / orchestration | Swift orchestrator + TS orchestrator + Go validator (3x) | Hand-coded x 3 (FHIR PlanDefinition clunky) | TS code in bundle (1 file) | TS code in bundle |
| Heavy compute (thermal, area) | Go (native, fast) | Go (native, fast) | Go (bundle calls Go; too slow in JS) | Go |
| Best at | Compile-time guarantees; simplest mental model | Form-level concerns: partial capture, required fields, codelists, versioning | Orchestration + projection; logic-as-code in one artifact | Each format's strengths cleanly separated; FHIR for form, SDUI for cross-form |
| iOS runtime requirement | Native Swift | Stanford FHIR lib (SpeziQuestionnaire + ResearchKitOnFHIR) | JavaScriptCore (already used today for create-project flow) | FHIR lib + JSC |
| Contract source of truth | DB schema + 3 hand-typed langs (Go/Swift/TS) | Questionnaire (one file) -> codegen Go/Swift/TS | TS bundle -> codegen Go (web/iOS consume directly) | Questionnaire per form + bundle for cross-form |
| Field-name/type drift caught at | Compile time across 3 codebases | Codegen time (CI) | Codegen time (CI) | Codegen time (CI) |
| Worst at | 3x duplication scales badly; high change cost per report | Orchestration (FHIRPath far less expressive than TS); projection still hand-written in Go | Versioning conventions; JS runtime ops; ecosystem fragmentation | Two formats to learn / maintain; operational complexity highest |
| Field-semantic drift caught at | Tests only (units, meaning shifts) | Tests only | Tests only (or contained if computation moves into bundle) | Tests only |
| Conditional visibility | Hand-coded x 3 | enableWhen / FHIRPath | TS branches in bundle (unlimited) | TS in bundle for cross-form; enableWhen for in-form |
| Ecosystem maturity | Native; well-trodden | Mature (healthcare; HAPI FHIR, Stanford BDHG) | Fragmented (Airbnb, Lyft patterns; mostly per-company) | FHIR mature + custom SDUI conventions |
| Adding a field — places to change | 5+ (SQL migration + Go + Swift + TS + XML) | 2 (Questionnaire + projection if changed) | 1 file (bundle, re-ship) | 1-2 (Questionnaire + bundle if cross-form change) |
| Operational complexity (Calor) | Familiar | Library learning curve; FHIR semantics | JS runtime ops; new failure modes | Highest — both runtimes |
| Partial capture | Hand-coded validators x 3 platforms; nullable columns + status enum | NATIVE: item.required + QR.status = in-progress | Bundle validators per gate (onSave / draftReady / submitReady) | FHIR native per form + bundle aggregates report-level submit-ready |
| Light computed fields | Hand-coded x 3 | FHIRPath (simple cases only) | TS in bundle | TS in bundle |
| Cross-field validation | Hand-coded x 3 platforms | FHIRPath (limited expressiveness) | TS code (unlimited) | FHIRPath for in-form; TS in bundle for cross-form |
| web runtime requirement | Native TS | TS FHIR library | NATIVE TS — cheapest of all platforms | TS FHIR library + native TS bundle |
| Adding a task — places to change | 3 (Swift orchestrator + TS orchestrator + Go validator) | 3 (PlanDef + hand-coded x 3 platforms) | 1 file (bundle) | 1 file (bundle orchestration layer) |
| Versioning mechanism | Schema migrations (forward-only) | NATIVE: Questionnaire.url + version; QR references url\|version | Convention (bundle semver); you build it | FHIR native per form + bundle convention |

## Canonical Alignment Docs

### Alignment 1 — Problem, Objective, and Report Families

#### 1. The Problem We Are Solving

We need to build and operate many property diagnostic reports across countries:

- **France** (regulated): Gaz, Electricity, Lead/CREP, Asbestos, Termite, Admin
- **Denmark** (regulated): DK Electrical, DK Condition
- **Existing computation-heavy reports**: DK EPC, FR EPC/DPE, Loi Carrez
- **Future bespoke customer-funded reports** such as a hypothetical Klimasikringsrapport

Today, every report is built bespoke across three separate codebases: iOS app, backend, and web app. Adding a single field, rule, or report type requires coordinated engineering work in all three. Each new report is a multi-month project, and the cost compounds as the catalogue grows.

Business goal: adding a new report should be primarily a content project with bounded engineering glue, not a re-implementation across three codebases.

Objective: speed — speed-to-capture and speed-to-report, owned by Plans — in service of maximizing scans and data captured.

- Idea-to-prototype: hours, not weeks. Idea-to-rollout: days or weeks, not months.
- Content work cannot depend on an engineer doing all of it, but engineering involvement is still expected.
- Full non-engineer self-service authoring is not a goal in itself.
- The data layer should be harmonized: the data is the same regardless of which customer captures it.

#### 2. What We Are Building

Property reports fall into three families:

**A — Regulated / Standardized, inspection-driven**

- Initiated by: National regulator.
- Driven by: New surveyor inspection data.
- Examples: FR Gaz, Electricity, Lead, Asbestos, Termite; DK Condition, Electrical.
- Notes: support bespoke options / feature flags on top of regulated reports.

**B — Regulated / Standardized, computation-driven**

- Initiated by: National regulator.
- Driven by: Calculations on existing property data.
- Examples: FR EPC/DPE, DK EPC, Loi Carrez.
- Notes: support bespoke options / feature flags on top of regulated reports.

**C — Bespoke customer reports**

- Initiated by: Individual customer.
- Driven by: Either inspection or computation.
- Example: hypothetical Danish Klimasikringsrapport.

The distinction is useful but not a hard boundary. Platform support is not sequenced per family. The useful distinction is who defines report content: regulator/external standard versus a specific customer.

Another useful lens is how much ownership Plans takes downstream: state management, review/approval, final artifact, regulator/customer callbacks. Less downstream ownership means Plans may hand off JSON/API data and the customer assembles the rest.

The board direction is: **be deliberately opinionated about capture, and more flexible downstream**. Capture must be reliable because it happens onsite and cannot be redone. Visualization and artifact layers should allow more flexibility and faster iteration.

All report families should share the same outer shape: Report, lifecycle, facts/evidence, validation, deliverables, and bundle/platform boundary.

### Alignment 2 — v1 Goal

The overall v1 goal is **8 reports as soon as possible**.

Concrete commitments with Botjek are fluid. It is not an external constraint that DK Electrical / Condition must be delivered by September 2026. Exact timeline commitments and roadmap depend on weighing tradeoffs across tracks.

Definition of done and implementation scope for DK Electrical / Condition are defined per-report, like all FR reports and future reports.

### Alignment 3 — Operating Model

#### Who Defines Reports

- Plans owns all report definitions.
- Customers may request and fund bespoke reports, but they do not self-produce report definitions.
- Most reports have a core driven by regulatory requirements.
- Customer-requested reports should ideally have an explicit funding/ownership model before work begins.

#### Who Does the Work

- Surveyors are trained, authenticated professionals.
- Inspections require an iOS device for onsite LiDAR/3D capture.
- A single Case may involve multiple surveyors at different times.
- Each Report can have its own assignee and lifecycle, and the assignee can change.
- Cases enter the system either manually or through customer IT imports.

#### Who Delivers

- Plans is responsible for producing the report/output artifact, per report.
- The responsible surveyor owns report content.
- Plans provides workflow, validation, artifact generation, audit trail, and delivery/submission infrastructure.
- Downstream customer integrations are currently custom per customer.

#### Reusable Property Data

The platform supports a property model that lives once per Home and is shared across reports and cases over time. Reusable property data such as geometry, some materials, and some technical installations should be consumed from the shared property model instead of recaptured.

What lives in shared property state versus a specific report is a per-fact decision.

Data ownership matters: if a customer owns building data, it may be reusable, but customers may not want their data used by a competitor later.

### Alignment 4 — Product Commitments

These user-facing commitments shape customer expectations, sales conversations, and support promises.

#### For Surveyors Onsite

- Partial capture is first-class.
- Surveyors can save incomplete facts and incomplete reports, then finish later.
- Onsite + offsite continuity supports lab phases, follow-up reviews, and office completion without redoing onsite capture.
- Offline scope remains open.

#### For Office Users / Reviewers

- Cross-platform report state: the same Report can move from iOS onsite work to web review.
- Picture-first and quick-create flows: a report fact can start from a photo before all structured fields are known.
- Auditable evidence trail: every report fact records who/when/where/device metadata.

#### For Plans

- One country/regulator alignment per report.
- Customer-specific UX customization can exist; regulated deliverable semantics are not customizable per customer for now.
- Adding a new report is primarily content work with bounded platform glue.
- Output and delivery ownership remains open.
- Complete the data area once started: avoid partial handovers that later require Plans to build the whole thing anyway.
- The downstream consumption/visualization layer must support rapid experimentation without inheriting capture-layer reliability constraints.

### Alignment 5 — Success and Trajectory

Success is not just shipping the first reports. Success means making each additional regulated report cheaper to add:

- Subsequent reports do not require a new multi-month project.
- Adding a field does not require coordinated changes across three codebases.
- A new country's regulatory diagnostic does not require re-architecting.
- Bespoke customer reports can be priced and delivered with predictable scope.

Another success criterion is estimation quality: after the first reports, the team should be able to distinguish content work, bounded platform glue, bespoke delivery/integration work, and complexity drivers.

Longer term, forward-deploy engineers could own reports at the report level.

Productization sequence:

- **Opinionated**: start with one opinionated flow per report; avoid premature generalization.
- **Packaged**: reports become repeatable, packaged building blocks.
- **Modular**: standardized modules an FDE can compose to assemble a report.

Trajectory and sequencing are deferred until cross-track sequencing is known and agreed.

### Alignment 6 — Open Questions and Out Of Scope

#### Open Questions

1. **Report flow and customer customization**
   - Are we fine enforcing a specific/opinionated flow per report, where every customer using that report gets the same flow?

2. **Bespoke / custom work constraints**
   - Do we want to explicitly stop building custom/bespoke things like current EPC/Botjek work?
   - Where is the line between generic and custom?

3. **Output, delivery, and completion**
   - Does Plans fully produce artifacts such as regulatory XML, signed PDF, and structured data?
   - Or does Plans provide integrable structured data and customers assemble/integrate final output?
   - When is a report complete or good enough?
   - Is completion iterated with the first customer/design partner until organization-wide adoption is ready?

4. **Prototype speed and forward-deploy ownership**
   - If idea-to-prototype should be hours, does that mean prototyping directly on production?
   - Should a report be fully producible by a forward-deploy engineer?
   - Should the FDE decide UX/UI and data shape/model for that report?

5. **Data capture vs visualization**
   - Should the core platform own only raw capture and calculations, or also derived/computed data?
   - Where does visualization start?
   - Should visualization be isolated per report or cohesive across reports?
   - Is the optimized boundary: data capture and calculations = core platform; report making/artifacts = FDE-owned?

6. **Scan ownership**
   - Is scan ownership per Home visit or per Case?

7. **Offline scope**
   - How much capture/review must work offline?
   - Earlier drafts committed to fully offline-first capture and review, with final handoff requiring internet.

#### Out of Scope For Now

- EPC/DPE re-platforming.
- Non-engineer authoring tools.
- Multi-tenant per-customer branded UX.
- Cross-report dependencies.

When in doubt:

- Business framing wins in the alignment brief.
- Engineering depth wins in `engineering-overview.md`.

## Shared Vocabulary

### Overview

These nouns are conceptual primitives, not storage commitments. The data model decides where each one lives.

### Nouns

**Address**: the legal locator: street, postal code, country. Durable across time; persists even when the building is replaced, rebuilt, or renumbered.

**Home**: the physical building/property at an Address. A Home has Home state: spatial model, materials, technical installations, thermal properties, and related data. Home state evolves over time. One Address may have multiple Homes over history; one Home lives at one Address at a given time.

**Case**: an inspection commission for one Home. A Home can have multiple Cases over time. Each Case has one or more Reports. The Case lifecycle is derived from Report lifecycles. Case owns thin admin scaffolding; per-report work lives at Report level.

**Report**: one regulated diagnostic, computation-driven report, or bespoke customer report produced for a Case. Each Report has its own assignee and lifecycle, so one Case can produce several Reports assigned to different surveyors.

**Inspection task**: a unit of surveyor work defined by a ReportBundle's task list. Each task references a Form/CaptureType and may carry visibility or completion logic. In v1, tasks are transient runtime constructs computed from ReportBundle definitions; they produce persistent Captures.

**ReportBundle**: the definition of a Report type. Specifies inputs, computations, validations, and output format. ReportBundles are per-report content authored by Plans. They are the per-report glue/template, not the foundation.

### How They Relate

Hierarchy:

Address -> Home -> Case -> Report

The Address outlives buildings. The Home outlives any single inspection. The Case orchestrates one commission. Each Report is independently assigned and tracked. ReportBundles define what each Report type contains. Inspection tasks are the units of work that populate Reports.

### Foundation Scope

Decided now:

- What Case, Report, Inspection task, and ReportBundle are conceptually.
- How they relate.
- Where each lives.
- Their lifecycles and ownership.

Not decided now:

- The content of any specific ReportBundle.
- Fields, validations, or layouts for specific reports.
- UX/workflow per report type.
- Task orchestration and surveyor app flow.

## Supporting Notes

### Loose Sticky-Note Questions

- What about reports spread over time for the same address: one level higher, a case lifecycle nuance, or something else?
- Should there be a noun for spatial aspect, e.g. spatial model / spatial Home data blob, with each Case having only one regardless of report count?
- Assignee per report: each assignee can only see their report.
- What do we optimize for?
- What is the timeline?
- We know different reports have different output formats, and we do not yet understand the full breadth of what could be required.

### Open Decisions Frame

The board also contains an "Open decisions" frame. Its questions are represented canonically in Alignment 6:

- Opinionated report flow vs customer customization.
- Constraints on bespoke work.
- Output ownership and completion.
- Prototype speed and FDE ownership.
- Core platform boundary between capture/calculation and visualization/artifacts.
- Scan ownership.
- Offline scope.

### Omitted Duplicate / Retired Records

The board includes older working-record documents and a retired goals document. These were not exported in full because the canonical Alignment 1-6 documents supersede them:

- Working record (2026-06-01) — Alignment 1 — Problem and Report Families.
- Working record (2026-06-01) — Alignment 2 — v1 Commitment.
- Working record (2026-06-01) — Alignment 3 — Operating Model.
- Working record (2026-06-01) — Alignment 4 — Product Commitments.
- Working record (2026-06-01) — Alignment 5 — Success and Trajectory.
- Working record (2026-06-01) — Alignment 6 — Open Questions and Out Of Scope.
- Alignment 0 — Goals — RETIRED.
