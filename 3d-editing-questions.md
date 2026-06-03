---
type: Note
relationships:
  Type:
    - "[[note]]"
related_to: "[[decisions]]"
---
# Report Track Questions

Questions for report track (some related to a long conversation I had with Martin today).

## Flow & opinionation

- Are we fine with enforcing a specific/opinionated flow per report?
  - Hence the same flow for all customers using that report.
  - Later on it can probably be more modular.

## Custom/bespoke vs. generic

- Today we are building custom/bespoke things for EPC/Botjek — do we want to set explicit constraints going forward that we don't want to do that anymore?
  - Is that a feasible constraint to set? E.g. where is the line between "genericness" and "custom/bespoke" things?

## Ownership & definition of done

- Who defines the output/delivery of a report?
- When is a report Complete / Good enough?
  - Iterative with first customer / design partner until they are ready for adoption in their whole organization?
- If the goal is that idea-to-prototype is incredibly fast (hours, not weeks), does that mean we should prototype directly on prod?

## Forward deployed engineer scope

- Is it a requirement that reports can be fully produced by a forward deployed engineer (Niels, Martin, Anders)?
  - In case that means the forward deployed engineer has to make decisions on:
    - UX / UI for that report
    - Data shape / model for that report
  - Is that how we want to operate? E.g. optimising for siloing / single thread per report.

## Data capture vs. visualisation

- Should the "core platform" only be responsible for raw capture and calculations — or does it also own derived/computed data?
  - Thermal envelope overview
  - HBEMO logic
  - Proposals
  - Texts
  - Measured areas
- Where does the visualization layer start — is it purely rendering, or does it include derived data too?
- Do we want the visualization layer isolated per report, or do we need cohesiveness across reports — e.g. shared concepts like case states, assignees, report status? (Think: harmonisation on downstream output.)

## Boundary / direction to optimize for

- Is the boundary/direction we want to optimize for the following?
  - Data capture & calculations (needs to be clearly defined) is core platform.
  - Report making / artifacts (on top of data capture) is owned by forward deployed engineers.

## Scan granularity

- Are we fine with a scan per home visit, or do we need it on case level (2 surveyors doing different reports on the same property)?
