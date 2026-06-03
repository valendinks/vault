---
type: Note
---
# Direction: 3D + Forms + Metadata

We can proceed with the engineering kickoff. The direction is firm enough to start both tracks, while the spikes should refine estimates, data lineage, and implementation details rather than reopen the product direction.

## Timing

Proposed answer to the team:

- MC/AV can answer the three blocking 3D questions in writing now, in this note.
- The Slack target was kickoff on Monday, June 1, or Tuesday, June 2, 2026, with 3D and report/forms/metadata introduced to the full engineering team together. If this is shared after that window, replace this with a new concrete kickoff date before sending.
- Spikes should start immediately after kickoff.
- Estimates should happen in two passes: high-level estimates before spikes, then more precise estimates after spike results.
- Calendar time depends on shared staffing across the two tracks. AV and Henrik should confirm a concrete sequencing proposal by EOD before this is sent.

## Firm 3D decisions

### 1. Accuracy vs. smoothness

We agree with the proposed resolution: derived-model generation should be modular, so we can iterate on or fork reconstruction approaches without rethinking the platform.

The internal model should preserve capture truth. External or derived models may be smoothed when the use case calls for it. The exact reconstruction algorithm is an engineering-track decision based on near-term product need, estimate, and spike results.

This should not become a separate big-bang architecture project. It fits naturally with the already-decided separation between internal models, external models, and envelopes. The data lineage is still to be decided inside the track.

The important engineering distinction is not "accurate vs. pretty." It is "locally correct vs. globally correct." Measured areas must remain authoritative and computed in Core. USDZ or visual output is for validation and communication, not precision measurement.

### 2. Edge-case investment

We should support every difficulty level we claim to support, but we should not automate the hard tail before we understand its cost.

The stance is:

- Capture complex buildings greedily, because the home visit is the one-way door.
- Do not optimize derivation and automation for rare complex buildings now.
- Provide a measured manual fallback for cases outside the automated path.
- Make the supported difficulty boundary explicit in product language.
- Revisit the edge-case SLA and automation investment after we have real case volume.

This resolves the apparent disagreement: Marina/AV are right that we must stand behind what we claim; Martin is right that we should not spend platform capacity automating rare cases prematurely.

Open product item: define the exact difficulty boundary we claim in V1.

### 3. Internal scan fidelity as reality capture

Yes: we care about internal scan fidelity as reality capture.

"Good enough for EPC" can guide derived models, report-specific interpretation, and short-term automation. It should not lower the capture floor. The visit is the only reliable chance to capture the home as it was on that date.

With RGBD, the answer becomes clearer: RGBD is the retained reality-capture source, and parametric models become derived, re-derivable artifacts. Without RGBD, we would need much higher confidence in the parametric representation, because it would carry more of the long-term truth.

For the next phase, treat RGBD as the capture baseline, not only a POC for LOI. The product surface can stay limited at first: use RGBD/USDZ/splat views mainly for rough verification that the property was captured, while Core owns the derived data needed downstream.

## Firm report/forms/metadata decisions

### 1. Report flow

Yes, each report should have an opinionated flow at first. All customers using the same report should get the same flow unless we deliberately create a new report variant.

We should not build a general flow engine now. The model should be fixed lifecycle points plus configurable tasks:

- Fixed lifecycle points: scan, RGBD, room plan, required materials, and other capture-floor components.
- Configurable tasks: forms, notes, annotations, fields, and report-specific questions inserted around those fixed points.

### 2. Bespoke customer work

We should not ban bespoke EPC/Botjek work. We should constrain where it lives.

Core should own raw capture, structured data, measured areas, generic classification, derived geometric data, and stable contracts. Report-specific interpretation and presentation should live above those contracts, owned by the report/FDE layer.

Bespoke work is acceptable when it teaches us and stays above the Core contract. It should graduate into Core only when a second independent customer needs the same capability and we can design it against at least three likely cases.

### 3. Report ownership and "good enough"

The report owner/FDE and the design partner define the report output.

Core defines capture completeness and data contracts. It does not define when a report is commercially or regulatorily good enough.

A report is good enough when the design partner can adopt it across the relevant organization, and the jurisdiction-specific calculations and output have been validated for that use case.

### 4. Prototyping and production

Directionally yes: the report layer should be able to move fast enough that idea-to-prototype takes hours, not weeks.

But "prototype on prod" needs guardrails before we use it as an operating principle:

- No experimental changes below the Core contract.
- Report prototypes must be isolated by report, customer, or feature flag.
- Production data must not be silently migrated by report experiments.
- A report can be single-threaded by an FDE above the contract, but shared concepts must remain shared.

FDEs can decide report UX, report-specific data shaping, and presentation. They cannot unilaterally change the Core canvas, canonical data model, or shared contracts.

### 5. Core vs. visualization boundary

The proposed boundary is right, with one correction: "calculations" split by type.

Core owns:

- Raw capture
- Structured property data
- Measured areas
- Minimum derived geometric data, such as JSON to 3D model and RGBD to splat
- Generic classification and data contracts
- Shared concepts such as property, case state, report status, and assignee when they must work across reports

The report/FDE layer owns:

- Report-specific interpretation
- HBEMO logic
- Thermal/junction calculations when they depend on jurisdiction or report meaning
- Material-to-catalog binding
- Proposals
- Text
- Report presentation

Thermal envelope overview is split: envelope geometry and measured areas belong in Core; the report-specific overview and explanation belong in the report layer.

### 6. Scan ownership

The canonical scan should attach to the property, not only to a visit or case.

Cases, reports, and surveyors should reference the property scan. The hard multi-actor problem remains open: if two surveyors visit the same property for different reports, we still need rules for who scans, how conflicts reconcile, and when a new scan supersedes an old one.

V1 should keep this simple: bind the RGBD scan to the highest-coverage flow first, avoid multiplayer/reconciliation work, and gather frequency data before investing further.

## Product decisions deferred

- Exact V1 difficulty boundary for buildings we claim to support.
- Manual fallback packaging, price, and SLA.
- Whether and how RGBD/point-cloud surfaces should appear to end users beyond verification.
- Report adoption threshold per customer: what proof is enough before rollout to the whole organization.
- Renewals/revisits: when a previous scan remains valid and when a new visit requires a new scan.

## Engineering decisions deferred

- Data lineage between internal model, external model, envelope, measured areas, and report artifacts.
- Reconstruction algorithm and smoothing strategy.
- Spike scope for room-level area verification and material override verification.
- Core contract shape for generic classification and report-specific interpretation.
- Guardrails for report-layer prototyping in production.
- Multi-actor scan reconciliation.
- Online/offline split for native app, web, and capture tasks.

## What the team should estimate or spike first

1. Internal/external/envelope model separation, including data lineage.
2. RGBD capture baseline and minimum verification surface.
3. Room-level measured areas and material override flow.
4. Report lifecycle points plus configurable task insertion.
5. Core contract for generic classification vs. report-specific interpretation.
6. Production-safe report prototyping guardrails.

## Definitions

- Capture: the iOS capture flow and on-site completeness checks.
- Core: shared, jurisdiction-agnostic capture data, derived geometry, measured areas, generic classification, and contracts.
- Report layer / FDE layer: report-specific interpretation, UX, calculations, text, proposals, and presentation.
- Capture floor: the minimum required data for a valid capture.
- Canvas: the Core-owned property model and generic attributes that reports build on.
- Interpretation: the report-specific meaning assigned to Core data.
