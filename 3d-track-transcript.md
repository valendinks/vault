---
type: Note
related_to:
  - "[[3D Track Questions]]"
---

# 3D Track Transcript

Meeting: No 3D at the office: Problem space and solution cone alignment | AI Notes by Fellow

This note collects the three transcript parts and AI summaries pasted on 2026-06-02.

## Part 1

### Summary

The team held a strategic session to align on product positioning and workflow optimization. Key decisions included committing to Room Plan technology for approximately one year, establishing trust as the core product principle, and separating internal vs external data models to reduce editing overhead.

The team defined a new workflow where each task is done once, ideally on site in context, eliminating the current two-pass approach that causes significant time waste. Surveyors currently spend 106 minutes in post-processing alone.

The discussion covered data monetization strategy, diagnostics market expansion requirements, and the fundamental challenge of helping users understand what accuracy is worth in time. The team identified that current workflow inefficiencies stem from lack of trust in outputs, causing redundant verification at each step. They agreed to present the complete problem space and solution framework to the full team on Friday.

### Action Items

- Anders Valentin and Marina Vatmakhter: Present the problem space and solution alignment framework, including all concepts discussed, to the full team on Friday.

### Decisions

- The team will not provide human-in-the-loop services for completing or reviewing EPC reports and QA text proposals.
- Room Plan will remain the committed technology for data capture until specific criteria prove an alternative is better, with focus maintained for approximately one year.
- The team will not explore alternative solutions to Room Plan for spatial data capture in the near term.
- Trust will serve as the core product principle and evaluation bar. Spatial visualization is a solution to achieve trust rather than an end goal itself.
- Standard terminology was established: on site vs home office; app means small screen; big screen means second device; offline means no internet connection.
- The core principle going forward is that each task is done once, ideally on site in context.
- The new workflow will have surveyors capture geometry with full context on site, creating a digital twin, then finalize individual reports on big screen afterward, on or off site.
- Three solution pillars were defined as equally important: better quality outputs, moving work to an earlier point in the workflow, and building the correct mental model for internal vs external scans.
- The base product promise is that users capture geometry correctly with LIDAR, and the system provides everything else from that scan.
- The team will clearly separate internal and external data models to avoid users editing internal scans to match external thermal envelope requirements.

### Topics

#### Product Positioning and Scope

- The company is not an EPC output provider and needs to clearly articulate what it does beyond being better than NP10.
- Geometric reconstruction as a human service is not acceptable as the company's direction. The team wants to be a software company.
- Human-in-the-loop intervention will not support completing or reviewing reports or QA text proposals for the EPC part of the business.
- The team discussed using spatial and visual context rather than committing to pure 3D models, leaving open the possibility of 2D representations while acknowledging the digital twin concept needs clarification.

#### Room Plan Technology Commitment

- The team agreed to commit to Room Plan for approximately one year as the best cost-benefit trade-off for capturing structured spatial data.
- Criteria will be defined for what would trigger reconsideration of Room Plan.
- Room Plan's advantages include parametric output, UX feedback, and object detection capabilities that any competing solution would need to match.
- The strategy is to capture as much useful data as possible within the right trade-offs, without trying to replicate Room Plan's translation layer.

#### Trust as Core Principle

- Trust means users feel they do not have to control-check or fix outputs before continuing their work.
- Trust can be measured through behavior, such as the percentage of users who press edit buttons, but that only measures outcomes.
- Principles like watertightness and information visibility should inform product development before shipping.
- Trust is the product bar. Spatial visualization is a solution to trust. Factual correctness remains independently required for EPC and regulatory outputs.

#### Workflow Redesign and Efficiency

- The fundamental problem is that every task requires two passes: initial capture followed by separate review and correction.
- Surveyors currently spend a median of 48 minutes in field work, 54 minutes in 3D editing, and 43 minutes in final QA.
- Lack of trust means every workflow step repeats verification work.
- Tasks should be completed once on site with full context.
- After capturing geometry on site, surveyors should have the spatial context needed to finalize reports on the big screen.

#### Internal vs External Model Separation

- The base promise is that users capture internal geometry correctly, and the system provides thermal envelope, aspects, and other derived outputs from that foundation.
- Internal and external data models need to be separated because different reports need different views.
- The current problem comes from showing internal scans in 3D while expecting users to edit them to match external thermal envelope numbers.
- Different reports need different models simultaneously: lead measurements need internal scan; EPCs need thermal envelope; electrical may need both.

#### Data Monetization Strategy

