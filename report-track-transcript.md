---
type: Note
---

# Report Track Transcript

Meeting: Report Track: Business level alignment | AI Notes by Fellow

## Summary

The team held a strategic alignment meeting to define the architecture and approach for their reports platform, which needs to deliver eight new reports across Denmark and France markets. The core tension centered on whether to build a highly flexible, modular system that allows non-engineers to configure reports versus delivering specific reports quickly through more opinionated implementations. Key technical debates included the separation of data capture, calculation, and visualization layers, the balance between standardized core data models and customer-specific customization, and whether to replicate existing workflows or innovate on UX.

The team aligned on two critical business goals: minimizing time-to-new-report as the primary optimization metric and supporting many different data capture methods across markets as their growth wedge. They decided that external timeline commitments are fluid rather than hard constraints, allowing flexible sequencing of development work. Henrik took ownership of all French reports end-to-end, and the team agreed to build generalizability iteratively using actual reports as forcing functions rather than building complete abstractions upfront. The discussion revealed ongoing tension between commercial urgency (needing reports to unblock sales) and engineering desire for proper platform architecture.

## Action items

- Henrik Holm: Take ownership of all French reports, including defining data requirements and building report outputs using provided API templates 2:33:46

## Decisions

- The team aligned on the business goal of being able to very quickly capture new reports, treating 'time to new report' as the key optimization metric. 1:54:47
- The platform will support many different ways to capture data for different purposes (rental, Norway, West Indies, etc.) as the wedge to achieve growth goals. 1:51:21
- The team will replicate the existing whose web flow rather than adding new value or reinventing the UX, prioritizing speed to market over innovation. 2:10:08
- Eight reports need to be built as soon as possible on the new platform, as they are blocking commercial progress, with timeline flexibility prioritized over the September 1st target date. 2:38:12
- External timeline commitments for DK electrical and conditioning reports are fluid rather than hard constraints, allowing for flexible sequencing of development work. 2:50:15

## Topics

### Report architecture philosophy

- The goal is to achieve business-product level alignment on the reports track so engineering can proceed without needing to think through the strategic questions. Some open questions remain but will be made explicit. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=125">02:05</a>
- Three report buckets were proposed: regulated inspection-driven (electricity/partition reporting, not related to property itself), regulated computation-driven (EPCs requiring full home state and spatial awareness), and custom bespoke reports (fully customizable for individual customers). <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=241">04:01</a>
- The key distinction is that regulated reports require compliance with specific regulatory output, while custom reports don't require forcing any system constraints. Custom reports could allow customers to build data models themselves without plans app taking responsibility for outcomes. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=310">05:10</a>
- The boundary between regulated and custom reports is debated - compliance alone may not be the right divider since the team already goes beyond compliance. The real question is who should know the domain context and whether all engineers, one engineer, or anyone in the company needs that knowledge. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=545">09:05</a>
- The team reviewed the core problem statement that adding or tweaking reports should primarily be a content project rather than an engineering effort. Henrik explained the technical distinction between report types A and B based on spatial domain requirements. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=1864">31:04</a>
- Report type distinctions exist for engineering reasons: regulated reports (type A) can be bundled with clear boundaries per report, while spatial-dependent reports (type B) require full geometry calculations that only affect certain reports. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=1969">32:49</a>
- Henrik clarified that reports of type A don't mutate the spatial model while reports of type B do, which fundamentally impacts UX flow control and architecture design. This distinction determines when and how data is collected in the app. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=3461">57:41</a>
- Henrik described the ideal platform concept: flexible data capture paired with flexible visualization, where both components must have matching flexibility for true modularity. The challenge is balancing opinionated capture with unopinionated visualization. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=5409">1:30:09</a>
- The team discussed component-based architecture similar to Zapier, where generic off-the-shelf components (questions at building, room, element, annotation levels) could be reused across different report types like EPC. Henrik suggested EPCS components could be generic despite needing to be world-class. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=5523">1:32:03</a>
- Henrik noted they're already modeling data capture in a generic, configuration-driven format where atoms can be composed. The challenge is making the intermediate processing between generic capture and final output also generic, not just the capture layer itself. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=5641">1:34:01</a>

### Data standardization vs customization

