---
title: "Plan Phase Documentation"
parent: "documentation-play.md"
lifecycle: beta
last_updated: "2026-01-26"
---

# Plan Phase Documentation

[← Back to Documentation Play](../documentation-play.md)

---

## What Happens in This Phase

The Plan phase establishes the strategic foundation for software delivery. Documentation created here—Capability Needs Statements, acquisition strategies, test strategies—drives funding, architecture, and acquisition decisions. AI serves as an augmentation partner, helping structure arguments and identify gaps, but humans own the strategic decisions.

---

## Where AI Helps

- **Structuring CNS narrative** from capability descriptions and operational context
- **Drafting test strategy sections** based on system architecture and security requirements
- **Structuring cost and schedule narratives** from provided estimates and historical program data
- **Consistency checking** across related planning documents
- **Gap identification** in template coverage and missing sections

---

## Example: CNS Capability Gap Section

### Context Setup

Before prompting, the user provided:

- DoD Adaptive Acquisition Framework CNS template
- Mission description and operational environment details
- Current capability assessment (manual planning process analysis)
- Target system requirements (IL5, DCGS integration)
- Performance targets (planning time reduction goals)

### Prompt

> *"I'm creating a Capability Needs Statement for a mission planning system that automates target analysis and route planning. The capability gap is a 6-8 hour manual planning cycle that introduces errors—we need to reduce to <1 hour with automated deconfliction. The system must operate at IL5 and integrate with DCGS intelligence feeds. Using the DoD Adaptive Acquisition Framework CNS template, help me draft the operational capability gap and proposed solution sections."*

### Output

```
Operational Capability Gap:
Current mission planning relies on manual processes requiring 6-8 hours per
mission, introducing risk of human error in target analysis and route
deconfliction. Planners manually correlate intelligence from multiple
classified sources, transcribe coordinates, and visually identify conflicts
with other operations.

Proposed Solution:
An automated mission planning system that integrates intelligence feeds,
applies AI-assisted target analysis, and automates route deconfliction.
The system will reduce planning time to <1 hour while improving accuracy
through automated cross-checking. Key capabilities include:
- Real-time intelligence feed integration from DCGS
- Automated airspace deconfliction with existing flight schedules
- Route optimization considering threat environments and fuel constraints
- Audit trail for all planning decisions supporting mission review
```

---

## What AI-Generated Plan Documentation Often Misses

- **Mission context and operational nuance** - Generic language that doesn't resonate with warfighters
- **Political and organizational dynamics** - Stakeholder concerns, funding competition, cross-command dependencies
- **Classification implications** - Combining information that raises classification level
- **Realistic cost and schedule factors** - Optimistic estimates without historical grounding
- **Acquisition regulation compliance** - FAR/DFARS nuances requiring legal review
- **Incomplete legacy discovery** - Undocumented tribal knowledge that AI cannot invent

**Human reviewers must validate strategic alignment, operational authenticity, and compliance.**

---

## Governance Checklist

Before accepting AI-assisted Plan phase documentation:

- [ ] Classification markings accurate throughout
- [ ] Mission impact articulated with operational scenarios
- [ ] Proposed solution technically feasible
- [ ] Cost and timeline validated against similar programs
- [ ] Stakeholders identified and concurred
- [ ] Assumptions and risks explicitly stated

---

## Brownfield Additions

For modernization efforts, Plan phase requires additional focus:

- **Migration Strategy** - Phased approach with dependency mapping, data migration criteria, and rollback provisions
- **Legacy Interface Inventory** - AI can help reverse-engineer from code and configs; SMEs validate completeness
- **Discovery Sprints** - Budget time for AI-assisted analysis validated by subject matter expert interviews

---

[← Back to Documentation Play](../documentation-play.md)
