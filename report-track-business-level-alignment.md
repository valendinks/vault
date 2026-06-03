---
type: Note
related_to:
  - "[[Direction: 3D + Forms + Metadata]]"
  - "[[Reports Foundation Alignment]]"
---

# Report Track: Business-Level Alignment

## Bottom line

We aligned on the direction for the report platform: optimize for **time to new report** while preserving a harmonized core data model. Reports are the commercial wedge; captured building data is the strategic asset.

The team should not build a fully generic report system before shipping real reports. It should build generality iteratively, using the first Danish and French reports as forcing functions. The open question is not whether modularity matters, but how much to invest in it now versus how much to defer until the first reports expose the right abstractions.

## Resolved

### 1. The primary metric is time to new report

Adding or changing reports must become much faster than today. The goal is internal speed for Plans, not customer self-service report creation.

Implication: configurability is a means to speed. It is not the goal by itself.

### 2. Reports are a means to capture building data

Plans makes money when it captures more useful building data. EPCs, PDFs, XMLs, and other report outputs are commercial vehicles for getting that data captured.

Implication: we should minimize the total company cost of producing reports while maximizing useful, reliable building data.

### 3. The core data model must stay harmonized

If a field exists, it should mean the same thing across customers, markets, and reports. A U-value, electricity price, area, condition status, or material field cannot mean different things depending on who captured it.

Implication: customers may vary in when or whether they capture a field, but the field's meaning and shape should not fork casually.

### 4. Capture needs stronger reliability than output generation

On-site capture has a high cost of failure because the surveyor often cannot return to the property. Downstream artifacts can tolerate more iteration because they can be recomputed, revised, or regenerated.

Implication: platform quality should be highest where data is captured, validated, and stored.

### 5. Consumption and visualization need high flexibility

The layer after capture must support fast experimentation. This includes PDFs, XMLs, customer-facing artifacts, downstream integrations, APIs, and prototypes for sales or product discovery.

Implication: Core should make captured data easy to consume without forcing every downstream artifact into the same codebase or release path.

### 6. Generality should be built from real reports

The team should not build a complete generic platform first and only then build reports. It should build the first reports, extract common patterns, and refine the platform as more reports stress the abstractions.

Implication: the first reports are not just deliverables. They are test cases for the platform.

### 7. Standardized and bespoke reports both exist

Some reports should become standardized Plans-owned products used across customers. Others may start as bespoke prototypes for one customer or market, then later be promoted into standardized products.

Implication: bespoke work is allowed, but it should not silently create permanent parallel report models for the same underlying concept.

### 8. DK electrical and condition reports should copy the existing flow

For the near-term Danish reports, speed to market matters more than UX innovation. The adoption lever is familiarity: make it look and behave like what the users already know.

Implication: do not reinvent the current workflow unless there is a direct need for the new platform.

### 9. External timeline commitments are fluid

September 1, 2026 is an aspiration, not a hard external constraint. The real business requirement is to ship the eight reports as soon as possible, preferably on the new platform, because they block commercial progress.

Implication: sequencing should optimize Plans' total speed and commercial leverage, not defend a date that is not externally fixed.

## Open tensions

### 1. How generic should the first implementation be?

Everyone agrees on the long-term cone: many markets, many data capture methods, many report outputs. The disagreement is cost and timing.

- Product/commercial pressure favors shipping specific reports quickly.
- Platform pressure favors reusable primitives, report bundles, and configurable capture.
- Engineering needs enough real report complexity to design useful abstractions.

Decision needed: define the minimum platform generality required before the first reports ship.

### 2. Where is the line between standardized and bespoke?

The old split between "regulated" and "custom" is too weak. A better split is:

- **Standardized report:** Plans owns the product definition and can sell it across customers.
- **Bespoke report:** built for a specific customer or market need, with a path to productization if it repeats.

Decision needed: define when a customer-specific variation becomes a feature flag, a sidecar, a report fork, or a new report.

### 3. Who owns report content end to end?

Engineering should not need deep domain knowledge for every report. But someone must own the report's data requirements, output, acceptance criteria, and customer fit.

Decision needed: assign one accountable owner per report family.

### 4. How separate are capture and visualization?

Capture and visualization can be conceptually separated, but they are operationally linked. Adding a capture field often requires changing output logic, and changing an output often reveals missing capture fields.

Decision needed: define the interface between capture config, core data, calculations, and artifact generation.

### 5. What is the role of non-engineers in report creation?

The goal is not that customers create reports. It may be useful that domain experts inside Plans can configure capture fields or outputs with light engineering support.

Decision needed: decide whether to optimize for "no engineer involved" or "low engineering involvement." These are different systems.

### 6. What must ship first?

The team discussed Danish reports, French reports, 3D editing, Mine Working, and broader platform work. All cannot be first.