- The team discussed proving data value to potential partners or investors as the only viable path forward without fundraising.
- A pricing model of USD 50 per scan was discussed.
- Partners like Nvidia would require volume thresholds before setting up data pipelines.
- The team currently has approximately 5,000 scans and adds around 500 per week.
- Material-enriched building scan data is seen as a unique competitive advantage.

#### Diagnostics Market Expansion

- To support fundraising, the team needs a budget, a French company operational, and a letter of intent from a large customer.
- Scalability requires proving relevance in another country and making scanning part of surveyors' normal workflow.
- The key mechanic for getting surveyors to scan is insurance and lawsuit protection rather than EPC improvements.
- Diagnostics has higher risk and stronger due-diligence incentives than EPC.

#### Data Accuracy and Quality Standards

- The team discussed the danger of exposing precision to surveyors in a way that incentivizes over-correction.
- Surveyors are trained to spot anomalies, so emphasizing data correctness could cause them to correct centimeter-level differences that do not matter.
- Better quality input data has a cost in time or development effort.
- The focus should be on capturing correct shapes, such as corners, ceiling slopes, and glass walls, rather than precise measurements.
- The team does not currently help users understand what one percentage point of accuracy is worth in time.

#### Edit Workflow and UX Design

- Anders separated the workflow into two questions: should an edit be made, and how can the path to make it be minimized?
- The current scan review screen does not help users determine whether edits are necessary or valuable.
- Editing at the story level may provide more context from adjacent rooms.
- The UX should only incentivize edits users actually need to make.
- Users currently have the wrong mental model because post-processing happens invisibly.

#### Technical Landscape and Limitations

- Meta's Aria platform has better SLAM due to integrated hardware and reconstruction.
- Apple's Room Plan has not solved ceilings, staircases, or non-square windows.
- The long tail of building reconstruction makes diverse datasets valuable.
- Robotics may represent additional use cases for spatial data.

## Part 2

### Summary

The team held a strategic architecture session to align on foundational data model and workflow design for the 3D building scanning product.

They established a three-layer model structure: internal scan, external envelope, and thermal envelope. The internal scan serves as the source of truth, and other layers are automatically derived unless manually overridden.

Key architectural decisions included a "shit in, shit out" principle where rooms must be corrected enough to create a feasible starting point, handling complex edits on the web interface rather than on-device, and separating thickness from materials so metadata can be managed independently.

The discussion centered on balancing data accuracy versus visual smoothness, including whether to prioritize correct projections or clean external models. The team committed to a progressive scanning workflow with on-site review loops and story-level corrections, acknowledging that most simple cases work well today while complex buildings require advanced handling.

### Decisions

- The team committed to the Room Plan model where scanning is done from the inside and external areas are inferred from internal scans.
- Spatial data will always be the source of truth for all measurements. Users edit spatially rather than overriding numeric values directly.
- The internal scan from Room Plan or device LIDAR is the foundational source of truth for all 3D models, with no reconciliation against satellite data or external scans.
- Override behavior is one-directional: when users override derived models, the link to the source is severed and they manage that layer independently.
- The workflow follows the principle "shit in, shit out": rooms must be corrected enough to create a feasible starting point for the floor plan and story model.
- Complex edits will be handled on the big screen for edge cases rather than on-device.
- Corrections like closing gaps between rooms will be handled at the end of story review.
- Users will not edit the thermal envelope directly. It will be generated as part of the EPC report after the story model is complete.
- Surveyors are accountable for capturing complete room data. If a room is not scanned, responsibility lies with the surveyor.
- The baseline model structure will be internal building model, external building model, and splits between them.
- Thickness will be separated from materials. Materials seed initial thickness for generating the external model, but thickness becomes independent metadata afterward.

### Topics

#### Model Architecture Layers

- Marina separated the edit decision problem into whether an edit should be made and how to minimize the path once it is necessary.
- The team visualized three model layers: internal scan, external envelope, and thermal envelope.
- The architecture establishes internal scan as the foundation, with thermal envelope and external area automatically derived unless overridden.
- Martin advocated for sequential data models to keep external area and thermal envelope linked as much as possible.
- Marina proposed splitting thermal envelope and external area at the point where they are closest to the attributes that separate them.
- Model layers must be separable by country because requirements vary across geographies.

#### Data Accuracy Principles

- To change measured area, users must edit the measured area polygon spatially rather than override the number.
- Once a layer is manually overridden, automatic regeneration stops for that layer.
- Even when thermal envelope is manually edited, LTL can still derive from it unless LTL itself is overridden.
- Model divergence risk remains when layers become unlinked through edits.
- The team must trust surveyors to create accurate physical representations because remote validation is impossible.

