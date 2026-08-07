---
# automatic badge generation
lifecycle: alpha
last_updated: "2026-05-25"
---

# AI4SDLC Play: AI-SWEC Assessment Outline

*This document serves two purposes. Workgroup reviewers: read the main content; skip the indented Author notes blocks. Authors: use the Author notes blocks for citations, framework alignments, and authoring guidance when writing prose.*

Play series: AI4SDLC | Status: Draft | Target publish: Late 2026

> **Copyright notice:** The AI Software Engineering Evaluation Criteria (AI-SWEC) is MITRE intellectual property (IPDF'd). Copyright MITRE Corporation. All rights reserved. This play reproduces AI-SWEC question text with MITRE authorization for DoW practitioner use. Do not redistribute the question set outside this play without MITRE authorization.

---

## Author Notes Legend

*Workgroup reviewers: skip this section. Authors: these shorthand tags appear inside the indented Author notes blocks throughout the document.*

| Tag | What It Means |
| --- | --- |
| `[CDAO]` | Align to or cite CDAO Pathway to AI Readiness, DoD RAI Toolkit (SHIELD/DAGR), or related guidance |
| `[NIST]` | Align to or cite NIST AI RMF 1.0 (2023) or NIST.AI.600-1 GenAI Profile (2024) |
| `[AI4SDLC:play-name]` | Cross-reference the named AI4SDLC play in docs/plays/ |
| `[FIELD:Army]` | Pull detail from the Army SMDCOE early adopter story draft |
| `[FIELD:Navy]` | Pull detail from the Navy Forge early adopter story draft |
| `*agentic*` | Extend this content to address agentic AI and autonomous workflows |
| `[VERIFY]` | Confirm question text or ID against the current AI-SWEC Beta Decision Tool (Excel) before prose draft |

---

## What This Play Is

This play is the AI-SWEC question set — the full pre-event (B-xx) and post-event (P-xx) instrument — with guidance on how to conduct the assessment and use the responses to build an ADR.

Read the [Defining Your Why](defining_your_why_play-outline.md) play first. That play explains the Before/Measure/After arc, the Go/Slow/No-Go decision framework, and when and why to use AI-SWEC. This play is the instrument itself.

---

## EXECUTIVE SUMMARY

- AI-SWEC is a structured question set for evaluating AI adoption in the SDLC — before the pilot starts and after it closes
- It is not a scorecard and not a maturity model. It is a provocation: define your exploration with intention, then verify whether it worked
- The pre-event (B-xx) questions build the ADR; the post-event (P-xx) questions complete it
- Eleven questions are mirrored — assessed in both phases — creating the before/after evidence base
- A planned online tool will automate the walkthrough and generate the ADR; currently in Beta as an Excel instrument

> **Author notes — Executive Summary:**
>
> - MITRE source quote: "It's not a scorecard. It's not a maturity model. It's a provocation: define your exploration or experiment with intention." Use verbatim in prose.
> - Central question from MITRE source: "Is the juice worth the squeeze?" — use as a section anchor
> - Do NOT re-explain the Before/Measure/After arc, Go/Slow/No-Go, or adoption decision logic — that is [AI4SDLC:defining_your_why_play]
> - [NIST] GOVERN + MAP = pre-event phase; MEASURE + MANAGE = post-event phase

---

## SECTION 1 — AI-SWEC Origins and Design Intent

*What AI-SWEC is, where it came from, and why it was built this way.*

AI-SWEC was developed by MITRE's Software Engineering Innovation Center (SEIC) through the ArchAITecture Research Initiative — MITRE's Independent R&D program focused on AI/human teaming across the SDLC. Inputs came from the ArchAITecture Research Collaborative (A²RC), a not-for-profit collaborative of academia, FFRDC, and industry researchers.

The framework draws on MITRE's Calibrated Trust Framework, MITRE AI Maturity Model, MITRE RISK, MITRE ARCCS (AI Relevance Competence Cost Score), SPACE, and DORA. It is currently Beta — the question set is in Excel format and is still evolving. An online wizard/tool is planned to automate the walkthrough, visualize assessment results, and generate a jumpstart ADR.

**The framework is designed around four constraints:**

1. Simple enough for any team to execute without an external facilitator
2. Minimum viable questions — enough to answer "is the juice worth the squeeze?" without becoming a burden
3. Balanced — judgment-based questions (trust, complexity, urgency) alongside quantitative ones (DORA, code quality metrics)
4. Aligned — items map to DORA, SPACE, MITRE Calibrated Trust, and MITRE AMM, connecting to data teams may already collect

> **Author notes — Section 1:**
>
> - PI: Trac Bannon. Research Program Leader: Guido Zarrella. MITRE Fellow: Dr. Judith Dahmann. Expert inputs: Dr. Mike Hadjimichael, Dr. Bob Cherinka, Dr. Flo Reeder, Jeff Stanley.
> - "Still evolving" — frame as a strength, not a limitation: the framework improves as case study data accumulates. Call to action from MITRE: looking for engineers, tool teams, and policy thinkers to help shape its future.
> - Do not describe the tool as available — it is planned. Say: "currently available as an Excel instrument; an online tool is in development."
> - [CDAO] CDAO Pathway to AI Readiness is an organizational-level framework; AI-SWEC operationalizes the team-level assessment that precedes CDAO pathway navigation

---

## SECTION 2 — Purpose and Scope

*What this play covers — and what is covered in the Defining Your Why play.*

| Covered in This Play | Covered in Defining Your Why |
| --- | --- |
| Full pre-event (B-xx) question set | Before/Measure/After arc and adoption decision framework |
| Full post-event (P-xx) question set | Go/Slow/No-Go decision and rationale |
| Mirrored items — eleven questions assessed in both phases | ADR Jumpstart fields and initiation point |
| ADR field mapping from AI-SWEC responses | Phase sequencing, autonomy scoping, readiness prerequisites |
| Assessment facilitation guidance | Field examples (Army SMDCOE, Navy Forge) |
| Planned online tool description | DORA baselines and software quality metric guidance |

**Audience:** Software engineers, DevSecOps engineers, and technical leads running or facilitating a structured AI adoption evaluation. Secondary: program managers who need to understand what evidence the assessment produces.

> **Author notes — Section 2:**
>
> - This section should be very brief. Its only job is to establish what is unique to this play vs. what lives in Defining Your Why. Resist the urge to add more.
> - The "Covered in Defining Your Why" column is not a dismissal — it is a pointer. Make that clear in prose.

---

## SECTION 3 — Framework at a Glance

*The pre-event and post-event categories, and the eleven mirrored items that create the before/after comparison.*

**Pre-event (B-xx)** — conducted before the pilot begins. Six category groups:

| Category Group | Questions | Purpose |
| --- | --- | --- |
| Organizational Profile and Risk Appetite | B-11, B-01, B-02 | Industry context and disruption tolerance |
| GAI Strategy | B-04, B-03, B-09 | Strategic alignment and criticality |
| Team Operations and Context | B-07, B-12, B-17, B-18, B-10 | Architecture, DevSecOps maturity, practitioner trust |
| Current Performance Measures | B-13, B-14, B-15, B-16, B-33, B-34, B-35 | Delivery and quality baselines |
| Needs Definition — The Why | B-22, B-21, B-19, B-20, B-27, B-25 | Purpose, scope, urgency, affected personas |
| Value and Efficacy Targets | B-23, B-24, B-28 | Success criteria and thresholds |

**Post-event (P-xx)** — conducted after the pilot closes. Five category groups:

| Category Group | Questions | Purpose |
| --- | --- | --- |
| Solution Profile | P-27, P-16, P-17, P-12, P-13 | Tool fit, novelty, transparency, and performance |
| Risk Assessment | P-01, P-02, P-32, P-31 | Operational, technical, and compliance disruption |
| Measures and Benchmarks | P-33, P-34, P-35, P-4, P-5, P-6, P-7 | Shifts in quality and delivery performance |
| Outcomes, Value, and Efficacy | P-8, P-9, P-36, P-33b | Whether intended outcomes were achieved |
| Human Impacts | P-19, P-22, P-15, P-23, P-35, P-34 | Oversight, team dynamics, freed-up time |

**Mirrored items — assessed in both phases:**

| Pre-Event ID | Post-Event ID | Topic |
| --- | --- | --- |
| B-01 | P-01 | Operational Risk Appetite |
| B-02 | P-02 | Technical Risk Appetite |
| B-13 | P-4 | Deployment Frequency |
| B-14 | P-5 | Lead Time for Changes |
| B-15 | P-6 | Change Failure Rate |
| B-16 | P-7 | Mean Time to Recovery (MTTR) |
| B-33 | P-33 | Quantitative Code Quality Metrics |
| B-34 | P-34 | Qualitative Code Quality Measurements |
| B-35 | P-35 | Code Security Metrics |
| B-23 | P-8 | Specific Outcomes |
| B-24 | P-9 | Success Metrics / Measurable Impact |

> **Author notes — Section 3:**
>
> - [VERIFY] All question IDs must be verified against the current AI-SWEC Beta Decision Tool (Excel) — `AI-SWEC_Beta_DecisionTool.xlsx`
> - Write into prose for mirrored items: "These eleven questions are the before/after comparison engine. If the pre-event phase does not capture them, the post-event phase cannot produce the delta that makes the ADR defensible."
> - [NIST] AI RMF MAP: categorize AI use, identify context — pre-event categories operationalize this
> - Note that B-03 and B-09 are conditional on B-04 — if no GAI strategic plan exists, those questions adapt or are noted as not applicable

---

## SECTION 4 — Guiding Principles

*Four constraints that governed AI-SWEC's design — and why they matter for DoW practitioners.*

1. **Team-executable** — designed for a senior engineer or technical lead to facilitate without outside help; no specialist certification required
2. **Minimum viable** — the question set asks only what is necessary to answer the central question: "Is the juice worth the squeeze?"
3. **Balanced** — judgment questions (disruption tolerance, practitioner trust, complexity of need) alongside quantitative baselines (DORA, code quality, security metrics); both types are required for a complete picture
4. **Framework-aligned** — AI-SWEC items map to DORA, SPACE, MITRE Calibrated Trust, and MITRE AMM; teams with existing data from those frameworks can connect it directly

> **Author notes — Section 4:**
>
> - Keep this section very brief — one short paragraph plus the four items. No more.
> - [CDAO] Principle 1 (team-executable) aligns to DoD's guidance that responsible AI governance should be practicable at the program level, not just the enterprise level
> - Source: MITRE slide 9 (Guiding Principles). Translate into DoW-practitioner framing; do not copy verbatim.

---

## SECTION 5 — Pre-Event Assessment (B-xx)

*The full pre-event question set. Conducted before the pilot begins. All question text is from the AI-SWEC Beta Decision Tool. Copyright MITRE Corporation. All rights reserved.*

### 5.1 Organizational Profile and Risk Appetite

*Clarifies disruption tolerance and failure appetite. Anchors GenAI use to mission context.*

| ID | Topic | Question |
| --- | --- | --- |
| B-11 | Industry | What is the primary industry domain or sector for which the software is being developed? |
| B-01 ★ | Acceptable Level of Disruption | What level of operational disruption is acceptable during integration of the GAI-assisted tool into your SDLC? |
| B-02 ★ | Tolerance to Technical Failure | How tolerant is the team to potential technical disruptions or failures integrating GAI tools into your SDLC and the resulting outputs? |

★ = mirrored — re-assessed post-event (P-01, P-02)

### 5.2 GAI Strategy

*Determines whether a strategic plan exists and anchors the evaluation to organizational priorities.*

| ID | Topic | Question |
| --- | --- | --- |
| B-04 | AI Strategic Plan | Does your organization have a strategic plan for GAI integration into software engineering processes? |
| B-03 | Criticality to Strategic Goals | *Conditional on B-04:* How critical is the GAI tool integration to achieving strategic goals? |
| B-09 | Strategic Alignment | *Conditional on B-04:* Which strategic goal does the integration of GAI-assisted software tools support at your organization? |

### 5.3 Team Operations and Context

*Reveals team readiness based on architecture, DevSecOps maturity, SDLC practices, and practitioner trust.*

| ID | Topic | Question |
| --- | --- | --- |
| B-07 | GAI Platforms or Tools | Does your organization have dedicated GAI platforms and tools specifically for software engineering tasks? |
| B-12 | Architecture Type | What type of architecture does the software follow? |
| B-17 | SDLC Methodology | Which software development methodologies does your team primarily use? |
| B-18 | Usage of DevOps Principles | Which DevSecOps principles has your team incorporated into your SDLC? (Select all that apply) |
| B-10 | Practitioner Trust | What measures are in place to build and maintain team member trust in GAI tools used for software engineering? (Select all that apply) |

### 5.4 Current Performance Measures

*The most critical step — and the most frequently skipped. Establishes the baseline without which no before/after comparison is possible.*

DORA — Team Process Metrics:

| ID | Topic | Question |
| --- | --- | --- |
| B-13 ★ | Deployment Frequency | How often does your team deploy code to production? |
| B-14 ★ | Lead Time for Changes | What is the typical lead time for changes from code commit to production deployment? |
| B-15 ★ | Change Failure Rate | What percentage of changes result in a failure in production that requires immediate remediation? |
| B-16 ★ | Mean Time to Recovery | What is the average time it takes to recover from a failure in production? |

Software Quality Metrics:

| ID | Topic | Question |
| --- | --- | --- |
| B-33 ★ | Quantitative Code Quality | Does your team currently track quantitative software code quality metrics? |
| B-34 ★ | Qualitative Code Quality | Does your team track or evaluate the following qualitative code quality characteristics? (Select all that apply) |
| B-35 ★ | Code Security Metrics | Does your team track the following code security metrics? (Select all that apply) |

★ = mirrored — re-assessed post-event

### 5.5 Needs Definition — The Why

*Forces clarity on purpose, urgency, scope, and which personas benefit. Links tool use to specific SDLC pain points.*

| ID | Topic | Question |
| --- | --- | --- |
| B-22 | Classification of Usage Motivation | Why do you want to use this AI-assisted software engineering tool? |
| B-21 | Team Persona Impact | Which people and roles within your software engineering team are most impacted by the adoption of this GAI-assisted tool? |
| B-19 | Complexity of the Need | How complex is the business or mission need this GAI-assisted tool aims to address? |
| B-20 | Timeliness of the Need | How urgent is the business or mission need for this GAI-assisted tool? |
| B-27 | SDLC Phase | Which phase of the SDLC are you targeting? |
| B-25 | Enhanced Capabilities | What specific tasks or capabilities are you aiming to enhance with GAI-assisted tools in the SDLC? |

### 5.6 Value and Efficacy Targets

*Defines success before the experiment begins. Prevents anecdotal evaluations.*

| ID | Topic | Question |
| --- | --- | --- |
| B-23 ★ | Outcome-Based Objectives | What specific outcomes are you aiming to achieve with the integration of AI tools? |
| B-24 ★ | Measurable Impact Metrics | What measurements or metrics will you use to indicate the success of the GAI tool integration? |
| B-28 | Target Success Threshold | For each improvement you are seeking, what is the threshold for considering the GAI-assisted tool adoption successful? |

★ = mirrored — re-assessed post-event (P-8, P-9)

> **Author notes — Section 5:**
>
> - [VERIFY] All question text against AI-SWEC Beta Decision Tool (Excel) before prose draft
> - [CDAO] Risk Management: B-01, B-02, B-03 map to CDAO risk appetite assessment
> - [NIST] AI RMF MAP: B-22, B-21, B-27, B-25 operationalize the MAP function
> - Write for 5.4: "If your team cannot answer the DORA questions, record that. 'We do not track deployment frequency' is a pre-event finding — not a blocker. It becomes the first post-event measurement opportunity." [FIELD:Army] Army SMDCOE did not capture all baselines before adoption — acknowledged in early adopter story.
> - *agentic* additions to 5.3 Team Operations: for agentic pilots, add to B-18 context — prompt and skills file version control is a DevSecOps practice. For B-10 Practitioner Trust: add shadow agent risk and agent approval process as trust-building measures to note. [AI4SDLC:ai-autonomy-implementation-guide]
> - *agentic* note for 5.5: after B-27 (SDLC Phase), add: if the targeted phase involves autonomous or semi-autonomous workflows, flag for Pattern 3/4 readiness check in [AI4SDLC:ai-autonomy_continuum_play]

---

## SECTION 6 — Post-Event Assessment (P-xx)

*The full post-event question set. Conducted after the pilot closes. All question text is from the AI-SWEC Beta Decision Tool. Copyright MITRE Corporation. All rights reserved.*

### 6.1 Solution Profile

*Assesses how well the GenAI tool fit the real-world need. Surfaces issues with transparency, novelty, or trust.*

| ID | Topic | Question |
| --- | --- | --- |
| P-27 | Type of AI-Assisted Solution | Which type of GAI-assisted tools did you leverage for this effort? (Select all that apply — model hosting and integration/access) |
| P-16 | Tech Maturity | How novel is the approach and supporting technology for this GAI-assisted tool? |
| P-17 | Transparency and Explainability | How transparent and explainable was the GAI-assisted tool's decision-making process? |
| P-12 | Relevance / Goodness of Fit | How appropriate was the GAI-assistance in solving the targeted software engineering problem? |
| P-13 | Competence: Needs Alignment | How well did the GAI-assisted tool meet the team's project requirements and performance expectations? |

### 6.2 Risk Assessment

*Identifies operational, technical, and compliance disruptions introduced by the pilot.*

| ID | Topic | Question |
| --- | --- | --- |
| P-01 ★ | Operational Risk | What level of operational disruption did you experience during AI tool integration? |
| P-02 ★ | Technical Risk | How tolerant was the team to potential failures in AI tool outputs during the experiment? |
| P-32 | Policy Compliance | Did the use of this GAI-assisted tool violate or challenge any policies and procedures? |
| P-31 | Security Vulnerabilities | Did the AI-assisted tool introduce any new security vulnerabilities during the project? |

★ = mirrored — compare to B-01 and B-02 pre-event responses

### 6.3 Measures and Benchmarks

*Captures observable shifts in quality and delivery. Anchors the before/after story in concrete metrics.*

Software Quality Metrics:

| ID | Topic | Question |
| --- | --- | --- |
| P-33 ★ | Quantitative Code Quality | If your team tracks software code quality metrics, did you experience changes, whether positive or negative? |
| P-34 ★ | Qualitative Code Quality | If your team tracks qualitative code quality characteristics, was there any change, whether positive or negative? (Select all that apply) |
| P-35 ★ | Code Security Metrics | If your team tracks code security metrics, was there any change, whether positive or negative? (Select all that apply) |

DORA — Team Process Metrics:

| ID | Topic | Question |
| --- | --- | --- |
| P-4 ★ | Deployment Frequency | How often did your team deploy code to production during the experiment? |
| P-5 ★ | Lead Time for Changes | What was the typical lead time for changes from code commit to production deployment during the experiment? |
| P-6 ★ | Change Failure Rate | What percentage of changes resulted in a failure in production that required immediate remediation during the experiment? |
| P-7 ★ | Mean Time to Recovery | What was the average time it took to recover from a failure in production during the experiment? |

★ = mirrored — compare directly to pre-event responses

### 6.4 Outcomes, Value, and Efficacy

*Confirms whether intended outcomes were achieved. Surfaces hidden barriers or unexpected friction.*

| ID | Topic | Question |
| --- | --- | --- |
| P-8 ★ | Specific Outcomes | What specific outcomes did you achieve with the integration of GAI-assistive tools for software engineering and the SDLC? |
| P-36 | Success Metrics Achieved | Were you able to improve the measurements and metrics that you indicated would represent success? If yes, which ones? |
| P-9 ★ | Success Metrics — Which Indicated Success | Which of your planned measurements indicated the success of the integration of GAI-assistance into your SDLC? |
| P-33b | Barriers to Adoption | Did you encounter any other challenges when adopting GAI-assisted tools for use in the SDLC? |

*★ = mirrored — compare to B-23 and B-24 pre-event responses*

### 6.5 Human Impacts

*Examines how GenAI shifted roles, oversight, and team dynamics. Surfaces whether time freed up was reinvested in quality and learning.*

| ID | Topic | Question |
| --- | --- | --- |
| P-19 | Human Oversight Requirement | Did the GAI-assisted tool require additional human oversight compared to traditional methods? |
| P-22 | Quality Improvement | How did the quality of outputs produced with the GAI-assisted tool compare to those produced by human-only methods? |
| P-15 | Task Handling | How well did the GAI-assisted tool handle tasks compared to human-only methods? |
| P-23 | Impact on Communication and Workflow | How has GenAI affected communication within your team about software development lifecycle topics? |
| P-35 | Training Required | Did the use of this tool require specialized training? |
| P-34 | Freed-Up Time Reinvestment | As you have begun using GAI-assisted tools for the SDLC, have you gained back any time? If so, how are you investing that freed-up time? |

> **Author notes — Section 6:**
>
> - [VERIFY] All question text against AI-SWEC Beta Decision Tool (Excel) before prose draft
> - [NIST] AI RMF MEASURE + MANAGE underlie the post-event phase
> - Write for 6.2: "If P-31 or P-32 surfaces a 'yes,' the result goes directly into the ADR's Risk section and into the team's security posture review. A pre-event No-Go on Policy or Security should have prevented reaching this question — a surprise here signals a gap in the pre-event assessment."
> - [FIELD:Army] P-19 (Human Oversight): Army's accountability posture — "I don't care if Claude wrote it. You are responsible for that." — is a governance answer to this question. Oversight was not additional burden; governance posture provided it.
> - [FIELD:Navy] P-34 (Freed-Up Time): Navy Forge measured time savings per use case but did not formally track reinvestment. Note as an open learning, not a critique.
> - *agentic* additions for 6.2 Risk Assessment: for agentic pilots, P-32 should specifically address skills file governance, agent registration, and shadow agent activity. P-31 should address prompt injection and skills file manipulation as security risk categories. [AI4SDLC:ai-autonomy_continuum_play]
> - *agentic* additions for 6.5 Human Impacts: for agentic pilots, P-19 "additional human oversight" manifests as: monitoring agent action logs, reviewing escalation triggers, auditing skills file changes. [AI4SDLC:ai-autonomy-implementation-guide]

---

## SECTION 7 — ADR Mapping

*How AI-SWEC responses populate the ADR. The B-xx questions build it; the P-xx questions complete it.*

The ADR is initiated at the Go/Slow/No-Go decision point — before the pilot begins. See the [Defining Your Why](defining_your_why_play-outline.md) play, Section 6.1 for the ADR Jumpstart fields and initiation logic. This section shows which AI-SWEC questions feed which ADR fields.

| ADR Field | Pre-Event Source (B-xx) | Post-Event Source (P-xx) |
| --- | --- | --- |
| Context | B-11, B-01, B-02, B-04, B-12, B-17, B-18 | — |
| Decision | B-22, B-27, B-25, B-21 | — |
| Alternatives Considered | B-19, B-20, B-03, B-09 | — |
| Consequences (expected) | B-23, B-24, B-28 | — |
| Pre-Pilot Baselines | B-13, B-14, B-15, B-16, B-33, B-34, B-35 | — |
| Post-Pilot Outcomes | — | P-8, P-9, P-36, P-33b |
| Risk Delta | — | P-01, P-02, P-31, P-32 |
| Measures Delta | — | P-4, P-5, P-6, P-7, P-33, P-34, P-35 |
| Human Impact | — | P-19, P-22, P-15, P-23, P-34, P-35 |
| Solution Fit Assessment | — | P-27, P-12, P-13, P-16, P-17 |

See the [ADR template](../resources/ArchitecturalDecisionRecord.md) in docs/resources.

> **Author notes — Section 7:**
>
> - MITRE source framing: "Your experiment becomes a recorded justification — not just a memory." Use in prose.
> - MITRE source: "Intentionality lives in the ADR — clear context, traceable logic, justifiable risk." Use in prose.
> - Cross-verify ADR field names against the template in docs/resources/ before prose draft — confirm field names match
> - [CDAO] DoD RAI S&I Pathway: defensible decision making requires documented context, not compliance checkboxes
> - The online tool (in development) will automate this mapping. Describe the vision: assessment responses feed directly into an ADR template with pre/post delta visualizations. Do not imply the tool is available.

---

## SECTION 8 — Common Assessment Pitfalls

*What goes wrong when teams shortcut the assessment. Brief — not an adoption decision checklist (that is in Defining Your Why).*

- **Skipping baseline capture** — the most common failure; without B-13 through B-35, the mirrored items produce no delta and the ADR has no evidence base
- **Answering for the organization instead of the use case** — AI-SWEC questions are scoped to a specific use case, not the whole team or program; answers for "the org" are too vague to build a defensible ADR
- **Treating the B-04 conditional questions as optional** — if no strategic plan exists, the absence is itself a finding; document it rather than skipping B-03 and B-09
- **Running the post-event phase immediately after the pilot closes** — allow time to observe actual delivery metrics before answering P-4 through P-7; sentiment collected at pilot close is not the same as a DORA measurement
- **Confusing the assessment with a compliance checklist** — AI-SWEC is a structured evidence-gathering tool; it does not produce a pass/fail verdict; the team interprets the responses and makes the decision

> **Author notes — Section 8:**
>
> - [FIELD:Army] Pitfall 1 (skipping baselines): Army SMDCOE acknowledged missing several metrics — specific gaps documented in early adopter story
> - [FIELD:Navy] Pitfall 4 (timing): Navy Forge captured time savings per use case in real time; the evaluation period had a defined close date. Model for how to time the post-event phase.
> - *agentic* addition: for agentic pilots — pitfall is treating P-31/P-32 as optional because "we're just testing." Any agentic pattern above P1 that touches production pipelines requires these questions to be completed before pilot close, not after.

---

## SECTION 9 — Key Takeaways

1. **The question is the work.** AI-SWEC's value is in forcing specific answers before and after the pilot. There is no score to optimize.
2. **"We don't track that" is a valid pre-event answer — and a finding.** Missing baselines are documented gaps, not blockers. The post-event phase measures whether that changed.
3. **The eleven mirrored items are the evidence engine.** Pre-event responses without post-event follow-through produce an incomplete ADR.
4. **B-03 and B-09 are conditional on B-04.** If no GAI strategic plan exists, note that explicitly — it affects how the ADR frames strategic alignment.
5. **The ADR does not wait for the pilot to close.** The pre-event questions build the ADR. The post-event questions complete it. See Defining Your Why, Section 6.1.
6. **P-31 and P-32 are not optional.** Compliance and security questions surface risk that the pre-event assessment may not have fully anticipated.
7. **The framework is still evolving.** Verify all question IDs and text against the current Excel before prose. The August 2025 PDF is the source for this outline; the Excel is authoritative.

> **Author notes — Section 9:**
>
> - Takeaway 5: [AI4SDLC:defining_your_why_play] Section 6.1 — link only, do not duplicate
> - Takeaway 7: Frame as a feature, not a limitation. MITRE: "We're looking for engineers, tool teams, and policy thinkers to help shape its future." DoW teams using this play are contributing to that data set.

---

## SECTION 10 — Companion Plays and References

**Related Plays:**

- [Defining Your Why](defining_your_why_play-outline.md) — read first; the adoption decision framework, Before/Measure/After arc, Go/Slow/No-Go, and ADR Jumpstart
- [AI Autonomy Continuum](ai-autonomy_continuum_play.md) — pattern selection; agentic governance readiness; P1-P4 patterns
- [AI Autonomy Implementation Guide](ai-autonomy-implementation-guide.md) — pattern-specific readiness checklists
- [Fundamentals for Designing an AI-Augmented Tool Chain](fundamentals-play.md) — hosting models, tool selection
- [AI Workflow Design and Governance](ai_sdlc_workflows-play.md) — workflow governance, skills file governance

**Key References:**

- MITRE AI-SWEC Beta Decision Tool (Excel) — authoritative source for all question IDs and text *(MITRE IP; distribution by MITRE authorization)*
- MITRE POV: *Healthy Software, Lasting Value — Engineering the AI-Augmented Software Lifecycle* (May 2025)
- ArchAITecture Research Collaborative (A²RC) — research origin
- MITRE Calibrated Trust Framework | MITRE AI Maturity Model (2023) | MITRE ARCCS Framework
- DORA 2025 State of AI-Assisted Software Development | SPACE Framework
- CDAO Pathway to AI Readiness — ai.mil
- NIST AI RMF 1.0 (2023) | NIST.AI.600-1 GenAI Profile (2024)

---

## Architectural Intent

This play is the structured assessment instrument behind the AI4SDLC adoption decision cycle. The pre-event question set (B-xx) builds the evidence base that initiates the ADR. The post-event question set (P-xx) measures the delta and completes the record. Together they make every AI adoption decision in the SDLC traceable, justifiable, and repeatable. The Defining Your Why play is the decision framework. This play is the instrument.

---

## DISCUSSION POINTS FOR THE WORKGROUP

1. **Copyright and reproduction authorization:** AI-SWEC is MITRE IP. This play reproduces the full question set with attribution. Confirm that MITRE authorization covers reproduction in this play format before publication.

2. **Question set currency:** Question text in this outline is sourced from the August 2025 MITRE PDF. The Excel Beta Decision Tool may contain refinements. A verification pass is required before prose draft. Workgroup members with current Excel access should flag discrepancies.

3. **DoW-specific framing:** Some questions use general enterprise language (B-04, B-11). Brief DoW-specific context notes may be needed — e.g., what serves as a "GAI strategic plan" in a program-level context without a formal organizational plan?

4. **Online tool timeline and scope:** MITRE is developing an online wizard. The AI4SDLC Working Group should clarify whether DoW intends to adopt that tool directly or build a DoW-specific implementation. This play currently describes the tool as "in development."

5. **Agentic extensions:** The agentic author notes in Sections 5 and 6 flag potential additions to the B-xx and P-xx question sets for agentic pilots. Workgroup input needed: should these be DoW-specific supplement questions, or should they be proposed back to MITRE for incorporation into the framework?

---

## AUTHORING NOTES

- **Copyright:** Copyright block is at the top of the document and on Sections 5 and 6 headers. Do not remove or weaken these notices in prose.
- **Verification required:** All B-xx and P-xx question IDs and text must be verified against `AI-SWEC_Beta_DecisionTool.xlsx` in `local_working_files/defining_why/` before prose draft. The August 2025 PDF is the source for this outline; the Excel is authoritative.
- **No duplication with Defining Your Why.** This play does not explain the Before/Measure/After arc, Go/Slow/No-Go logic, adoption prerequisites, DORA metrics, field examples, or the ADR initiation point. Reference and link to the Defining Your Why play for all of that.
- **Link paths:** This file now lives in `docs/plays/`, and its links are set for that location. Relative links to sibling plays are bare filenames; links to `docs/resources/` are prefixed `../resources/`.
- **No score, no maturity model.** Never frame sections as scoring. Never imply a threshold that constitutes passing.
- **Online tool:** Describe as "in development" with intended capability. Do not imply availability.
- **Framework evolution:** AI-SWEC is Beta as of August 2025. State that the question set may be refined.
- CDAO realigned under USD(R&E) August 2025 — frameworks remain in force; note in footnote if mentioning CDAO organizational structure.
- Voice: No em-dashes. No "ensure." No "leverage." No "transformative." Specific over vague. Direct over hedged.

---

*AI4SDLC Play Series — Department of War CIO Guidance. Draft. Not for distribution beyond the AI4SDLC Working Group.*
