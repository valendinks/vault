---
type: Note
_width: wide
---
# Decisions

## FDEs vs. Core - Roles

We want to operate with two roles: FDE and Core

Role is NOT fixed, nor bound to current titles, but a timed responsibility.

### FDE (Forward Deployed Engineer)

Role is to

1. Bridge the product between core-platform with client & report reality
   1. Owns what correct is
   2. Owns the part of Tasks that cannot be moved
2. Implement reports using Core data components
3. Implement artifacts using Core data components

##### Core vs. Surface vs Capture

- I think we need to separate that configuration happens both on the capture layer and then also the "data transformation layer". Is capture composed of "free" objects that dont live in core (just json) or are they objects stored in core (let's say that real estate agents want a "stemningsbillede" - is that a json or a core object?
  - You could see it as in core it's an "Image" with metadata properties
  - In capture it's a image note type (configured)
  - In the visualization/integration layer it's an api call, with the metadata as a filter

#####

- You

##

## 3D Track

### Unstated Assumptions

**Cost to Fix 3D vs. Cost to be QA vs. SLA Time**

**Cost to Fix 3D**

1. Cost per case of Phillippines team fixing wrong scans
2. Cost of Mika QA on 3D edits alone
3. Cost of Final QA on 3D edits alone

**Cost to be QA**

1.

|                                         |                                |               |                               |                                  |              |    |      |
| --------------------------------------- | ------------------------------ | ------------- | ----------------------------- | -------------------------------- | ------------ | -- | ---- |
| Responsibility                          | **Comes from product failure** | **Should Be** | Field Surveyor Onsite/Offline | Someone at Botjek Offsite/Online | 3D editor PH | QA | Mark |
| Fixing Walls/Window/Door/Floor Geometry | Yes                            |               |                               |                                  | X            | X  | X    |
| Fixing Areas                            | Yes                            |               |                               |                                  |              |    |      |
| Assign/Fix B-Factors                    | Partially                      |               | X                             |                                  | X            | X  | X    |
| Fixing Linear Thermal Loss              | Partially                      |               |                               |                                  |              |    |      |
| Assign Materials                        | Partially                      |               |                               |                                  |              |    |      |
| Change Element Type                     | Yes                            |               |                               |                                  |              |    |      |
| Change/Fix Heating                      | No                             |               |                               |                                  |              |    |      |
| Cha                                     |                                |               |                               |                                  |              |    |      |
|                                         |                                |               |                               |                                  |              |    |      |

| Task                            | **Why**                                                                                                                    | June | July | August |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ---- | ---- | ------ |
| Rearchitect web to "Task Based" | We need the surveyors to take responsibility for all non-geometric edits asap. If geometry disappears, this still happens. |      |      |        |
| Pavel                           |                                                                                                                            |      |      |        |
| Niels                           |                                                                                                                            |      |      |        |
| Wolthers                        |                                                                                                                            |      |      |        |
| Marina                          |                                                                                                                            |      |      |        |
| Kasper                          |                                                                                                                            |      |      |        |
| Martin                          |                                                                                                                            |      |      |        |
| Anders                          |                                                                                                                            |      |      |        |
| Mark                            |                                                                                                                            |      |      |        |

## Forms & Metadata Track

| Question | Answer |
| -------- | ------ |
|          |        |
|          |        |

#### Flexibility

1. Clients will not make their own reports
2. People without ability to install Claude Code will not edit reports

### Definition - Reports

The product need is:

1. FDEs can specify what datapoints are captured in a visit
2. FDEs can specify
3. When are those datapoints captured within the pre-selected

### Definition: Customizability

The product need is:

1. Form-based components can be created as a set of fields with types
2. Form-bas

### Suggested Priority

1. A current understanding of needed form objects, form variable types, and form UIs a) Tilstand & El (AV+Niels) b) French reports (Martin + Musketeers
2. A decision on form infrastructure (e.g. using spikes, with clear deadline)
3. Technical Installation Fields fully implemented using new form infrastructure by C
4. FDE for Tilstand, FDE for Electrical, FDE for 1 french report