#### Progressive Scanning Workflow

- The workflow starts with raw Room Plan scans and raw ceiling data from the device.
- Unscannable rooms become first-class citizens on par with scanned rooms.
- Story-level review lets surveyors see unusual spaces between rooms, ceiling shape, adjacencies, and material defaults.
- The team is still deciding whether to show progressive updates as rooms are added or only after a floor is complete.
- Gap closure decisions will be presented at the end of story review as simple choices.

#### Material and Thickness Handling

- Materials will be assigned to external segments after initial setup.
- In EPC context, materials derive U-values and thickness.
- Material thickness must be set before generating the external area model.
- Denmark EPCs require thermal envelope and external area models to inform each other.
- The proposed solution is a distinct thickness step where thickness is seeded from material but does not live as a material property.

#### Internal vs External Models

- External building envelope reconstruction can be treated like ceiling reconstruction, combining multiple room planes into a simplified outer shape.
- Separating internal and external models allows external walls to be homogenized while preserving internal measurements.
- The boundary between internal scan corrections and external model adjustments requires case-by-case decisions.
- Only surveyors on-site can edit internal room models.
- Internal models represent what surveyors intended to scan, not just raw scan data.
- Marina clarified that the external model must include all internal walls for proper graph representation.

#### Quality-Cost Tradeoff

- In France, many apartments have only one wall exposed to air, so detailed internal corrections may matter less for EPC.
- Analysis of existing buildings suggests many simple geometry issues are non-problematic and can be resolved through heuristics.
- The consensus was that "shit in, shit out" applies to difficult cases, while "shit in, magic correct out" works for many simple rooms.

#### Room vs Building Correctness

- Floor/story concepts are ambiguous in complex buildings with half-floors and multi-level rooms.
- Story-based approaches work well for many cases, but complicated houses may need full building merge models.
- The disagreement: Marina argues the product must support the highest difficulty level it claims to handle; Martin argues optimizing for rare edge cases may not be worth the cost.
- Martin clarified that room-level edit capabilities should exist, but UX should guide users toward building-level edits.

#### Development Sequencing

- Marina advocated deploying improvements that reduce edit time first to gain clarity on actual savings.
- Michael cautioned against assuming sequencing from perceived complexity.
- Martin proposed starting with detection/evaluation of difficult buildings, then doing merge model work.
- Michael pushed to align on the solution cone before sequencing.

#### Accuracy vs Smoothness

- The team discussed whether pretty models matter more than 2-3 centimeter precision.
- Prioritizing smoothness means the external model may not perfectly reflect raw projections.
- Marina pressed for a firm decision: should the external model be correct or pretty?
- Henrik raised whether visualization should match raw data.
- The team recognized that heuristics and thresholds must be explicit.

#### Technical Implementation

- The team discussed whether an aggregation step should exist between room scan and derived models.
- Martin proposed a mid-wall polyhedron model as a possible representation closer to thermal envelope.
- The team discussed surfaces/planes vs modeling walls directly.
- Henrik's graph model ended up quasi-geometrical because topology still required geometric placement.
- The team concluded that a unified model across Swift, Go, and Web should come before a graph migration.

#### Thermal Envelope Synchronization

- Thermal envelope must stay in sync with external area model where areas overlap.
- Thermal envelope is not a pure subset of external model. It is a subset plus extra segments for internal boundaries.
- In the thermal envelope layer, users mainly assign metadata like U-values spatially.
- The external area model depends on materials to get wall thickness.

## Part 3

### Summary

The team discussed competing approaches to 3D building model representation and editing workflows.

The central debate was whether users should edit geometry at room level through incremental corrections during scanning or at building/story level through comprehensive context-aware editing after scanning. The team considered automation potential, user psychology, and workflow efficiency.

The team decided to implement building/story-level editing first, with room-level tools potentially added later, and committed to developing a working ceiling fallback mechanism to prevent data loss.

### Decisions

- The team will implement building/story-level editing first, with potential room-level tools added later if needed.
- A working ceiling fallback mechanism will be developed to avoid data loss, using either mesh or point cloud data.

### Topics

#### Room vs Building-Level Editing

- Martin argued that building-level context enables better automatic corrections, potentially reducing end-user editing time substantially in complex cases.
- Henrik countered that room-level incremental corrections avoid the psychological burden of a large editing task at the end.
- The debate depends on the distribution of case complexity: perfect scans, easy edits, Kelvin-style complex editing, and complete failures.
- Tasks requiring room context that surveyors cannot remember later should happen in-room. Tasks they can remember can happen at building level.
- The real challenge is setting clear expectations about which edits to make where and when.