Decision needed: choose the first commercial forcing function and resource split across reports and 3D.

## Action points

### Henrik Holm

- Own all French reports end to end.
- Provide the required capture fields by level: case, building, room, element, annotation, and any other relevant hierarchy.
- Define the required output for each French report, not only the capture requirements.
- Identify where engineering support is needed for scaffolding, access, integration, permissions, or artifact generation.

### Anders Valentin

- Define the product requirements for the Danish reports: what questions are asked, where they are asked, and what output is considered adoption-ready.
- Confirm with Thomas the exact integration target and delivery expectation for DK electrical and condition reports.
- Decide whether the near-term DK output is XML, API push, PDF, JSON, or another handoff.

### Engineering team

- Scope the first platform version around real reports, not an abstract generic system.
- Estimate two paths: a specific report-first path and a more configurable platform-first path.
- Identify which primitives are shared across Danish and French reports.
- Define the boundary between capture configuration, core data model, calculations, and artifact generation.
- Flag where a requested report variation would fork core data semantics.

### Leadership / MC + AV

- Decide the initial sequencing across Danish reports, French reports, 3D editing, and broader platform generality.
- Decide whether four engineers on reports and two on 3D is the intended starting assumption.
- Decide whether Danish and French reports should run in parallel or whether one market should be the first forcing function.
- Decide the acceptable trade-off between speed, genericness, and rework.

## Topic structure

### Report taxonomy

The useful taxonomy is not simply regulated versus custom. The better distinction is ownership and coupling:

- **Standardized inspection reports:** Plans owns the report definition; spatial mutation is limited or absent.
- **Spatial/computation-heavy reports:** full geometry, thermal envelope, or home-state calculations affect the output.
- **Bespoke/customer reports:** created for a specific customer need; may later become standardized.

The key architecture distinction is whether the report mutates or depends on the spatial model. Reports that require full geometry and home-state calculation place different demands on UX, data lineage, and platform boundaries than reports that only need anchored inspection data.

### Data model and harmonization

The team wants flexibility in capture without losing downstream meaning. A customer may skip a field, capture it on-site, capture it off-site, or use only part of a report. But if the field is present, it should have one meaning.

This protects downstream use cases: banks, insurers, real estate agents, and future data consumers need stable semantics. Without harmonization, Plans or the customer must reconcile parallel concepts later.

### Capture, calculation, and visualization

The platform can be understood as four layers:

1. **Capture:** questions, measurements, photos, annotations, geometry, room or element data.
2. **Core data:** stable stored building facts and report-relevant fields.
3. **Calculation:** derived values, geometry calculations, thermal logic, report-specific engines.
4. **Artifacts:** PDFs, XMLs, JSON, APIs, customer-facing screens, sidecar documents.

Capture and core data need the strongest reliability. Artifact generation needs more flexibility and faster iteration.

### Standardization versus customization

The team sees commercial value in both standardization and flexibility, but the value differs by layer.

Standardization sells reliability, compliance, harmonized operations, and confidence that users are done. Flexibility helps expansion into new markets, new report categories, and customer-specific outputs.

The proposed middle ground is one Plans-owned report model with optional fields, feature flags, sidecars, or customer-specific outputs where needed. Avoid separate parallel report models unless the underlying product meaning is truly different.

### Platform modularity

The long-term direction is modular: reusable capture primitives, reusable report components, and outputs that can be assembled faster than today. The short-term implementation should avoid pretending the full generic system is already known.

The team should start with real reports, extract primitives, then use the next reports to stress-test and refine them.

### Timeline and sequencing

The team is in a hurry because eight reports block commercial progress. But the date itself is not the only decision variable. The real sequencing question is how to maximize total speed to the eight reports while also building the platform in the right direction.

Likely sequencing options:

- **Report-first:** build Danish and French reports quickly, then extract patterns.
- **Platform-first:** invest in generic primitives, then build reports faster later.
- **Parallel:** one or two engineers build concrete reports while others build shared primitives, with learnings flowing both ways.

The meeting did not resolve which option to choose.

## Decisions still needed before engineering estimates

1. Which reports are first: Danish, French, or parallel?
2. What is the minimum platform foundation required before the first report ships?
3. Who owns each report family end to end?
4. What are the exact outputs for each first report?
5. What is the acceptable amount of rework if a report ships faster?
6. Which parts of report creation should be configurable by domain experts?
7. Where does Core ownership stop and FDE/report ownership begin?
8. Which customer-specific variations are allowed without creating a new report model?

## Working stance

Proceed with a report-driven platform build.

Use the first reports to force the architecture, not to bypass it. Keep core data semantics stable. Keep capture reliable. Keep artifact generation flexible. Make explicit trade-offs when speed requires a report-specific shortcut.
