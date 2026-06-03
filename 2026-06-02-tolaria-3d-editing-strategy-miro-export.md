---
date: 2026-06-02
topic: tolaria-3d-editing-strategy-miro-export
source: https://miro.com/app/board/uXjVHaYzk00=/
---
# 3D Editing Strategy - Miro Board Export

Source board: [3D Editing Strategy Miro board](https://miro.com/app/board/uXjVHaYzk00=/)

This is a faithful Markdown extraction of the Miro board. Exact Miro document content is preserved where available. Visual frames, diagrams, sticky notes, and unsupported shapes are summarized from Miro context and visible board text.

## Board Overview

Source: [board overview](https://miro.com/app/board/uXjVHaYzk00=/)

The board is a strategic and technical workspace for removing "3D editing" as a distinct, heavyweight workflow step. It frames the problem as context loss between onsite capture and office review, then explores an integrated survey workflow where the surveyor captures, reviews, validates, and corrects building geometry and energy-performance data in context.

The board follows a problem-to-solution-to-implementation structure:

- Foundation documents define operational constraints, correctness, and terminology.
- Current workflow diagrams show where information is lost when tasks are split across onsite and office phases.
- Proposed workflow diagrams collapse work into the onsite context and keep final review possible.
- Technical architecture frames separate internal scan data from derived external, thermal, measured-area, and EPC/DPE models.
- Processing split diagrams and tables analyze which geometry and reporting tasks move to iOS, shared geometry core, or backend.
- Validation and engineering frames list hypotheses, proof-of-concepts, and open implementation areas.

Core themes:

- Optimize for the surveyor doing the work themselves, while still supporting handover in a minority of cases.
- Preserve the internal scan as observed reality; do not mutate it to satisfy external or thermal model rules.
- Make derived models explicit so users stop editing internal geometry to fix external-area or EPC outputs.
- Move geometry review and correction earlier, while the surveyor still has context.
- Keep backend authority for persistence, validation, regulatory/reporting calculations, and networked enrichment.

## Foundation / Preread

### Constraints, Assumptions, Choices

Source: [Constraints, assumptions, choices](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671505428903)

# ✅ Constraints, assumptions, choices

# Operational

- ✅ Implementation of Plans must not require changing the operations inside the company
  - ✅ We can't expect our customers to hire and train an office team of professional case handlers (and specialized in 3D tooling).
- ✅ We **don't optimize** for office staff/surveyor split, we **optimize for and encourage** the surveyor doing everything themselves, but in some small percentage of cases it can be a different human, so we **support** it. We don't have clarity that we can remove support for handover, but don't prioritize handover-related features in the product.
- ✅ We can't expect surveyors to become specialists in complex 3D-specific tooling, however a minor learning curve is acceptable as part of switching to using Plans.
- ✅ We can't expect them to spend more time **in total** to do existing work (past the learning curve)
  - ✅ **All else equal**, we would rather have low onsite time and higher off-site time (to allow more visits per day)
- ✅ Turnaround time for our customers can't be increased from the SLAs they have today
- ✅ The surveyor is ultimately legally responsible for the content of the reports

# Hardware onsite

- ✅ We need to support capture on an iPhone *(iPad-only is a no-go)*
- ✅ Our users will always own a large-screen device and likely have it with them - tablet, iPad, or laptop.
- ✅ A user is allowed to have a single device onsite that is an iPad.

# Connectivity

- ✅ We are in the real world:
  - ✅ Mobile internet coverage varies deeply based on country and location. Denmark has one of the best mobile coverages.
  - ✅ Even when in a coverage area, network access can be limited to low-speed technologies such as EDGE, or be degraded in parts of the home, fx in basements.
- ✅ Experience when in offline needs to be subjectively great
- ❓The user needs to be able to survey a home efficiently when network coverage is spotty or missing throughout the home. When the network is principally not available at the time of visit, degrading into reasonable fallbacks is allowed, but the user must not be prevented from using the app to capture survey data. Connectivity issues can't result in data loss
- ❓Our software is *not* a fully offline, desktop program. Not all functions have to be available on no connection, but the user needs to be able to survey a home.

# Quality

- ✅ For measurements relying on LiDAR and own calculations, there is an acceptable margin of error (3%)

# What are we?

- ✅ "Digital twin" is part of our value proposition (workspace canvas) ~~OR we provide 3D models relevant for reports/artifact~~ - we are glue - this is why they choose us
- ✅ We gather and structure building data - best cost-vs-value ratio
- ✅ Today the best tradeoff is - from Lidar scanning on Apple Pro, consumer-grade devices ("wave the phone around"), we produce correct spatial and areas relevant for given reports and a digital twin of the building - positioned differently depending on the customer.
- ✅ Our aspiration is to maximize usefulness of the data
- ✅ We serve building surveyors to give them the best tools to solve their survey jobs to be done, while maximizing usefulness of data ([@Martin Collignon](3458764543730172538) will rephrase it)
- ✅ Aspirationally we want to be a software company, not a service business
  - ✅ When we have to have human-in-the-loop at Plans, it's a product failure
  - ✅ We will (never) be a human-in-the-loop back-office for completing/reviewing reports, texts, proposals, ... ("We help you do your job")
  - ✅ We can, in a small percentage of complex cases, be a human-in-the-loop for specifically geometric reconstruction (when our product fails). However it mustn't a trained energy consultant.
    - **"geometric reconstruction" to be defined depending on product workflow**

### Definition of Correctness

Source: [Definition of correctness](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671505428904)

# Definition of correctness

An output is correct when **both** of these are true:

- Spatial models and areas are correct within the acceptable error of margin
  - can be evaluated and measured in %
- Output is *subjectively* trusted by the user\
  (here to add: trust is the key value prop for surveyors to adopt our product)

- **'trust' is the primary bar/principle**, with spatial visualization serving as a solution to achieve trust rather than being the goal itself. Factual correctness remains independently required for EPCs and regulatory outputs.

### Terms

Source: [Terms](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671505428905)

# Terms

**Onsite** - while at the surveyed property, by surveyor

**Offsite** - not at the surveyed property, by surveyor or a different human

**In the app** - can be done on an iPhone screen

**On the big screen** - on iPad or desktop

**Offline** - doesn't depend on internet connection

**On device** - [avoid using, can be understood differently depending on context]

**Thermal envelope gap** - water tightness 'leak' in the outer spatial model

**Digital twin** - buzz word, let's try not to use it

### Visible Preread Notes

Sources: [preread frame](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671557561568), [RoomPlan note](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764672198315803), [value proposition note](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764672186245173)

The board includes a "preread here" area and several standalone notes:

- Define what "3D editing" means.
- Clarify the difference between supporting a workflow and optimizing for it.
- Commit to RoomPlan today as the best available tradeoff, define "ready" or "good enough," and explain why RoomPlan is the best tradeoff.
- Plans translates atoms to bits at the best available cost/value ratio. Easy, cheap capture and structured building data are the core offering.
- Capturing as much raw data as possible improves future usefulness.
- The product replaces pen, paper, and spreadsheets, so it must meet a near-100% reliability bar.
- Spatial visualization helps users verify that Plans did the job correctly and helps them deliver more to their audience.
- Plans should be software-first. Humans can step in temporarily or for edge-case geometric reconstruction, but should not replace user expertise.

## Current Workflow

Source: [Todays workflow](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671505622136)

The current workflow is a left-to-right process split across onsite capture and office review. It maps a property assessment and EPC/report workflow from case assignment to final export.

Visible summary:

- The user receives a case and comes onsite.
- Onsite work captures room geometry, story geometry, cross-story elements, orientation, materials, B-factors, EPC areas, and technical installation data.
- Many onsite capture activities have no review/correction path or only limited review.
- Office work then reviews and corrects room geometry, story geometry, cross-story data, orientation, materials, B-factors, EPC areas, installations, texts, and exports.
- Detailed correction tasks include adding/removing windows, hatches, doors, fixing ceilings, dormers, gaps, room rotation, knee spaces, and missing rooms.
- The office phase depends on context from satellite imagery, notes, documents, previous EPCs, BBR, and the digital twin.
- Red information-loss indicators mark handoff points between phases.

Standalone visible note:

> First, do ALL tasks on device\
> then, do ALL tasks again on web, in a completely different system\
> Each task is broken up into two parts separated by time, space, across multiple humans\
> Context is lost in transition

Main problem captured by the frame: work is duplicated across device and web, split across time and sometimes people, and context is lost before the user can validate or correct the final outputs.

## Proposed Workflow Without Separate Office

Source: [Workflow without separate office](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671505622222)

The proposed workflow keeps the work co-located in time, place, and human ownership. A single user handles case assignment, onsite visit, geometry capture, orientation, materials, B-factors, technical installations, per-report data capture, review, preparation, and export.

Visible summary:

- The user receives a case and comes onsite.
- Property geometry capture creates the digital twin.
- Orientation capture/review/correction, materials capture/review/correction, B-factor review/correction, technical installation capture, and per-report data capture happen in the same central process.
- Prior EPC data, documents, and satellite imagery feed context into the workflow.
- Report preparation splits into EPC, Report A, Report B, and other export paths.
- Final export paths include E10/BT, Report A, and Report B.
- Some work can remain onsite/offsite flexible, but context has already been ingested.

Standalone visible note:

> Each task is done ONCE onsite, in-context\
> Final result can still be reviewed & corrected at the end\
> If report is viewed offsite, all context has been already ingested into the digital twin

Main design intent: remove the separate office step as the primary workflow. Final review can still happen, but the meaningful survey context has already been captured into the product.

## Removing 3D Editing As A Step

Source: [How do we remove "3D editing" as a step?](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671505622273)

# How do we remove "3D editing" as a step?

## **Three bets:**

- **Bullet-proof AR space**
- **Better input data -> better quality (fewer edits needed overall)**
- **Move work to earlier, ++in-context++ (same work feels lighter)**
- **Building the correct mental model for our users - internal vs external (avoid unnecessary edits)**

### **Bullet-proof AR space**

- No bugs ([@Marina Vatmakhter](3458764569556278121) will describe better)

### **Better input data -> better quality (fewer edits needed overall)**

- The flow of data, exemplified:
  - correct internal walls result in more correct internal ceilings
  - proper ceiling fallback for advanced internal ceilings
  - ceiling algo tolerance gives more correct internal ceilings
  - correct internal room geometry gives better story-level geometry
  - tolerances on story-level geometries
- We need to incentivize the right edits and disincentivize wrong edits (to hit correctness)
- Our assumption is that on story and cross-story level, you only review the floor plan / doll house with **rare edits** needed only for edge cases, where full 3d suite of tools is acceptable to use.

### **Move work to earlier, in-context (same work feels lighter)**

- Avoiding context loss reduces the weight of each edit (no handover + full context while onsite)
- As-you-go fixes feel lighter as opposed to batch-fixing everything at the end
- Targeted mobile-friendly tools to edit individual rooms - less overwhelming than full the 3d editing tool suite

### **Building the correct mental model for our users (avoid unnecessary edits)**

- Our promise - from internal room scans, we can produce correct spatial and areas relevant for given reports.
- Clearly separate mental models of internal, external, country-specific EPC logic (thermal envelope, LTL) and measured areas (report & country specific)
- What is an *internal* scan, what are walls. Gaps between rooms are not a problem to fix, they represent the walls that the camera can't see.
- It's about both rendering in a user-friendly way, and making correct *derived* calculations for the reports. (For example, for DK EPC we need a floor area including walls - rooms don't need to be extended to fill the gaps, instead the calculation need to derive correct area from the internal room scans)
- We arrive at the *external/thermal envelope model* from the *internal scan* as a starting point Right now our model mixes the two concepts together, and much of the confusion for what you should edit or not comes from that. User can't edit the *external/thermal evenlope* model right now, so they end up fiddling with the internal model to arrive at correct external model.

**The overall idea is: when full-story or full-dollhouse 3d editing is a rare occasion, and most houses are good-to-go with the above, it's actually OK to edit on a big-screen in those small percentage of edge cases.**

**3D edits still needed:**

- review odd shapes (indents) - suggestion-based on-device
- editing external volume and thermal envelope of the building is easy enough compared to complex room-level edits

**Materials and B-factors:**

- once you see the correct geometry, reviewing and tweaking materials and b-factors per-story becomes a trivial task

## Digital Twin Onsite

Source: [Digital twin onsite](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764670464744645)

This frame explores how raw RoomPlan scans and ceiling-cover data become a correct story-level 3D model and dollhouse view.

Core flow:

- Raw RoomPlan scans and ceiling-cover data enter the pipeline.
- The base technical pillar is coordinate system and placement consistency in AR and 3D.
- The frame asks how to move from potentially wrong room-level inputs to correct story-level outputs.
- Two theories are contrasted:
  - "Shit in - shit out": correct room data first, then merge into a story model.
  - "Shit in -> magic -> correct out": let story-level heuristics, AI, or ML fix room-level issues.
- Processing stages include room-level correction, story-level merging, automatic post-processing, review, and cross-story/dollhouse review.

Implementation notes visible in the frame:

- Basement splitting can happen at room level.
- RoomPlan merge alternatives may come from anchors or other merge strategies.
- Post-processing could be online or offline.
- Improved ceiling algorithms and tolerances may reduce downstream edits.
- Reprocessing triggers and editing frequency are open questions.
- Orientation may need satellite context.
- The system needs a path for unscannable rooms and material defaults.

Standalone visible notes:

- Mental framing idea: in EPC context, the scanning app promise is that the user scans inside and Plans produces the correct EPC thermal envelope.
- If the internal scan is correct and the user did their best, Plans should deliver on that promise.
- Internal scan correctness matters for robotics data; do not edit internal data.
- The external-envelope algorithm should be iterated as input/output.

## Home State Layers

### Home State Data Flow - Target Model

Source: [Home State Data Flow - Target Model](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671457766273)

# Home State Data Flow — Target Model

A layered home-state model, from iOS scan -> EPC/DPE views. See docs/home-state-data-flow-proposal.md.

## Layers (each owns one thing)

1. **Internal Scan** — observed interior reality (iOS LiDAR). Not distorted to satisfy EPC rules.
2. **External Area Model** — regulatory/projected external geometry.
3. **Thermal Envelope Model** — heat-loss boundary + thermal assumptions (material, B-factor, FR adjacency, orientation).
4. **Linear Thermal Loss** — junctions, lengths, ψ-values, derived from envelope.
5. **Measured Areas** — country/report-specific floor/area; calculated unless manually edited.
6. **EPC/DPE View** — read model over a HomeSnapshot of 1–5.

## Where material / B-factor / FR adjacency live

Today: on internal segments (from iOS). Target: **promoted onto Thermal Envelope surfaces** at import; iOS values are *initial thermal assignment candidates*, not the long-term owner.

## Orientation / inclination / azimuth

Derived from geometry, so each layer just has whatever its own geometry implies — not duplicated state. EPC reads from the envelope's geometry, since it can legitimately differ from the internal scan (knee walls, projected external lengths, ceiling vs roof).

## Edit policy

*Generated until touched. Canonical once edited. Linked to source always.*

Each derived object carries source refs + hash + origin (generated | editedGenerated | manual). Scan changes -> regenerate generated objects; mark edited ones stale / needsReview. Downstream edits never mutate upstream.

## Backend-authoritative calculations

- Calor owns EPC/DPE math. Kelvin should not maintain a parallel implementation.
- Side-effect-free preview endpoint (e.g. POST /homes/{id}/room-by-room/calculation-preview).
- Same calculation path used for saved reads, draft previews, and report export.
- Kelvin keeps local UI responsiveness, but authoritative numbers come from Calor.

## Phased rollout

1. Calor authoritative for calculations (preview endpoint, segment storage unchanged).
2. Expose external-area / envelope / LTL / measured-area read models.
3. Persist first-class tables; migrate segment material / B-factor / FR off internal scan.
4. iOS/Kelvin contracts split internal scan from initial thermal assignments.

### Home State Layers Diagram

Source: [Home state layers frame](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671463337945)

The diagram presents the same layered model visually:

- iOS Scan / RoomPlan output flows into Internal Scan, which is the source of observed interior reality.
- Derived, regenerable layers include External Area Model, Thermal Envelope, Linear Thermal Loss, and Measured Areas.
- A HomeSnapshot is a versioned bundle of all layers.
- EPC/DPE View is a computed read model over the snapshot.

Edit lifecycle:

- `generated`: auto-derived from upstream; regenerates when the source changes.
- `editedGenerated`: user version wins; upstream changes only flag stale/needsReview.
- `manual`: canonical from creation; no regeneration, provenance link only.

Invariants:

- Editing Thermal Envelope does not mutate Internal Scan.
- Editing LTL does not mutate Thermal Envelope.
- Editing Measured Area does not mutate rooms.
- Each derived object remembers its source and hash.

Visible problem statement:

- What is the 3D model actually?
- There is too much invisible magic, which weakens trust.
- Changing internal segments affects thermal envelope, scanned area, measured areas, and LTL at once.
- LTL, measured-area cutoffs, B-factors, materials, and related concerns are entangled on segments.
- Knee spaces do not have a clear home today.
- The current model is big bang and high blast radius: 3D algorithm changes can affect many outputs at once.

## RbR Client Processing Split

### RbR Client Processing Split Document

Source: [RbR client processing split](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671461839960)

# RbR client processing split

This note backs up the cadence diagram. Full detail lives in [docs/rbr-client-processing-split.md](https://github.com/lun-energy/sofia/blob/main/docs/rbr-client-processing-split.md).

## Intent

Move the scan-parsing and geometry-building half of the RbR pipeline onto iOS. Keep persistence, material resolution, measured/external areas, and reporting on the backend. The goal is an offline-first survey where the surveyor can scan, edit geometry, and pick materials without a network round-trip — and where the backend cannot silently overwrite client-confirmed geometry on the next reprocess.

## Owner split in one line

- **iOS** owns canonical onsite geometry + material intent.
- **Backend** owns validation, material resolution, persistence, and cross-client derivations (measured/external areas, reports, BBR/EPC).

## Processing cadence

**Per room scan** (iOS): parse RoomPlan JSON, build canonical room geometry (partial merge, attach windows/doors/openings), room-local best fit, wall graph, boundary classification, material intent, stable IDs + provenance.

**Per story — story complete** (iOS, provisional): same-story horizontal adjacency, envelope + b-factor defaults (air / ground / heated / unheated), vertical ceiling -> wall, ceiling type, gap *detection* (diagnostic only — no auto-mutation).

**Building complete — final reprocess** (owner TBD, deferred from first port): takes the current RbR model as input and returns a new RbR model. **Modifies the thermal envelope only — never the raw scan source.** Includes gap *repair*, knee walls, grade/plate splits, post-parse best fit, segment azimuths. These need full story/building context, so running them progressively is risky.

**Sync / confirm** (handoff): iOS sends *confirmed model + material intent*.

**Backend resolution + persistence** (backend): validate geometry + material refs, resolve material intent to home material IDs (create custom + home-specific child materials), persist snapshot.

**Online derivations** (backend, not shown offline): measured + external areas, U-values, reports, BBR/EPC.

## Why this split

- **Offline-first survey** — surveyor scans, edits, and picks materials with no network round-trip.
- **No silent overwrites** — confirmed geometry attaches to stable IDs and survives reprocesses; backend validates references but does not recompute.
- **Backend stays authoritative where it makes sense** — DB-backed material lifecycle, snapshots, regulatory derivations, BBR/EPC lookups.

## Building-complete reprocess constraint

The final reprocess is a pure **RbR-in, RbR-out** operation. It is allowed to mutate the *thermal envelope* (gap repair, knee walls, grade/plate splits, post-parse best fit, segment azimuths) but never the raw scan source. This protects the observed interior reality from being silently rewritten by envelope rules — and lines up with the home-state layering proposal where the internal scan owns observed interior reality and the thermal envelope is a separate derived model.

## Open questions

- **Client vs server split for story-complete and building-complete processing.** Still open. Story-complete is drafted as iOS so b-factor / adjacency defaults work offline; building-complete is parked as owner-TBD. Either band could be argued the other way. Decide before committing to the first port.
- **Vertical (story-to-story) adjacency** is not yet addressed. Same-story horizontal adjacency is iOS-owned; vertical adjacency needs at least two stories of context and likely belongs in the building-complete reprocess.

## Not in the first port

- The building-complete final reprocess described above, until the owner is decided. Until then, expose its outputs as explicit suggestions rather than silent mutations.
- A shared Rust/C++ geometry core. Only worth extracting later if backend, web, or Android also need to run the same geometry — not the case today.

### RbR Processing Split Diagram

Source: [RbR processing split cadence view](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671455940527)

The cadence diagram organizes processing into ownership and timing bands:

- Per-room scanning starts from onsite RoomPlan scans and manual edits.
- Per-room iOS processing parses JSON, builds canonical room geometry, generates the wall graph, classifies boundaries, assigns material intent, and creates stable IDs/provenance.
- Per-story provisional iOS processing handles same-story horizontal adjacency, envelope generation, B-factor defaults, vertical ceiling-to-wall logic, ceiling type, and diagnostic-only gap detection.
- Vertical story-to-story adjacency is marked as an open question.
- Building-complete final reprocess handles gap repair, knee walls, grade/plate splits, post-parse best fit, and segment azimuths. Owner is TBD and the work is deferred from the first port.
- Confirm/sync sends the confirmed model and material intent to the backend.
- Backend validates geometry/material refs, resolves material intent to durable home material IDs, persists the model, and runs online derivations.
- Backend-only derivations include measured/external areas, U-values, reports, BBR, and EPC.

Key constraints:

- Gap detection is diagnostic only and does not auto-mutate.
- Final reprocessing mutates the thermal envelope only, not raw scan source data.
- Online derivations are not shown in offline mode.
- The sync boundary prevents backend reprocessing from silently overwriting confirmed geometry.

## Processing Tables

### Server vs Device / Shared-Core Tradeoffs

Source: [Tradeoffs table](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764670546012744)

| Axis                           | If we keep processing on server, we lose...                                      | How much it matters | If we move canonical processing to device/shared core, we lose...                                                                                      |
| ------------------------------ | -------------------------------------------------------------------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Clear authority                | Client cannot fully guarantee onsite-confirmed areas are the final stored areas. | High                | Backend must accept or explicitly reject client-confirmed geometry; it cannot silently "fix" it.                                                       |
| Bug-fix velocity               | -                                                                                | High                | Canonical fixes need app/shared-core rollout. Mitigate by storing raw/canonical input and processor version, but no silent rewrite of confirmed scans. |
| Onsite responsiveness          | Full reprocessing requires network roundtrip.                                    | Medium              | -                                                                                                                                                      |
| Server outage resilience       | Confirmation/finalization blocked.                                               | Low-Medium          | Device can process and confirm offline; sync waits.                                                                                                    |
| Server compute cost            | Backend pays per scan.                                                           | Low                 | -                                                                                                                                                      |
| Battery and thermals           | -                                                                                | Medium              | Geometry processing consumes battery and may throttle long sessions.                                                                                   |
| App size                       | -                                                                                | Low-Medium          | Shared core plus cached catalog/material snapshots increases app size.                                                                                 |
| Algorithm iteration speed      | -                                                                                | High                | Dev loop becomes shared lib + bindings + iOS/device tests, not just Go tests.                                                                          |
| Catalog freshness              | Server always uses current catalog/material data.                                | Medium-High         | Device may process with stale catalog snapshot; backend must reject or require re-confirmation if versions conflict.                                   |
| Team skill commitment          | Go/backend ownership is enough.                                                  | Medium-High         | Permanent shared-native-core ownership needed.                                                                                                         |
| Engineering investment         | Existing Go path remains.                                                        | High                | Multi-quarter shared-core extraction/rewrite, parity tests, FFI, versioning, iOS integration, ops.                                                     |
| Server analytics / improvement | Centralized processing traces are easy.                                          | Medium              | Need client-uploaded diagnostics, hashes, processor version, and optional raw input.                                                                   |
| Subjective onsite correctness  | Final result may be reviewed after roundtrip/outside home.                       |                     | -                                                                                                                                                      |
| True offline confirmation      | Surveyor cannot fully confirm final geometry/areas in no-signal conditions.      | Market-dependent    | -                                                                                                                                                      |
| Privacy / data residency       | Raw scans may leave device.                                                      |                     | -                                                                                                                                                      |
| Capture in poor connectivity   | Nothing meaningful; RoomPlan capture already works offline.                      | Zero                | -                                                                                                                                                      |
| Single source of truth         | -                                                                                | High                | Lost only with duplicate implementations. Preserved with shared Rust/C++ core used by client and backend tooling.                                      |

### Processing Stage Ownership

Source: [Stage ownership table](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764670546012745)

| Stage                                                      | Why                                                                                        | Today                           | Final Owner                                                                              |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------------------ | ------------------------------- | ---------------------------------------------------------------------------------------- |
| Gap repair / knee walls / vertical ceilings / grade splits | Geometry-affecting and surveyor-visible.                                                   | Server                          | Shared geometry core                                                                     |
| DB persistence                                             | Durable source of truth.                                                                   | Server                          | Backend                                                                                  |
| Material selection per segment                             | This is an onsite surveyor decision.                                                       | Server receives upload metadata | Client authoritative confirmation                                                        |
| Adjacency detection                                        | Drives external area and thermal semantics.                                                | Server                          | Shared geometry core                                                                     |
| Apple RoomPlan normalization                               | Geometry-affecting and must be deterministic.                                              | Server                          | Shared geometry core or strict canonical client format                                   |
| SpawnSystemTasks                                           | Server orchestration after confirmed model.                                                | Server                          | Backend, except pure geometry tasks move to shared core                                  |
| Measured areas                                             | If surveyor signs off on it, client/shared core owns it.                                   | Server/read path                | Shared geometry core if confirmed onsite; otherwise backend derived from stored geometry |
| Material/default validation                                | Backend checks IDs, hashes, catalog version, country rules.                                | Server                          | Backend                                                                                  |
| Material thickness used by geometry/projection             | Areas depend on it, so it must be part of the confirmed package.                           | Server material map             | Client/shared-core input, with material snapshot/version                                 |
| External area projection                                   | Must match what surveyor confirms onsite.                                                  | Server/read path                | Shared geometry core                                                                     |
| BBR/EPC/external lookups                                   | Networked enrichment, not needed for offline confirmation.                                 | Server                          | Backend async                                                                            |
| Boundary classification                                    | Determines external/internal surfaces and areas.                                           | Server                          | Shared geometry core                                                                     |
| Snapshot creation                                          | Storage/version authority.                                                                 | Server                          | Backend                                                                                  |
| GCS download + ZIP extraction                              | Keep raw-input path for audit/reprocess, but not required for normal offline confirmation. | Server                          | Backend optional/debug path                                                              |
| FR custom material drafting                                | Client can define it; backend owns durable material identity.                              | Server                          | Client draft, backend persists canonical record                                          |
| U-value calculation                                        | Preview is useful; reports need backend authority.                                         | Server                          | Client preview, backend re-derives                                                       |
| Heat loss / EPC / report formulas                          | Regulatory/report output should stay server-side.                                          | Server                          | Backend                                                                                  |
| Material catalog cache                                     | Device needs offline processing, backend protects consistency.                             | Server                          | Client cache for processing; backend validates version/hash                              |
| ZIP/JSON parsing                                           | Client already has source data; backend parser helps debug and migrate.                    | Server                          | Client primary, backend optional                                                         |
| BestFitRooms / cleaning / wall graph                       | Sensitive geometry; one implementation.                                                    | Server                          | Shared geometry core                                                                     |

### Progressive Processing Feasibility

Source: [Progressive processing table](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764670844613845)

| Step                                               | Can Run After Each Room Merge? | Can Be Final?                          | Notes                                                                            |
| -------------------------------------------------- | ------------------------------ | -------------------------------------- | -------------------------------------------------------------------------------- |
| Parse RoomPlan room surfaces                       | Yes                            | Yes                                    | Purely local to scanned room.                                                    |
| External area projection                           | Maybe                          | No                                     | Depends on final envelope and material thickness.                                |
| Room segment cleaning                              | Yes                            | Yes-ish                                | Removes room-internal scan artifacts.                                            |
| Build walls/floors/ceilings/windows/doors/openings | Yes                            | Yes                                    | This is the client-owned raw geometry.                                           |
| Coordinate normalization into shared story frame   | Yes                            | Mostly                                 | Final only if the story/global frame is stable.                                  |
| Room-local best fit                                | Yes                            | Yes-ish                                | Wall/floor snapping inside the room is safe.                                     |
| Knee walls / grade splits                          | Maybe preview                  | Usually no                             | Needs story/roof/grade/material context.                                         |
| Vertical ceiling detection                         | Yes                            | Yes-ish                                | Room-local geometry classification.                                              |
| Default adjacency values                           | Yes                            | No, provisional                        | Avoid locking air/ground/heated too early. Use unknown/provisional.              |
| Gap detection                                      | Diagnostic only                | No                                     | Missing unscanned rooms can look like gaps.                                      |
| Room boundary classification                       | Yes                            | Provisional but useful                 | It says "room perimeter", not "external envelope".                               |
| Envelope classification                            | Yes                            | No, provisional                        | Depends on adjacency + room/story context.                                       |
| Gap repair                                         | No                             | Only after story completion            | This mutates geometry; too risky progressively.                                  |
| Measured areas                                     | Maybe                          | No, unless user confirms current scope | Useful preview, but final after story/building completion.                       |
| Final report/thermal derivations                   | No                             | Backend/server final                   | Not needed for progressive scan loop.                                            |
| Same-story horizontal adjacency                    | Yes                            | No, provisional                        | Recompute after each new room. Missing rooms create false "outside" assumptions. |
| Vertical adjacency between stories                 | No                             | After story/building completion        | Needs story stack.                                                               |
| Room wall graph                                    | Yes                            | Yes                                    | Local graph of this room's wall segments.                                        |
| Room gross area/dimensions                         | Yes                            | Yes-ish                                | Can change if merge/frame changes, but basically local.                          |

## Validation Questions

Source: [What to validate?](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671044504312)

The frame lists five validation hypotheses or questions:

- Room correction UX after scanning, including AR white lines for review.
- Whether external projection editing should be possible and how to solve thermal envelope holes.
- Whether better internal scan accuracy materially reduces thermal envelope editing needs.
- How to render the thermal envelope differently enough that users understand it as a separate model.
- Whether LTL should be calculated from the thermal envelope.

These appear to be validation topics rather than settled decisions.

## Engineering Areas

Source: [Eng areas](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671309778609)

The frame groups engineering investigation areas:

- Analyze post-processing pipeline stages and propose what can move.
- Draw progressive scanning and processing flows.
- Separate thermal envelope model architecture from EPC/report consumption.
- Decide model directionality and required accuracy.
- Resolve coordinate-space choices, especially world-coordinate versus transform-based systems.
- Explore AR editing capabilities for RbR models.
- Investigate a Mini-RoomRbR model concept.
- Verify ceiling projection feasibility.
- Keep projection algorithms pure and easy to iterate.
- Analyze historical scan data to validate thermal envelope assumptions.

## Loose Thoughts

Source: [Loose thoughts](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764670470759438)

The frame contains brainstorming notes across architecture, data modeling, UX, processing, and proof-of-concepts.

Themes:

- Reuse or test 3D rendering components and measure RAM/performance.
- Decide which processing belongs on device, in an API, or in shared core.
- Handle coordinate system differences between iOS RoomPlan and RbR models.
- Separate transform-based models from global-coordinate models.
- Keep mobile edits simple and room-level.
- Reserve story-level or cross-story 3D edits for bigger screens and rare edge cases.
- Consider whether story-level material defaults are blockers or nice-to-have.
- Support preliminary report views and large-screen report/EPC review.
- Investigate RoomPlan apps and correct inner-room scanning.
- Build POCs for room-level edits, post-processing, and performance.

## Source Index

- [Board overview](https://miro.com/app/board/uXjVHaYzk00=/)
- [Constraints, assumptions, choices](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671505428903)
- [Definition of correctness](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671505428904)
- [Terms](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671505428905)
- [Todays workflow](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671505622136)
- [Workflow without separate office](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671505622222)
- [How do we remove "3D editing" as a step?](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671505622273)
- [Digital twin onsite](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764670464744645)
- [Home State Data Flow - Target Model](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671457766273)
- [Home state layers frame](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671463337945)
- [RbR client processing split](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671461839960)
- [RbR processing split cadence view](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671455940527)
- [Tradeoffs table](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764670546012744)
- [Stage ownership table](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764670546012745)
- [Progressive processing table](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764670844613845)
- [What to validate?](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671044504312)
- [Eng areas](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764671309778609)
- [Loose thoughts](https://miro.com/app/board/uXjVHaYzk00=/?moveToWidget=3458764670470759438)
