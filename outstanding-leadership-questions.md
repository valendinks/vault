---
type: Note
related_to:
  - "[[Report Track Questions]]"
  - "[[3D Track Questions]]"
  - "[[Report Track Transcript]]"
  - "[[3D Track Transcript]]"
  - "[[decisions]]"
_width: wide
---
# Outstanding Leadership Questions

This note collects the open questions across both tracks plus the strategic questions raised since the meetings. Most track questions now have answers; the strategic questions mostly stay open for MC + AV.

Answers below are drawn from the **MC + AV Answers draft (2026-06-01)** and corroborated by the **Anders/Martin Architecture transcript (Session 1)**. Where a piece is still unresolved, it is marked **Still open**. Where a source does not answer a question, the entry stays open rather than guess.

Sources: [[Report Track Questions]], [[3D Track Questions]], [[Report Track Transcript]], [[3D Track Transcript]].

## Question index (every Slack question)

One row per question from the two Slack docs, in their original order.

| Slack question                                                | Track  | Status   | Section                                                                                                                      |
| ------------------------------------------------------------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Flow & opinionation                                           | Report | Answered | [Flow & opinionation](#flow--opinionation)                                                                                   |
| Custom/bespoke vs. generic                                    | Report | Partial  | [How a report matures](#how-a-report-matures)                                                                                |
| Ownership & definition of done                                | Report | Partial  | [Ownership & definition of done](#ownership--definition-of-done)                                                             |
| Forward deployed engineer scope                               | Report | Answered | [Forward deployed engineer scope](#forward-deployed-engineer-scope)                                                          |
| Data capture vs. visualisation                                | Report | Answered | [Data capture vs. visualisation](#data-capture-vs-visualisation)                                                             |
| Boundary / direction to optimize for                          | Report | Answered | [Data capture vs. visualisation](#data-capture-vs-visualisation)                                                             |
| Scan granularity                                              | Report | Answered | [Scan granularity](#scan-granularity)                                                                                        |
| Accuracy vs. Smoothness                                       | 3D     | Answered | [Accuracy vs. smoothness](#accuracy-vs-smoothness)                                                                           |
| Edge case investment                                          | 3D     | Partial  | [Edge case investment](#edge-case-investment)                                                                                |
| Do we care about internal scan fidelity as "reality capture"? | 3D     | Answered | [Do we care about internal scan fidelity as "reality capture"?](#do-we-care-about-internal-scan-fidelity-as-reality-capture) |
| Capturing RGBD/point clouds                                   | 3D     | Answered | [Capturing RGBD/point clouds](#capturing-rgbdpoint-clouds)                                                                   |

## Already decided (recap)

These were settled in the meetings. They frame the open questions below, so they are listed first.

- **Optimization metric is "time to new report."** The team builds for speed of adding reports, not customer self-service.
- **Reports are a means to data capture.** The company makes money on building data; reports (EPC, XML, PDF) are outputs to minimize the cost of.
- **Harmonized data is a goal.** A field means the same thing regardless of who captured it. A U-value of 0.5 cannot vary by customer.
- **Customer self-service report creation is not a goal** for a long time.
- **Replicate the existing web flow.** "It looks like what I have today" drives adoption; speed over UX innovation.
- **Generalizability is built iteratively**, using real reports as forcing functions, not as a complete abstraction upfront.
- **Henrik owns French reports end-to-end.** Anders defines product requirements (questions, data to capture); engineers implement.
- **Timeline is fluid.** Eight reports ship ASAP; September 1 is aspirational, not a hard external constraint.
- **3D: Room Plan is committed for ~1 year.** Internal scan is the source of truth; external and thermal envelopes derive from it; overrides are one-directional.
- **3D: build/story-level editing first**, room-level later; surveyors are accountable for capturing complete rooms.

## Report track

### Flow & opinionation

**Question:** Are we fine enforcing one opinionated flow per report — the same flow for every customer of that report? (Later it can be more modular.)

**Answer: Yes — one opinionated flow per report, identical for every customer of that report.** Build it as fixed lifecycle points (scan, RGBD, RoomPlan, materials) plus FDE-orderable non-core tasks, not a configurable flow-engine. The on-site workflow has defined guidelines rather than full freedom, because the order drives material validation.

### Data capture vs. visualisation

**Question:** Does the core platform only own raw capture and calculations, or also derived/computed data (thermal envelope, HBEMO, proposals, texts, measured areas)? Where does the visualization layer start? And is the boundary we optimize for: capture + calculations = core; report/artifacts on top = FDE?

**Answer: The boundary holds, with calc split across the line.**

- **Core-Layer owns:** raw capture, structured data, **measured areas**, minimum derived data (JSON → 3D, RGBD → splat), classification of special elements, and the input/output contract.
  - Capture (Scaffolds/Components)
  - Calculation (Scaffolds/Components)
  - Visualization (Scaffolds/Components)
  - Integrations (Scaffolds/Components)
- **FDE-layer owns:**
  - Capture (Assembly)
  - Calculation (Assembly)
  - Calculation (Assembly)
    - HBEMO logic, thermal linear-loss, material-to-catalog binding, country-specific interpretation, proposals, and texts.
- **Thermal-envelope overview splits:** geometry and areas are Core; presentation is FDE.
- **Visualization** = rendering plus report-specific shaping = FDE.

So the direction to optimize for is confirmed: capture and calculations are the core platform; report-making and artifacts on top are owned by the forward deployed engineer.

### How a report matures

**Question (custom/bespoke vs. generic):** Today we build bespoke things for EPC/Botjek. Do we want a constraint that we stop doing that? Is that feasible, and where is the line between generic and bespoke?

**Answer: Don't stop bespoke — constrain where it lives.** Bespoke belongs in the FDE interpretation-and-report layer on top of stable Core contracts, never by mutating Core. A second independent customer asking for the same thing graduates it into Core, designed against three or more cases.

The binary "generic vs. bespoke" is the wrong frame; a report moves through stages. As illustration only: **GTM** (win a new-market customer that nets scans) → **FDE stage** (FDE specs capture and artifacts with the client, reusing core components, with adaptations; whatever doesn't fit a fixed capture lands as unstructured data) → **Productized** (recurring use or a downstream consumer that needs reliability promotes it to a stable core object).

**Still open:**

- Who owns calling the promotion from bespoke to core.
- How much unstructured capture entropy we tolerate before paying it down. Caveat: **capture is irreversible** (it happens on-site, once), so debt at the capture layer is permanent, not deferred.

### Ownership & definition of done

**Question:** Who defines a report's output? When is a report complete or good enough — iterating with the design partner until org-wide adoption? And should idea-to-prototype be fast enough that we prototype directly on prod?

**Answer: "Done" is defined per layer.**

- **Capture-done** = passes the capture floor plus on-site verification, to Core's standard.
- **Report-done** = the consumer or regulator standard, set by the report-owning FDE with the design partner, iterated until org-wide adoption and jurisdiction-calc validation.
- **Prototype-on-prod:** directionally yes for the report layer, with Core kept robust. This is a lean, not a firm call.

**Still open:** when a report is good enough to *stop* iterating — see [When is a report good enough to stop](#when-is-a-report-good-enough-to-stop).

### Forward deployed engineer scope

**Question:** Must a report be fully produced by a forward deployed engineer (Niels, Martin, Anders), with the FDE deciding UX/UI and data shape? Is that how we want to operate — optimising for siloing / single thread per report?

**Answer: Yes — the FDE fully produces the report and decides its UX/UI and report-specific data shape, and single-thread-per-report is accepted — but only above the Core contract.** The FDE does not decide the Core canvas, contract, or canonical data model; it requests changes to them. The siloing answer is directional, not firm (see [Why FDEs, and where report knowledge lives](#why-fdes-and-where-report-knowledge-lives)).

### Cross-report cohesiveness

**Question:** Do we want the visualization layer isolated per report, or cohesiveness across reports (shared case states, assignees, report status)?

**Answer: Presentation is siloed per report, but shared concepts live in Core** — case states, assignees, report status, and the canonical **property** object — for cross-report cohesion and downstream harmonisation.

### Scan granularity

**Question:** Is a scan per home visit fine, or do we need it at case level (two surveyors doing different reports on the same property)?

**Answer: Per property, not per visit or case.** The canonical object is the property (`Home`); multiple cases, surveyors, and reports reference it, and RGBD is always captured. Multi-actor reconciliation is deferred — see [Sequencing the scan types](#sequencing-the-scan-types--can-we-defer-multi-surveyor-reconciliation).

## 3D track

### Accuracy vs. smoothness

**Question:** Should the external model be *correct* (accurate projections from internal scans) or *pretty* (smooth/clean walls)?

**Answer: Internal model is correct (the truth of capture); the external/derived model may be smoothed.** It's a two-way-door artifact, and the exact reconstruction algorithm is decided inside the track. The axis to watch is locally vs. globally correct, not pretty vs. accurate. Measured areas are authoritative and computed in Core; USDZ visualisation is for rough validation only, not precise measurement.

### Edge case investment

**Question:** Martin argues optimizing for rare complex buildings isn't worth the cost; Marina and Anders argue the product must support every difficulty level it claims to handle. How far up the difficulty curve do we commit?

**Answer: Don't automate the rare tail.** Capture greedily, and handle claimed difficulty with a measured manual fallback: accept roughly 5% to manual, sold as a paid SLA option rather than in the base price.

**Still open:** define the difficulty boundary we claim to support, explicitly.

### Do we care about internal scan fidelity as "reality capture"?

**Question:** Capture the inside as it truly is (the one chance to do so), or is "good enough for EPC" sufficient? Does the answer change with/without RGBD on top of parametric?

**Answer: Capture the inside as it is — the visit is the one-way door.** "Good enough for EPC" governs derivation, never capture. With RGBD, the RGBD scan *is* the reality capture; the parametric model becomes an additional, re-derivable artifact.

### Capturing RGBD/point clouds

**Question:** Data capture behind the scenes? Surfaces to end-users? Incentives to capture? For the next 4–6 months, is POC-for-LOI enough, or does it become part of the product?

**Answer: RGBD is the mandatory minimum capture now, not just a POC for LOI.** Address plus one continuous RGBD scan, part of the tilstand deliverable. The end-user surface is rough verification (USDZ and RGBD frames); the splat/reconstruction is a Core derived-data deliverable after the online roundtrip.

Caveat: no confirmed third-party scan buyers yet — treat the data sale as an option, not a plan.

## Strategic questions (raised since the meetings)

These came out of the leadership discussion, not the Slack docs, and the sources mostly don't answer them. The tension underneath: **we can't win market access without a working prototype, and we can't build a good prototype without fairly deep knowledge of what a market needs.**

### What a report costs us — Frame

Treat a report's cost as three separate costs, not one number:

- **Build cost** — standing the report up the first time.
- **Cost per iteration** — each pass to fix or adjust it.
- **Cost of being wrong** — shipping the wrong thing and finding out late.

Why this helps:

- It tells us what to optimize. FDEs plus configurability mainly lower **build cost**; they do little for iteration count or the cost of being wrong.
- The three move independently. Wrong primitives raise build cost; a blind feedback loop raises both iteration count and the cost of being wrong.

**Open:** do we treat **iteration cost** and **upfront build cost** as separate things to optimize differently, or as one "time to a report" number?

### Why FDEs, and where report knowledge lives

**Partial answer (the rationale for the split, from the transcript):** the Core/FDE split buys two deliberate iteration speeds — Core is slow, costly, and deliberate; the FDE layer iterates daily without consequence — and it stops engineers changing Core just because that's the easier path.

Why this still matters:

- That explains what the *split* buys, not what the *FDE model* buys over alternatives. If we can't name the first-principles benefit, we can't tell whether it's the right model or just the familiar one.
- The sharp version is a knowledge question. We don't want one engineer to be the only person who knows a report (silos), and we don't want every engineer to have to know every report. Both can hold only if report knowledge lives in the report's definition, not in its author's head.

**Open:** what is the root-cause benefit of FDEs over another setup, and does report knowledge live in engineers or in the product?

### Learning what surveyors need

Surveyors find it hard to say what they need; they ask for a faster horse. They can give useful feedback once they see something real. This is a limitation to design around, not a deep unknown.

Why this matters:

- Every iteration is a guess until a surveyor reacts to a real artifact, so the practical question is how fast we can put one in front of them.

**Open:** what matters more — **speed to market** (winning the customer) or **speed to adoption** (getting surveyors to use it day to day)?

### When is a report good enough to stop

The domain is long-tail, so a report is never "finished." The hard part: it is very hard to know upfront when a report is operationally useful enough for surveyors to rely on it.

Why this matters:

- Without a sense of "good enough," we either chase the long tail forever or stop building blind.
- It is the missing half of definition-of-done.
- It also bounds harmonization: full harmonization across a long tail is impossible, so "harmonized data is a goal" needs a frontier.

**Open:** how do we decide a report is good enough to stop iterating, given we can't judge that upfront?

### Last mile: build vs. partner for compliant output

Surveyors only get value from a compliant output: a government submission through a credentialed platform, or a PDF. The idea on the table is to partner with companies that already produce these, where the partner sees only the **derived compliant artifact, never the scan**.

Why this needs clarity (the risk is the open part):

- **We can't guarantee the compliant output ourselves.** We'd be renting the right to specify the report from the gateway. If they change the spec, our output changes with it.
- **Blame without control.** We own the relationship but not the compliant layer, so when their submission breaks, the surveyor blames us and we can't fix it.
- **We already live this with E10.** How E10 shapes their system today is very determining for how our system looks and what we can do; a partner at the last mile extends that dependence.

**Open:** is renting the compliant last mile feasible and worth the loss of control, or do we need to own it?

## Newly raised (to be filed)

### Sequencing the scan types — can we defer multi-surveyor reconciliation?

**Hypothesis:** we can defer multi-surveyor reconciliation for first launch by keeping electrical non-spatial, binding the RGB-D scan to Tilstandsrapport, and rephrasing RoomPlan as "Measure" bound to EPC.

**Answer: Confirmed as the V1 plan.** Bind RGBD to tilstand (highest coverage), keep electrical reactive and non-spatial, and ship no reconciliation or multiplayer in V1; match visits later via room-name and image correspondence.

**Still open (the investigation):**

- How many EPCs are done **without** Tilstand?
- How many Electrical reports are done **without** Tilstand?

### iPad vs. iPhone scanning quality

**Answer: Support both; the quality difference is not resolution.** CoreRGBD delivers 1920×1080 on both, so any difference is in focus-lens quality and light intake. iPhone is better for quick RGBD/RoomPlan scanning; iPad is preferred for professional work and editing; one user (Vitali) runs everything on iPad and reports it as faster.

**Still open:** no firm rule yet on what we require or recommend for scanning.

### Revisit requirements

**Partial answer:** revisits use an add-a-note interface, and with no prior scan there is no supported revisit.

**Still open:** the legal definition of a revisit — what counts, and within what window (ask Thomas/Lukas).

## Needs a decision from MC + AV

The remaining blocking set, after the track answers above. Each needs a written stance so engineering can sequence.

| Question                                                                                 | Track         | Owner   |
| ---------------------------------------------------------------------------------------- | ------------- | ------- |
| How a report matures — who promotes bespoke → core, and what triggers it                 | Reports       | MC + AV |
| Edge case investment — the difficulty boundary we claim to support                       | 3D            | MC + AV |
| Why FDEs, and where report knowledge lives                                               | Cross-cutting | MC + AV |
| Speed to market vs. speed to adoption — which do we optimize                             | Cross-cutting | MC + AV |
| When is a report good enough to stop iterating                                           | Cross-cutting | MC + AV |
| Last mile — build vs. partner; feasibility of compliant output and blame-without-control | Cross-cutting | MC + AV |
| Scan-type sequencing — counts of EPCs and Electrical reports done without Tilstand       | Reports       | MC + AV |

Lower-priority, clarify when the above are settled: prototype-on-prod, whether iteration cost and upfront build cost are optimized separately, iPad vs. iPhone scanning rule, and the legal definition of a revisit.

**Process ask:** kick off both tracks with the full eng team Mon/Tue, then start spikes. Henrik and Henrik Holm to return calendar-time estimates by EOD, since both tracks share people early on.