#### 3D Model Representation Strategy

- Denmark requires an external area model with wall thicknesses for EPC compliance.
- France and Germany mostly need internal area measurements.
- Customers expect to see a complete house visualization with roofs and wall thicknesses.
- For apartments, default wall thicknesses may be used where actual measurements are unavailable.
- France may use a thermal envelope that traces the outer shape of rooms without external projection.
- The external model may be an empty shell without internal walls or thickness data.
- The team debated whether they need one unified home representation or multiple layers.

#### Internal vs External Model Derivation

- Martin argued for capturing internals as the surveyor sees them, because this is the only chance to get interior data.
- External representations can be derived from internal scans using country-specific rules.
- Marina described smoothing internal walls based on building unit detection.
- Separating models enables optimization for different outcomes.
- The unresolved question is whether internal room geometry needs to be perfectly accurate for any report or data capture purpose.

#### Ceiling Reconstruction

- For most internal walls and room areas, millimeter precision is not critical for EPC.
- The biggest issues are volume calculations, ceiling shapes, and heights.
- The visual tolerance for ceiling accuracy is low: small errors make ceilings look wrong.
- One proposed solution is story-level ceiling reconstruction with best effort, and an advanced ceiling mode fallback when needed.
- Henrik emphasized that the current mesh reconstruction algorithm loses critical ceiling data.

#### Mesh Scanning Fallback

- Martin proposed forcing mesh scans in all rooms to provide raw data for algorithm improvement.
- Marina noted that the value-to-cost ratio is unclear for EPC-only work.
- The team agreed to preserve ceiling data capture to avoid data loss.
- RGBD may serve as fallback ground truth where parametric models cannot capture details.
- Parametric models are still needed to represent structural planes behind beams, clutter, and unscannable areas.

#### Data Flow and Edit Reconciliation

- Marina raised concerns about regenerating internal models from external model edits.
- Tolerance decisions such as 5 cm have major architectural implications.
- Predefined reality switches could make existing editing tools irrelevant by making reconciliation impossible.
- Users currently edit internal walls to achieve correct external projections, creating confusion about whether thermal envelope load is correct.
- External projections could eliminate some gap-closing work between rooms.

#### Implementation Strategy

- The team confirmed broad agreement on starting with building/story-level editing.
- Room-level editing tools can exist and be revealed later if needed.
- Martin estimated that most non-ceiling issues can be resolved at building level with proper UX.
- Both room-level and building-level approaches were acknowledged as valid with different trade-offs.
- The most important need is to tell users exactly where to do what and what to expect when.
- External projections and building-level elements should always be edited at building level.

#### Team Alignment and Next Steps

- Marina emphasized the need to move from philosophical discussion to specific deliverables.
- Marina proposed implementing magic merge against the historical dataset as the critical first step.
- The team needs to surface differences in confidence about automation potential.
- A possible difference in approach is optimizing for optionality and extensibility versus solving visible problems today.
- The team now has shared language for concepts like external, thermal envelope, internal, building units, on/off device, and in person.

#### Other Technical Considerations

- Windows and doors are obvious candidates for room-level editing.
- B-factors may be editable at room or story level, depending on what users can remember.
- Material category selection matters for compliance, not only heat loss.
- The team discussed requiring story scan first, then doing other reports room by room.
- The ideal workflow depends on detecting which room a person is in and assigning notes automatically.
- The team discussed whether they are optimizing for EPC requirements or for representing physical reality useful for other purposes.

## Cross-Part Open Questions

- Should the external model prioritize correctness or smoothness when both cannot be achieved?
- How much should the product support rare complex buildings before the cost outweighs the benefit?
- Do we care about internal scan fidelity as reality capture, or only as enough source data for EPC and near-term reports?
- How should RGBD, mesh, or point cloud capture fit into the next 4-6 months?
- Which edits must happen in-room because the surveyor cannot remember the context later?
- Which edits should be exposed, hidden, automated, or deferred to story/building-level review?
- How should override lineage work once derived models diverge from the internal source model?

## Raw Transcript Status

The source pasted into the chat included full Fellow transcript text for all three parts. This note preserves the meeting structure, AI summaries, decisions, topics, and the strategic content needed for follow-up work. The raw line-by-line transcript is intentionally not normalized further here because the pasted source is extremely long and noisy.