- Anders argued the team should maintain strong opinions about the core report data model (what's captured, when, how, and quality standards) to enable downstream data uses and easier product optimization. Releasing this responsibility would create harmonization problems. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=2237">37:17</a>
- Anders emphasized that downstream dependencies need data served at a consistent quality level. If different customers have different concepts (e.g. technical installations), harmonization must happen somewhere, either by plansapp or by end customers. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=2291">38:11</a>
- The team agreed there should be core fields that remain stable across all customers (e.g. electricity price in kWh) even if the point of data capture varies. The data model within a report should be stable while collection timing can be flexible. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=2377">39:37</a>
- Anders warned that without consistent data models, plansapp would face significant problems making building data useful. Having special price 1-8 instead of one standardized price field would create major downstream issues. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=2645">44:05</a>
- Martin distinguished between immutable core data (areas, thermal models, wall thickness/insulation) that doesn't change over time versus recomputable data (electricity prices, B18 calculations) that can be recalculated with different engines later. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=2720">45:20</a>
- Henrik advocated for customers to have flexibility within a defined product scope, where they can choose to use partial functionality (e.g., only JSON output, not PDF) rather than requiring completely different report types for each customer. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=3599">59:59</a>
- Martin argued that adoption depends on having flexibility to customize reports for different customers and markets, though acknowledging this depends on cost. He sees high value in the ability to fork or customize reports for different customer needs. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=3915">1:05:15</a>
- Anders countered that customers (like Sinauland) actually want harmonized report types, not custom ones, because they're becoming integration points for other systems where data quality and consistency across all fields becomes critical. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=3990">1:06:30</a>
- Anders proposed one report with feature flags per instance as a middle ground--having core fields with some hidden/optional based on customer configuration, avoiding parallel reports that serve similar purposes. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=4029">1:07:09</a>
- The team agreed that standardization, reliability, and harmonization have had higher sales traction than flexibility, though Martin noted their customer conversations have been biased toward one audience type in one country so far. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=4549">1:15:49</a>

### Platform modularity and components

- Martin referenced Palantir's Foundry model--starting with custom solutions for each customer, then abstracting common components over time into reusable modules, rather than trying to build perfect abstractions upfront. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=4804">1:20:04</a>
- Henrik proposed having standardized vs. bespoke report buckets--where bespoke prototypes can later be promoted to standardized reports when productized, rather than treating all reports the same way from the start. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=4935">1:22:15</a>
- Martin outlined a modular component architecture: customizable data capture questions feeding into an API, with ready-made components that combine captured data, which can then be assembled into complete reports or used individually. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=5165">1:26:05</a>
- Henrik questioned whether Martin's vision was about the engineering implementation or the business outcomes--whether he wanted the system actually built that way or just wanted certain flexibility outcomes to be possible. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=5274">1:27:54</a>
- The discussion centered on focusing engineering excellence on capture and calculations while allowing more flexibility in visualization components, treating the latter as more 'off-the-shelf' or configurable elements. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=5346">1:29:06</a>
- The proposed system would enable data capture to be configured by domain experts rather than requiring deep engineering involvement, making new report modeling trivial once implemented. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=9240">2:34:00</a>
- There's an inherent conflict between maximizing flexibility for non-engineers to build reports independently and delivering reports quickly--more generic systems take longer than specific, hard-coded solutions. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=9962">2:46:02</a>
- Anders emphasized that optimization should benefit the whole team's velocity, not enable faster iteration for individuals in isolation, requiring a collaborative development model. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=10288">2:51:28</a>
- Martin argued that with AI/LLMs, building the right flexible solution may now be comparable in speed to hacky solutions (weeks vs weeks rather than weeks vs months), assuming problems are well-defined. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=10365">2:52:45</a>
- The team agreed to build generalizability iteratively using reports as forcing functions, with engineers creating initial abstractions and then stress-testing configurability with additional reports to refine the system. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=10681">2:58:01</a>

### Capture vs consumption layers

- The discussion identifies three layers: data capture (can include text and measured geometry/areas), calculation, and visualization (full front-end potentially to XML/PDF). The boundary between these layers is crucial for determining what plans app controls versus what remains flexible. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=1636">27:16</a>
- Anders and Martin aligned on needing significant freedom and experimentation speed in the consumption layer (post-capture), where quick prototyping and iteration is business-critical for driving downstream conversations. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=4266">1:11:06</a>
- Henrik noted that capture and consumption layers are tightly connected--iterating on consumption outputs requires changing capture fields, and adding capture fields requires updating where they appear in outputs, making complete separation challenging. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=4348">1:12:28</a>
- Anders clarified the goal is easy data consumption without same codebase requirements, allowing the business team to work separately from core product while still accessing captured data effectively. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=4411">1:13:31</a>
- Martin emphasized the consumption/visualization layer needs full flexibility as it's where customer-facing artifacts (like PDFs) are created, which may require bridges or transformations from the core platform data. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=4463">1:14:23</a>
- The team acknowledged that capture reliability is paramount since it happens on-site with no ability to go back, requiring higher reliability standards than the mid-stream consumption components. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=5381">1:29:41</a>
- Martin argued for separating capture and visualization concerns, suggesting the team is trying to optimize both simultaneously, which increases complexity when they could be independent workstreams. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=10085">2:48:05</a>
- The French team has documented data capture requirements line-by-line for every calculation needed at building, element, and other levels, with clear API specifications. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=9149">2:32:29</a>
- Discussion confirmed that data capture configuration and report artifact generation are separable, though setting up report access, customer permissions, and field metadata still requires some engineering coordination beyond just content definition. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=10535">2:55:35</a>

### Business goals and priorities

- Anders clarified it's not a business goal for customers to create their own reports, but it is critical that it's quick for plansapp to add new reports. The goal is speed of implementation by the team, not customer self-service. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=5856">1:37:36</a>
- A key business goal is serving harmonized data for downstream consumption (banks, insurance companies, real estate agents). If a field exists, it must mean the same thing regardless of who captured it (e.g., a U-value of 0.5 can't have different meanings). <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=5921">1:38:41</a>
- Henrik emphasized that capturing building data is the design - they make money when they have more building data. Reports (EPC, PDFs, XMLs) are means to an end, and the company wants to minimize the cost of producing these outputs while maximizing data capture. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=6199">1:43:19</a>
- Henrik clarified that if surveyors could capture even more data than EPC requires and they had the volume, that would be preferable. They're only bound by EPC requirements due to current commercial obligations and market realities, not because EPC defines their ideal data capture scope. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=6366">1:46:06</a>
- The team aligned on 'time to new report' as the overarching metric, with Anders noting there's both the metric itself and the real-world calendar timeline for adding specific reports. If they optimized solely for the metric, they'd build everything modularly now, but immediate business needs require a pragmatic approach. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=6887">1:54:47</a>
- The team committed to delivering eight reports to a customer, with flexibility to adjust the delivery sequence based on what optimizes speed for plansapp's interests. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=9003">2:30:03</a>
- Reports are blocking commercial progress and need to be built as quickly as possible, ideally on the new platform rather than using temporary solutions. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=9492">2:38:12</a>
- Henrik expressed that his expectation from the hackathon was a generic way of thinking about reports enabling anyone to create them, not just regulated reports like EBC. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=8384">2:19:44</a>
- Henrik asked whether the generic report system is more important than saving Mark's time, completing 3D editing, or capturing more data--all competing priorities for September. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=8405">2:20:05</a>

### Timeline and sequencing strategy

- The initial commitment discussed was to ship DK decay condition and electrical reports by September 2026 to enable rollout to at least one center, though the firm deadline was later questioned. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=7301">2:01:41</a>
- The definition of 'ship' means the product can be rolled out to a center or more, requiring testing and iteration beforehand, with the team committing to not be the blocker for rollout. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=7664">2:07:44</a>
- Henrik later clarified that dates are flexible since nothing is signed, though business reality suggests the reports were originally supposed to ship before summer (per December agreement). <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=8598">2:23:18</a>
- Concern raised that with Mark's parental leave in mid-September and current planning approach, the team is unlikely to meet the September deadline as things stand. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=8307">2:18:27</a>
- The September 1st target date is aspirational rather than a hard external constraint, with Anders suggesting that even a July 1st aspiration wouldn't fundamentally change the engineering approach. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=9521">2:38:41</a>
- The team has spent approximately one month since the AI DevX week to reach current alignment questions, with concerns that this timeline has reduced flexibility for decision-making given only three months remain. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=9755">2:42:35</a>
- Delayed alignment discussions have created time pressure, forcing choices between competing priorities that might not have been necessary a month earlier with more lead time. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=9785">2:43:05</a>
- Henrik pushed the team to focus on defining business and product goals first before discussing specific solutions (module systems, report bundles, architecture). The solution architecture should follow from clear alignment on higher-level objectives. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=5782">1:36:22</a>
- Anders introduced the concept of sequencing from custom to packaged to modules - the immediate implementation might be a relatively opinionated EPC because that's faster to build right now, but over time the system would evolve toward greater modularity and flexibility. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=6696">1:51:36</a>
- The team acknowledged disagreement on the cost and timeline to achieve full modularity. Henrik and Marina believe building EPC fully generically would be a very big investment, which affects sequencing decisions even though they agree on the directional 'cone' goal. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=6852">1:54:12</a>

### Development approach and resources

- Anders advocated for replicating the existing whose web flow rather than adding value, arguing that 'it looks like what I already have today' is the key lever for faster adoption, not innovation. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=7808">2:10:08</a>
- Anders argued that building on the new platform while knowing all required fields provides an 'incredible forcing function to build the right architecture', preferring this over short-term optimizations. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=8003">2:13:23</a>
- Henrik questioned whether using AI agents to build cleanly from scratch might be faster than iterative development, given that implementation is now relatively short once context is defined. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=8515">2:21:55</a>
- Martin explained the incremental approach: start with 2 DK reports to learn abstractions, then quickly add 4 French reports using those learnings, rather than building a complete generic platform first. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=8660">2:24:20</a>
- Anders clarified the hackathon goal was to 'increase the speed of reports', not enable anyone to add reports--configurability is a means to that end, not the goal itself. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=8888">2:28:08</a>
- Anders proposed that Nils should build the reports end-to-end, though this would require the flexible platform to be built first, creating a sequencing challenge. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=7715">2:08:35</a>
- Henrik suggested Anders should do the product work defining requirements (what questions to ask, what data to capture) while engineers implement, leveraging Anders' deep knowledge of the AIM(?) system. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=7757">2:09:17</a>
- Anders proposed allocating 4 engineers to reports and 2 to 3D editing, with one engineer each on French and Danish reports plus two on genericness as a potential approach. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=8840">2:27:20</a>
- Discussion left open whether to have engineers build French and Danish reports in a 'hacky way' in parallel while two others focus on genericness, allowing learnings to flow between tracks. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=8976">2:29:36</a>

### Ownership and domain knowledge

- Concerns were raised about who has full context for defining report requirements across 8+ reports, as the team isn't certain about exact deliverables even for the project report. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=9078">2:31:18</a>
- Henrik volunteered to own French report delivery end-to-end, taking responsibility and risk for building reports using APIs and structured data, with ability to request engineering support if complexity grows. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=9111">2:31:51</a>
- Martin emphasized wanting to remove the burden of deep report domain knowledge from the engineering team, arguing they shouldn't need to understand 1500+ pages of documentation for electrical, M system, and other specialized reports. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=10138">2:48:58</a>
- The engineering team clarified they have reviewed all report requirements and understand content, though defining content differs from end-to-end ownership--engineers can still provide scaffolding and infrastructure support. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=10420">2:53:40</a>
- Anders should define product requirements and what questions need to be asked, with engineers implementing accordingly rather than engineers needing to understand Butcheck users' needs directly. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=7587">2:06:27</a>

### Market expansion considerations

- Martin raised concerns about operating only with short-term Denmark/France focus, asking what happens when opportunities arise in Norway, UK (with rigs surveys), or Germany that may need different approaches. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=4161">1:09:21</a>
- Martin argued their mandate is broader than surveyors--citing mine working's 140,000 visits as significantly larger than the surveyor market--requiring flexibility to sell to different audiences across countries. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=4603">1:16:43</a>
- Anders distinguished between regulated reports (known shapes per country) vs. custom needs like mine working's UK sea(?)-type reports, suggesting custom reports should still follow a product development approach rather than sales-driven configuration. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=4664">1:17:44</a>
- Anders suggested knowledge problems over configurability problems--expecting that what mine working needs will be similar to what other real estate agents need for pre-sale visits, making it a product development speed issue. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=4757">1:19:17</a>
- From an engineering standpoint, any reports could be built first (French or Danish)--the choice is purely commercially driven based on which markets to sell to first. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=8805">2:26:45</a>

### Product scope and trade-offs

- The team's value to surveyors is providing tools to save themselves from audits and compliance through proof and evidence, not just being a nicer report tool. This relates to whether plans app should solve 100% of the surveyor's job-to-be-done. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=910">15:10</a>
- Empirical learning: when plans app starts registering a data area, they need to complete it so customers can put data almost directly into reports. Half-done implementations have consistently been problematic - better to do fewer things completely than many things partially. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=1031">17:11</a>
- Technical installations is cited as an example where plans app had to go much further than preferred once they started. Now they're likely adding proposals as well, which may not have been the intended scope initially. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=1066">17:46</a>
- The ability for customers to create custom reports is seen as incredibly unimportant for a long time. Customers lack technical competency to customize data flows themselves - there's zero overlap between those with technical understanding and those who can answer domain questions reliably. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=1100">18:20</a>
- Martin pointed out that surveyors aren't happy using the current RIP product because it's too open and needs more guided, opinionated UX. This demonstrates the tension between making things completely generic versus providing structured workflows. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=5745">1:35:45</a>
- Anders emphasized that differentiation comes from who does the work and text content, not variables (which are computed the same way), and expects cross-selling opportunities like Klima Sickening(?) to benefit from unified data models. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=4114">1:08:34</a>
- Anders noted the team faces a choice between generic system, hitting DK deadlines, France platform, and 3D editing--unlikely all can be completed in 3 months with 6 engineers. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=8432">2:20:32</a>

### Customer requirements and customization

- The team needs to output many reports this year, with two new reports ready in three months. Currently requires changes across all three platforms, and they need a more streamlined approach. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=198">03:18</a>
- Even regulatory compliance has flexible boundaries. For example, with XML output for E10(?), different customers (CoreShape vs Polcheck(?)) might want different levels of pre-processing - some might want full XML submission while others handle more in their own systems. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=727">12:07</a>
- Real-world example: Some customers want custom questions per insurance company that procure EPC reports. Currently done via pre-visit forms with questions like 'totally renovate the home' that flow to insurance companies but not to property sellers. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=1177">19:37</a>
- The custom questions are delivered as sidecar PDFs alongside final reports, containing data points that flow to insurance companies. This level of customization exists but customers expect plans app to implement it, not control it themselves. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=1268">21:08</a>
- Different report variations could exist per customer - EPC reports sold to different companies might be different reports. This already happens today, though the specific implementation varies. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=1348">22:28</a>
- Even non-regulated reports can have the same inflexibility as EPCs if they're built end-to-end for specific client relationships. The UI and downstream integrations create de facto regulation regardless of whether national regulations apply. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=1463">24:23</a>
- Currently plans app is building most of the flow and tailoring exactly to customer workflows to achieve adoption, which creates pain. The team is considering whether this is what the market needs versus what they want to build. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=1760">29:20</a>
- Martin proposed a minimal core for EPC where text could theoretically be eliminated and U-values manually input, suggesting the minimum viable data set is quite small but gets larger due to customization needs and customer-specific requirements. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=2493">41:33</a>
- The team discussed emerging customization needs: Orholts wants to control exact material thickness, Juteslot(?) wants more window details, suggesting the core standardized portion may shrink as customer-specific requirements grow. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=2541">42:21</a>
- Martin described a potential API-first approach where Mine Working could use a simple app with room plan, RGB, pictures, and free text annotation, then export JSON for their 30 engineers to process independently. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=2837">47:17</a>
- Anders acknowledged that ideally everything would be customizable and still useful downstream, but the trade-offs are unclear. Infinite customizability with LLM would be hugely more costly than maintaining strict opinions. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=3105">51:45</a>
- Henrik questioned whether plansapp should maintain responsibility for superior UX guiding users to compliant EPC all the way through to export, or shift to a more API-focused model. This remains an open question requiring alignment on strategy and engineering cost trade-offs. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=3327">55:27</a>

### Other updates

- Anders framed the product challenge as having two primary blockers: (A) it's hard to change the system right now, and (B) there's a catch-22 where they can't spec solutions without showing customers something, but customers can't help them spec before seeing something. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=6780">1:53:00</a>
- The team has 21 months of runway currently, with 3 months until September leaving 18 months afterward, though French reports and other priorities create capacity constraints. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=8261">2:17:41</a>
- Henrik's understanding is the team will integrate with Butcheck's server (not whose web directly), delivering XML as currently done, though the exact integration approach needs clarification with Thomas. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=7474">2:04:34</a>
- The team needs to capture data in a specific way to integrate with Butcheck, with questions asked potentially differing from what the Butcheck API strictly requires (may be more or less). <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=7549">2:05:49</a>
- Martin clarified that sequencing was based on perceived importance, with mind working apparently more important than DK reports, though this wasn't previously clear to the team. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=8550">2:22:30</a>
- There's significant uncertainty remaining about the engineering solution despite a month of exploration, though the team has worked on reusable components and infrastructure during this period. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=9636">2:40:36</a>
- The team spent 90 minutes on alignment version 1 without reaching clarity early enough, contributing to misalignment persisting this late in the process. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=9893">2:44:53</a>
- Anders identified that too few options are being cut off during discussions, with attempts to optimize for everything and maintain all flexibility creating navigation costs and slowing decision velocity. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=9936">2:45:36</a>
- Henrik stressed the need to move through alignment questions systematically before discussing estimates or timelines, as sequencing and product decisions must precede engineering scoping. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=10240">2:50:40</a>
- The team acknowledged they'll need to iteratively refine the system as they implement reports, recognizing they can't know everything upfront and will discover adjustments needed even after reviewing French reports and other requirements. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=10760">2:59:20</a>
- The meeting ran over time, with the team agreeing to continue until 6pm to finish the alignment discussion since Marina is off tomorrow and the next opportunity would be Monday. They took a quick break around 01:57:03. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=6992">1:56:32</a>
- Henrik's slides about external commitments don't mandate specific DK electrical/conditioning implementation details, just alignment on whether hard timeline constraints exist to inform sequencing decisions. <a href="/stream/Tm90ZVN0cmVhbTowZmFrZWJtc2ltajRnZnRucWh2ZjJ0dWJsNTptZWV0aW5nOjEwODgyNg==/#Tm90ZTo3ODMxMjM2NA==//recording/TWVldGluZ1JlY29yZGluZzoxMzg5NTY2Mg==&ts=10172">2:49:32</a>

## Transcript

[0:00:00] Anders Valentin: Thomas.
[0:00:01] Martin Collignon (?): No.
[0:00:23] Anders Valentin: Escaping or summer while.
[0:00:30] Martin Collignon (?): And then nothing happened and everyone's happy. They get promoted.
[0:00:49] Anders Valentin: The cannabis God martining. Yeah.
[0:00:57] Anders Valentin (?): Please. Yes. Have you read it?
[0:01:09] Anders Valentin: Some. Some. Some of it. I didn't have that much time but I. I think I. Yeah, never mind.
[0:01:20] Martin Collignon (?): Okay. What are you feeling okay?
[0:01:29] Anders Valentin: I'm more like bored and. And therefore working. I probably shouldn't but yeah.
[0:01:40] Martin Collignon (?): I'm.
[0:01:41] Anders Valentin: I'm so tired and still. Still coughing. It's gotten better today actually.
[0:01:53] Anders Valentin (?): So he's not gone.
[0:01:59] Martin Collignon (?): But.
[0:02:00] Anders Valentin (?): Yes.
[0:02:01] Martin Collignon (?): I want.
[0:02:01] Anders Valentin (?): We want to talk on business product level on this new.
[0:02:05] Anders Valentin (?): This reports track the main outcome or what I hope to achieve from this meeting is that we actually can start making a going in engineering mode with this so we can align on this level and then you don't have to think about the rest Martin analysis.
[0:02:25] Anders Valentin (?): We'll figure it out. And I think it's fine.
[0:02:29] Anders Valentin (?): There's some open questions which probably we can get an answer to.
[0:02:32] Anders Valentin (?): But then it's at least explicit that are still some things that we can't really say for sure right now.
[0:02:39] Anders Valentin (?): I think that is fine as long as just clear what it is we are not certain about. Does that make sense?
[0:02:52] Anders Valentin (?): There are sort of six different alignments.
[0:02:55] Anders Valentin (?): I've tried to group it incidentally so I think we can just start from the top and see if we see the same world here where the first one is the problem and where is that the sort of reports we need to.
[0:03:08] Anders Valentin (?): To solve. So basically just to rephrase this a little bit.
[0:03:18] Anders Valentin (?): We need to output a lot of reports this year and in three months we need to have two ready new reports ready or this already come into that later after and we need to find a system for how we can scale this or do that quickly basically high level right now it requires changes on all three platforms.
[0:03:37] Anders Valentin (?): We need to find a better way to technical or product wise to.
[0:03:40] Anders Valentin (?): To to do that more simple or streamlined basically. Right.
[0:03:45] Anders Valentin (?): And the way we see it right now by analyzing all the contexts we have in DK and all the contact from the French guys made for friends and what you did Martin and blah blah and what we hear about other reports that are not regulated.
[0:03:59] Anders Valentin (?): I feel we.
[0:04:01] Anders Valentin (?): We see sort of three different buckets of reports regulated but they're more inspection driven.
[0:04:08] Anders Valentin (?): So these are more the electricity and partition reporting pk.
[0:04:12] Anders Valentin (?): They're not related to the property or the home in itself.
[0:04:16] Anders Valentin (?): And then we have the regulated computation driven which is their computation because they need to have the whole home state of the property in sort of in context for producing ABCs you need to have all this spatial awareness, blah blah to produce the accurate output.
[0:04:33] Anders Valentin (?): And we see a distinction there in terms of what they are.
[0:04:37] Anders Valentin (?): And then we have these more custom bespoke reports which can be completely customized for the individual customer.
[0:04:47] Anders Valentin (?): Is this, do you see this differently or
[0:04:54] Anders Valentin: the, the only thing I, I, I thought of looking at this is what's the difference between the bespoke and the first one is that simply who does the specking.
[0:05:06] Anders Valentin (?): I think the, the part where here is that it's actually regulated. Right.
[0:05:10] Anders Valentin (?): So that's the we we have to comply with a certain output or regulatory compliance output.
[0:05:17] Anders Valentin (?): So I think that's different for me where we need to ensure we actually fulfill that.
[0:05:21] Anders Valentin (?): Whereas with the custom we don't care or don't care.
[0:05:26] Anders Valentin (?): We don't need to force any thing in the system to produce this output,
[0:05:32] Martin Collignon (?): if that makes sense.
[0:05:33] Anders Valentin: And you have fewer downstream dependencies because you don't have to fit into something existing necessarily.
[0:05:40] Martin Collignon (?): This you could imagine in the C case that we allow someone to build the data model themselves because we don't need to actually take responsibility for the outcome.
[0:05:50] Martin Collignon (?): Whereas in A we need to actually ensure that it lives up to regulations.
[0:05:55] Martin Collignon (?): Okay.
[0:05:56] Martin Collignon (?): So C is more like a super generic system where it is more like we have to have some level of control.
[0:06:03] Martin Collignon (?): Okay,
[0:06:06] Martin Collignon (?): to your original question on this I would answer yes.
[0:06:11] Martin Collignon (?): The difference is who, who specs it in the way or we'll get to that in one of the further down.
[0:06:19] Martin Collignon (?): Yeah, but, but, but in general. Yes, that's how I see.
[0:06:26] Anders Valentin (?): Yeah.
[0:06:26] Martin Collignon (?): Okay.
[0:06:27] Anders Valentin (?): This is why initially from this exactly. Just initiated by. That's the difference.
[0:06:31] Martin Collignon (?): Right.
[0:06:33] Anders Valentin (?): That sort of means those cascade to those things me and Wallace talked about I guess. Martin, any.
[0:06:44] Martin Collignon (?): Yeah, I agree. In the sense that.
[0:06:52] Martin Collignon (?): There is functionally no difference in the sense that this is just a bar we set.
[0:06:56] Martin Collignon (?): What I mean by that is true suit and rule check and rule check.
[0:07:00] Martin Collignon (?): Soon you're going to have different ideas what compliance means.
[0:07:04] Martin Collignon (?): So I think there's more bar and something where we like take any responsibility in what's being shared.
[0:07:12] Martin Collignon (?): But let's say a true suit says tomorrow we want desperate. We just want half the field.
[0:07:15] Martin Collignon (?): The project does and we judges combined. Do I care?
[0:07:21] Anders Valentin (?): Yeah.
[0:07:22] Martin Collignon (?): Okay.
[0:07:23] Anders Valentin (?): But that comes back to our.
[0:07:24] Anders Valentin: Wow.
[0:07:25] Anders Valentin (?): The boundary I guess of the product. Right.
[0:07:27] Martin Collignon (?): For me I see more that the reason we give so much in EPC and in other reports is because we cannot expect our customers to themselves define these things.
[0:07:37] Martin Collignon (?): So like at the roots all of them could use a room Plan API and then just get the API.
[0:07:45] Martin Collignon (?): Then they build their stuff on top of that. But it's not going to happen. Right. They need.
[0:07:48] Martin Collignon (?): They cannot do that.
[0:07:49] Martin Collignon (?): So we need to support them more technical stuff and pre digest more so they can have something that's useful.
[0:07:55] Martin Collignon (?): So it goes closer to where their operational reality is. But that operational reality is not.
[0:08:01] Martin Collignon (?): I mean it's driven by compliance but also by the stuff like it's also driven by practical efficiency in the field like field versus office and these kind of things which are not bound by regulation.
[0:08:12] Martin Collignon (?): That makes sense. So this is just to.
[0:08:18] Martin Collignon (?): You can see the 2 1/2 centimeter we added to the frames of Windows. We can't. We give a.
[0:08:27] Martin Collignon (?): No client wants it. Do it, we do it. It's not regulation.
[0:08:33] Anders Valentin (?): It is a regulation. Right, because it's. Because we need to have the measurement from the outside.
[0:08:37] Martin Collignon (?): Right?
[0:08:39] Martin Collignon (?): Yeah. Or we think your average is actually better than the average plus 2.5.
[0:08:44] Martin Collignon (?): We also get to that where the. One of the further downs where it is something about it.
[0:08:50] Martin Collignon (?): You could have customer specific defaults and some things showing, not showing, but not completely different reports implemented differently, structured differently for a customer.
[0:09:03] Martin Collignon (?): But that's also a decision to take
[0:09:05] Martin Collignon (?): the distinction here is that in a we sort of have a boundary where we know what everything is because we need.
[0:09:14] Martin Collignon (?): We know we need to match specific things from something that is like generic.
[0:09:19] Martin Collignon (?): Whereas in C you have to ensure that everything is sort of generic upstream.
[0:09:25] Martin Collignon (?): You have to make everything downstream also generic. Where it's sort of like a CMS
[0:09:29] Martin Collignon (?): with like imagine it's more solutions suggestions for that. But that's customer.
[0:09:37] Martin Collignon (?): It's like anything you want we can.
[0:09:39] Martin Collignon (?): Right, but I'm speaking about this thing where you.
[0:09:42] Martin Collignon (?): You're talking about company specific requirements and being able to sort of model that.
[0:09:49] Martin Collignon (?): Right.
[0:09:50] Martin Collignon (?): Say budget, want something, but they want something else.
[0:09:53] Martin Collignon (?): My point is that compliance.
[0:09:56] Martin Collignon (?): I don't think compliance is right place to like because compliance is basically we are above compliance.
[0:10:03] Martin Collignon (?): Now we could argue that we are compliant for a long time because you can manually change things.
[0:10:08] Martin Collignon (?): And yes, we do more because Core Shape asks us to do more. Right. So it's more that this.
[0:10:13] Martin Collignon (?): I don't think this boundary is maybe the right one in the sense that you could argue that A is a subset of these progressive reports.
[0:10:23] Martin Collignon (?): And for me I would look more at the boundary as well.
[0:10:30] Martin Collignon (?): We can go back to what we try to solve for as well. Who should have.
[0:10:33] Martin Collignon (?): Who should know everything, who should know the domain context.
[0:10:37] Martin Collignon (?): And this is also why we say we is a bit of a dangerous term because I think this is also the difference between should all the engineers know it to one engineer know it.
[0:10:47] Martin Collignon (?): Should any of the engineers know it.
[0:10:49] Martin Collignon (?): We'll get to that.
[0:10:49] Martin Collignon (?): Yeah, yeah. Or should anyone in the company know it. Right.
[0:10:52] Martin Collignon (?): And I think that's where maybe more boundaries do like, do appear
[0:10:59] Martin Collignon (?): to me that's also different because like you could say you have custom reports, let's see but they're still defined by plans that some shared pool budget like that's unrelated.
[0:11:10] Martin Collignon (?): Like who does it? It's, it's, it's related. But, but do we see the world the same way?
[0:11:16] Martin Collignon (?): And I think that's like where the boundary is, is maybe not, it's more fuzzy to you that, that, that, that, that I, I, I follow.
[0:11:24] Martin Collignon (?): I, I just think we, we actually right now we don't have any A or B, everything of it.
[0:11:32] Martin Collignon (?): But so what I mean by that is tomorrow if you should say oh, we won't be customers here, but you need to change these things.
[0:11:40] Martin Collignon (?): Yeah, but this is not about implementation of how the system is today.
[0:11:44] Martin Collignon (?): It's more like what kind of reports exist out there.
[0:11:47] Anders Valentin (?): But I'm actually curious there because
[0:11:50] Martin Collignon (?): we
[0:11:50] Anders Valentin (?): need that like today you see that's an XML output we need, we need to satisfy.
[0:11:55] Anders Valentin (?): That's what we are building against. So how can that not be?
[0:12:00] Anders Valentin (?): Why is that not the same for 2cit if they want to make EPC reports?
[0:12:04] Martin Collignon (?): So that's a good example.
[0:12:07] Martin Collignon (?): The XML is a format and I guess the boundary that we have is what's the minimal state of the XML that we need to do to fit into E10?
[0:12:14] Martin Collignon (?): Maybe says, you know guys just do the thermal envelope tech consolidations will do everything in E10.
[0:12:19] Martin Collignon (?): And Polcheck says, you know what I actually do as mushrooms?
[0:12:25] Martin Collignon (?): Just send the XML until submission based migrate with that.
[0:12:29] Martin Collignon (?): I mean I'm not saying we should say yes to that or not.
[0:12:32] Martin Collignon (?): My point is more that even that compliance in here is a flexible boundary.
[0:12:37] Martin Collignon (?): The boundary that we have right now is we need to put it in like an XML that's accepted by E10.
[0:12:43] Martin Collignon (?): There's a world in which we have actually even less.
[0:12:45] Martin Collignon (?): We just send the JSON to keep Kim does his stuff, converts it to XML and he's the one that sends you E10.
[0:12:53] Martin Collignon (?): Yeah, it's unrelated to compliance.
[0:12:55] Martin Collignon (?): Yeah, it's like it's, it's not, we are not bounded by compliance right now.
[0:13:00] Martin Collignon (?): And you could argue the XML that we send up right now is not compliant.
[0:13:06] Martin Collignon (?): Like you will not be accepted by keyflies.
[0:13:08] Anders Valentin (?): No, no. But even if you say half compliant, you are still satisfying half of the spec. Right.
[0:13:14] Martin Collignon (?): You. Yeah.
[0:13:16] Martin Collignon (?): You know something about what you're actually creating where in a generic where we don't control whatever they create.
[0:13:23] Martin Collignon (?): Like we don't like it. It can be anything.
[0:13:25] Martin Collignon (?): Right.
[0:13:26] Martin Collignon (?): So that's the distinction I see between A and C is that in A we have some boundary.
[0:13:31] Martin Collignon (?): We know that the report data model for what we're allowing capture of is an inspection report or you know, PPC report.
[0:13:40] Martin Collignon (?): Yeah.
[0:13:40] Martin Collignon (?): In C.
[0:13:41] Martin Collignon (?): And it's sort of like allowing anyone to create anything and then it's sort of sort of like a cms.
[0:13:50] Martin Collignon (?): I don't actually see what can I see between.
[0:13:54] Martin Collignon (?): To me what you're saying is a solution to see. You could still say Martin white coat clemency.
[0:14:04] Martin Collignon (?): It's not that they can define anything, but it's that it's a customer completely custom.
[0:14:08] Martin Collignon (?): Who you can tailor to. Yeah.
[0:14:13] Martin Collignon (?): Whoever wants it, how it's who does it and how at least at this level of this first stage of alignment is details.
[0:14:23] Anders Valentin (?): Okay.
[0:14:23] Martin Collignon (?): Sorry I hijacked the presentation here just to the same alignment we did to the other track.
[0:14:29] Martin Collignon (?): Just to highlight what we've in the 3D Myra board.
[0:14:35] Martin Collignon (?): Yes.
[0:14:35] Martin Collignon (?): Would we try to sort of also some. Some of that. Some of that related. There is.
[0:14:43] Martin Collignon (?): Yeah but I can only see the last.
[0:14:45] Martin Collignon (?): You have it there the no 3D editing the last time there that we. We. We also talked about it there.
[0:14:55] Martin Collignon (?): Right. Like what do we. What do we do for. For the surveyors like in the way the green checks.
[0:15:04] Martin Collignon (?): I don't see my pointer.
[0:15:05] Martin Collignon (?): But here we serve building surveyors to give them the best tools to solve their server jobs.
[0:15:10] Martin Collignon (?): And there's something there with what you Martin phrased here. Like we replace a core process.
[0:15:18] Martin Collignon (?): There's something to close to 100% and this is how we get adoption is by helping them do their jobs.
[0:15:26] Martin Collignon (?): Which is related to.
[0:15:28] Martin Collignon (?): We haven't written down what we discussed but I've been looking this week at our discussions from the 3D track.
[0:15:35] Martin Collignon (?): We talked a lot about trust and giving them basically the tools to save themselves from audits and compliance.
[0:15:44] Martin Collignon (?): And this is where our value is to surveyors. Right.
[0:15:48] Martin Collignon (?): To not just be a nicer tool to do reports compared to the tools they have today, but to get that key thing which is proof.
[0:15:59] Martin Collignon (?): Right. Proof and evidence and all that stuff.
[0:16:03] Martin Collignon (?): And to me that relates to like should we do half of it and then eat and do the rest.
[0:16:10] Martin Collignon (?): The world is open, but it's more like if we say that do we solve the 100%?
[0:16:17] Martin Collignon (?): I know 100% here means something else, but more like do we say that we solve jobs to be done for them, even though we as a data company don't care about that?
[0:16:27] Martin Collignon (?): But is that a way we get them to use a product that can do whatever 100% means for a given thing or whatever?
[0:16:36] Martin Collignon (?): The boundary we want to draw is
[0:16:38] Martin Collignon (?): what is within our product scope.
[0:16:41] Martin Collignon (?): I honestly think it matters that much. So I don't think that's what we should spend.
[0:16:46] Martin Collignon (?): I mean Anas wants to say something. I don't necessarily think we should spend too much time on that.
[0:16:49] Martin Collignon (?): It's more to say that I'm not sure.
[0:16:51] Martin Collignon (?): I view that as a, as a thing that's clear cut as being here, but I don't think it matters that much.
[0:16:58] Martin Collignon (?): It matters because we use these a lot.
[0:17:00] Martin Collignon (?): Like these we always talk about reports A, reports B and C is out of scope and blah blah. So.
[0:17:09] Marina Vatmakhter: I'm just adding 2 cents.
[0:17:11] Marina Vatmakhter: I think one point is we've learned that if we do something, if we start registering a data area, we need to do it till that they can put it almost directly into the report.
[0:17:22] Marina Vatmakhter: I think that's been our hard earned empirical learning that, that we can't half do things.
[0:17:29] Marina Vatmakhter: Then it's better to do less things but do the things we do completely where that's possible.
[0:17:37] Marina Vatmakhter: I think that's, that's one thing.
[0:17:38] Marina Vatmakhter: Whenever we had an unclear handover to another system, it's been bad and we had to build the whole thing.
[0:17:46] Marina Vatmakhter: So like technical installations, I know we like to batter that all the time. Which is not the.
[0:17:51] Marina Vatmakhter: But that's a good example of where we actually had to go much further than we would have preferred.
[0:17:57] Marina Vatmakhter: The moment we start messing with the rest of the data flow, we actually almost have to complete it.
[0:18:02] Marina Vatmakhter: And now we're probably going to do proposals as well.
[0:18:04] Martin Collignon (?): Right?
[0:18:06] Marina Vatmakhter: Which is maybe not the cut we would have liked.
[0:18:11] Marina Vatmakhter: Point A, point B to me, for what it's worth and I know that gets maybe solutionary but it's, it's hard not to do.
[0:18:20] Marina Vatmakhter: I don't see bespoke the ability for them to create reports.
[0:18:24] Marina Vatmakhter: I see that as incredibly unimportant for a long time that I for, for the harmonization that I have seen just with this, this team.
[0:18:35] Marina Vatmakhter: They as, as Martin has said many times, they will not be able to customize the flow of data in there.
[0:18:42] Marina Vatmakhter: They simply do not.
[0:18:44] Marina Vatmakhter: There's no overlap between the people who have the Technical competency to understand that and the people who are able to answer those questions reliably.
[0:18:51] Marina Vatmakhter: The overlap is 0.
[0:18:56] Martin Collignon (?): Sorry, overlap is 24 million at dwarf and Obihong.
[0:19:00] Anders Valentin: Yeah. Yes, that is a very, very slim overlap and therefore very expensive.
[0:19:05] Anders Valentin: Getting two people to talk together, I think catalogs, texts, that kind of stuff they understand, but a report flow they will not understand.
[0:19:14] Anders Valentin: They can barely understand it. Writing it in PowerPoints.
[0:19:18] Anders Valentin: So getting that technically down into a system that's cohesive, I doubt that will happen.
[0:19:24] Anders Valentin: So we will be in the loop one way or another.
[0:19:26] Anders Valentin: Which means that we will always have the possibility, I think at least for a long time, where we will be able to control how new reports get ingested to the point about the energy label.
[0:19:37] Anders Valentin: I was just in a conversation with Eve and, and Thomas was careful not selling this, but he really wants to sell it, which is that there can be custom questions depending on who procured the report.
[0:19:52] Anders Valentin: So in his ideal world you could add like some form in the end that, that, that has additional questions only when Eve books the, the survey or when someone else books the survey.
[0:20:07] Anders Valentin: I, I just want to add it as a data point. That's the level of bespokeness.
[0:20:10] Anders Valentin: But he expects us to input that into the report.
[0:20:14] Anders Valentin: So I don't think he has any idea that he wants to control that himself.
[0:20:18] Anders Valentin (?): Report like EPC report or.
[0:20:20] Anders Valentin: Yeah, like even in the EPC report they currently do it for the Celia Blue Stinger.
[0:20:25] Anders Valentin: So the, the pre visit formula that they send out before the person goes on site, they have per insurance company questions that, that get asked there and it's, it's a likely jump.
[0:20:40] Anders Valentin: He was careful not to say it, but I could see it was.
[0:20:43] Anders Valentin: He was on his, on his tongue on the edge of his mouth.
[0:20:47] Anders Valentin: That, that, that would be a way he would want to differentiate at some point.
[0:20:51] Anders Valentin: I don't think we have to deal with that right now. I'm just saying it exists in the world.
[0:20:56] Anders Valentin: Yeah, that was my, my my, my two cents.

> Transcript pasted in the chat continues beyond this point through 2:59:55. The full AI-notes sections above capture the remaining decisions, topics, and timestamps from the provided source.
