---
# automatic badge generation
lifecycle: alpha
last_updated: "2026-05-20"
---

# AI4SDLC Play: Defining Your Why Outline

*This document serves two purposes. Workgroup reviewers: read the main content; skip the indented Author notes blocks. Authors: use the Author notes blocks for citations, framework alignments, field example detail, and agentic extensions when writing prose.*

Play series: AI4SDLC | Status: Draft | Target publish: Mid-2026

---

## Author Notes Legend

*Workgroup reviewers: skip this section. Authors: these shorthand tags appear inside the indented Author notes blocks throughout the document. They are prompts for citations, cross-references, and content extensions — not part of the play text itself.*

| Tag | What It Means |
| --- | --- |
| `[CDAO]` | Align to or cite the CDAO Pathway to AI Readiness, the DoD Responsible AI Toolkit (SHIELD/DAGR), or related CDAO guidance |
| `[NIST]` | Align to or cite NIST AI Risk Management Framework 1.0 (2023) or NIST.AI.600-1 GenAI Profile (2024) |
| `[MITRE AMM]` | Reference the MITRE AI Maturity Model — as a job aid diagnostic only, not as the core framework |
| `[AI4SDLC:play-name]` | Cross-reference the named AI4SDLC play in docs/plays/ — link to it in prose |
| `[FIELD:Army]` | Pull detail from the Army SMDCOE early adopter story draft |
| `[FIELD:Navy]` | Pull detail from the Navy Forge early adopter story draft |
| `*agentic*` | Extend this content to address agentic AI and autonomous workflows — woven into existing risk framing, not a separate section |

---

## What This Play Is

Most AI4SDLC plays assume the adoption decision is already made — and treat every expansion decision the same way. This play is the cycle that comes before each of those decisions.

The play follows a three-part arc built on the MITRE AI Software Engineering Evaluation Criteria (AI-SWEC) framework:

```text
BEFORE              →    MEASURE             →    AFTER
Define the why           Capture baselines        Run the pilot
Assess readiness         Scope the pilot          Evaluate results
Decide Go/Slow/No-Go     Set success threshold    Produce the ADR
```

**The arc repeats.** Two triggers re-enter the cycle: adopting something new (new tool, new SDLC phase, new autonomy level), or revisiting a prior decision to pause or not adopt. A "No-Go" is not permanent — conditions change, tooling matures, governance catches up. When it does, start the cycle again.

**The output of this play is a deliberate adoption decision** — to continue, adjust, or pause — made with evidence and documented in an [Architectural Decision Record (ADR)](../resources/ArchitecturalDecisionRecord.md). The goal is the quality of the decision and the evidence behind it. The ADR is how that decision becomes traceable, justifiable, and defensible.

---

## EXECUTIVE SUMMARY

*This section frames the problem and states the arc, the output, and the core framework.*

Key claims:

- Mandated AI adoption without defined intent produces blind adoption — inconsistent, untrustworthy, and sometimes costly
- The play is a repeating discipline — applied at first adoption and again at every tool change, phase expansion, or autonomy level shift
- The primary output is a deliberate adoption decision — to continue, adjust, or pause — documented in an ADR. The goal is the decision and the evidence behind it, not the document itself.
- MITRE AI-SWEC is the core framework: two structured question sets (B-xx before the pilot; P-xx after) that produce the evidence this play uses to build the ADR. It is not a maturity model. Explained briefly in this section.

> **Author notes — Executive Summary:**
>
> - [CDAO] CDAO Pathway to AI Readiness and NIST AI RMF briefly referenced as governing frameworks; this play operationalizes the "why" question that precedes CDAO pathway navigation
> - [NIST] NIST AI RMF GOVERN + MAP underlie the Before phase; MEASURE + MANAGE underlie the After phase
> - AI-SWEC explainer block (write into prose): "B-xx questions define intent before the pilot starts; P-xx questions assess outcomes after the pilot closes. Those answers are the structured evidence that builds the ADR. If your team has not encountered AI-SWEC before, this play is the entry point."
> - Audience note: primary = practitioners and technical leads; secondary = executives and acquisition stakeholders (Sections 6 and 8 most relevant to them)
> - **MITRE AI-SWEC copyright:** On first use of "MITRE AI-SWEC" in prose, add a footnote: "MITRE AI Software Engineering Evaluation Criteria (AI-SWEC). Copyright MITRE Corporation. All rights reserved." Use IEEE-style footnote format consistent with other plays.
> - Repeating cycle framing is the distinctive claim of this play vs. other readiness frameworks — every section should reinforce it, not compete with it

---

## SECTION 1 — Why This Play Matters

*The cost of "just use it," why intentionality is the differentiator, and where most DoW teams sit on the adoption curve.*

Three failure modes from mandated-but-unintentional adoption:

1. Unused tooling — investment waste with no measurable outcome
2. Wrong phase adoption — AI introduced in high-risk phases before governance exists
3. Quality and security regression — speed-quality inversion; faster delivery of worse output

Gartner (2024): AI-augmented software engineering teams take 2–5 years to reach the productivity plateau. Teams that sprint into adoption without readiness spend those years recovering, not accelerating.

Teams that define the why first succeed more often:

- *Army SMDCOE:* Six-person team. Pain point: unit testing and security scanning pushed right in the sprint. Targeted one tool for those two tasks specifically. Scoped tightly. Expanded only after trust was established.
- *Navy Forge:* Built an evaluation around nine use cases relevant to the program's own release cycle. For each use case, defined success criteria before testing. Measured time savings only when quality criteria were met.

**Where most DoW teams sit on the Technology Adoption Life Cycle (TALC):**

| TALC Segment | DoW Parallel | What They Need From This Play |
| --- | --- | --- |
| Innovators / Early Adopters | Already experimenting | Structured evidence to document what is working |
| Early Majority — Pragmatists | Primary audience | ADR and baselines as proof before committing further |
| Late Majority | Adopting under mandate | Governance infrastructure, not just tools |
| Skeptics / Constrained Programs | Adoption when directed or constraints resolve | Not the conversion target; governance built for the majority serves them when their time comes |

> **Author notes — Section 1:**
>
> - Geoffrey Moore, *Crossing the Chasm* (1991) — TALC framing. Most DoW delivery teams sit in the Early Majority. Pragmatists cross the chasm on evidence from comparable contexts, not on hype. The ADR this play produces is exactly that evidence.
> - "Skeptics" is Moore's preferred alternative to Rogers' "Laggards." For DoW contexts, add parenthetical: *(often constraint-driven — policy, accreditation, or mission risk — rather than posture-driven)*
> - GitClear 2025: code churn and duplication trending up alongside AI adoption in large repos [AI4SDLC:ai_sdlc_workflows_play]
> - DORA 2025 "State of AI-Assisted Software Development": highest-performing teams measured before they adopted [AI4SDLC:fundamentals-play]
> - [CDAO] DoD RAI S&I Pathway, p. 9 — "Overview Depicting RAI's Journey to Trust" — frame the why as the first step in the trust arc, not a compliance exercise
> - Two audiences framing: the practitioner author of the why (specific pain point, specific phase) vs. the executive audience of the why (strategic alignment, mission readiness, acquisition defensibility) — address both in prose but keep primary focus on the practitioner
> - *agentic* extension: same "just use it" pressure now applies to agentic workflows at proportionally higher risk. Pattern 3/4 workflows deployed without Pattern 1/2 governance in place create systemic risk: cascading errors, shadow agent activity, compliance gaps. Key line to write: "The same discipline that applies to adopting a code assistant applies to deploying an autonomous pipeline — and the cost of skipping it scales with the autonomy level." [AI4SDLC:ai-autonomy_continuum_play]
> - [FIELD:Army] additional: phased rollout gave time to build shared context files before usage fragmented across individuals — not accidental, deliberate governance choice
> - [FIELD:Navy] additional: evaluation produced concrete data before any tool selection decision; tool selection must still clear hosting and security approval for the mission's impact level

---

## SECTION 2 — Purpose and Scope

*What this play covers and what it explicitly does not — organized by arc phase.*

| Covered in This Play | Covered Elsewhere |
| --- | --- |
| Readiness assessment across four dimensions | Tool selection and procurement → AI4SDLC Fundamentals play |
| SDLC phase targeting and sequencing | Post-adoption model governance → AI4SDLC Workflow Design and Governance play |
| Autonomy pattern scoping | Agentic pipeline architecture → AI4SDLC Autonomy Continuum play and Implementation Guide |
| Go/Slow/No-Go decision | AI policy development → DoD/DoW AI policy guidance |
| Baseline metric capture | Acquisition strategy for AI tools → Defense acquisition pathway and DAU guidance |
| Adoption health composite evaluation | Skills file architecture → AI4SDLC Agentic Workflow Design play (forthcoming) |
| Deliberate adoption decision, documented as an ADR | |

**Audience note:** Primary — software engineers, DevSecOps engineers, technical leads, product owners. Secondary — executives and acquisition stakeholders (Sections 6 and 8 most relevant to them).

> **Author notes — Section 2:**
>
> - [NIST] GOVERN + MAP = Before phase; MEASURE + MANAGE = After phase
> - Out-of-scope cross-references:
>   - Post-adoption model governance → [AI4SDLC:ai_sdlc_workflows_play]
>   - Agentic pipeline architecture → [AI4SDLC:ai-autonomy_continuum_play] + [AI4SDLC:ai-autonomy-implementation-guide]
>   - Tool selection and hosting models → [AI4SDLC:fundamentals-play]
>   - Skills file architecture → [AI4SDLC:ai_sdlc_workflows_play]

---

## SECTION 3 — Prerequisites and Readiness

*Four dimensions — gaps are documented and addressed, not used to block all adoption. Teams with gaps should target lower-autonomy, lower-risk phases first.*

| Dimension | Key Checks |
| --- | --- |
| **DevSecOps** | CI/CD established and used; automated testing in at least one phase; source control with MR/PR workflows; IaC practiced; secrets management via vault; vulnerability scanning (SAST/DAST/SCA) |
| **Governance** | GAI strategic plan (even if in development); disruption tolerance defined; data classification rules for AI inputs known; IL-level alignment confirmed; policy review completed; ADR practice exists or adoptable |
| **Team** | SDLC methodology documented; DORA baselines capturable; trust-building measures in place or planned; AI limitations and failure modes discussed; training plan exists |
| **Environment** | Architecture type documented; GAI platform access identified (SaaS, self-hosted, IDE plugin); security constraints understood; data handling for sensitive code defined |

> **Author notes — Section 3:**
>
> - [CDAO] Pathway to AI Readiness maps to these four dimensions: Responsible AI + Risk Management → Governance; Data Management → Environment; Workforce Talent + Training → Team; Oversight → cross-cutting
> - [MITRE AMM] MITRE AI Maturity Model — job aid diagnostic only, NOT the core framework. Strategy and Resources pillar = Governance readiness. Technology Enablers = DevSecOps + Environment. Workforce Development = Team. Use for "where are we now?" assessment, not as a scoring target.
> - *agentic* additions to DevSecOps: prompt and skills file version control (same rigor as source code); agent registration process exists or adoptable; rollback mechanisms can revert agent-generated changes [AI4SDLC:ai-autonomy-implementation-guide]
> - *agentic* additions to Governance: skills file governance policy exists (reviewed, version-controlled, audited); agent approval process defined; escalation rules exist for when agents exceed intended scope [AI4SDLC:ai_sdlc_workflows_play]
> - *agentic* additions to Team: team understands difference between assistive (P1/P2) and orchestrated (P3/P4) patterns; team has discussed agentic failure modes — prompt injection, skills file manipulation, shadow agent risk [AI4SDLC:ai-autonomy_continuum_play]
> - *agentic* additions to Environment: sandbox capability separate from production pipelines; network policies govern outbound traffic to AI services [FIELD:Army] Army used Bedrock GovCloud specifically to keep code inside the GovCloud boundary

---

## SECTION 4 — Guiding Principles and Guardrails

*Seven non-negotiable starting points. No action under this play should contradict them.*

1. **Human Oversight** — AI outputs must be validated by a human before integration into any artifact. No autonomous action without a defined approval gate.
2. **Intentionality Before Tooling** — No AI tool introduced without a defined why, targeted phase, success threshold, and pre-capture baseline.
3. **Trust Calibration** — Reliance must match demonstrated capability in context. Trust is earned incrementally and revisited.
4. **Model Boundary Awareness** — All team members must understand what the model can and cannot do. Overconfidence is a leading cause of defect introduction and security regression.
5. **Agent Auditability** — Any autonomous or semi-autonomous AI action must be traceable and reversible. If you cannot audit what an agent did, you cannot trust what it produced.
6. **Data Classification Discipline** — Never use classified, CUI, or PII data as AI input unless the tool and hosting environment are explicitly approved for that classification level.
7. **Policy Alignment** — All AI tool usage must be assessed against DoD AI RMF, Zero Trust, SSDF, and DoW-specific policy. Industry tools do not inherit DoD compliance posture automatically.

> **Author notes — Section 4:**
>
> - [NIST] NIST AI RMF GOVERN function underpins all seven principles
> - [CDAO] DoD AI Ethical Principles (2020) align to principles 1, 6, and 7
> - Principle 3 Trust Calibration: [AI4SDLC:calibrated-trust]
> - Prompt Hygiene is out of scope for this play's principles — it governs how teams use AI tools operationally, not whether or how carefully to adopt. Cross-reference `[AI4SDLC:prompt-engineering]` in the companion plays section.
> - Principle 5 Agent Auditability: [AI4SDLC:ai-autonomy_continuum_play] [AI4SDLC:ai-autonomy-implementation-guide]; [FIELD:Navy] eval logs maintained per use case, compared against defined quality criteria
> - Principle 5 skills file security: skills files must be treated with the same rigor as source code — reviewed, version-controlled, audited. A malicious or poorly crafted skills file can corrupt agent behavior pipeline-wide.
> - Principle 7: [FIELD:Army] Bedrock GovCloud path chosen to keep codebase off public infrastructure — a deliberate policy decision, not a workaround. [CDAO] RAI Toolkit SHIELD

---

## SECTION 5 — Core Patterns and Practices

### 5.1 Define the Why Before the What

*Four structured questions answered once per use case. A team may have multiple use cases — each gets its own answers, its own pilot, and its own ADR. These questions are not answered once for the team; they are answered once per adoption decision.*

1. What specific SDLC pain point or gap are you addressing?
2. Which SDLC phase does this use case target? *(Plan, Design, Code, Build, Test, Release, Deploy, Operate, Monitor, Secure, Validate)*
3. Which team personas are most impacted by this use case?
4. What is the classification of the motivation? *(improve quality / increase efficiency / automate / enhance security / accelerate delivery / support compliance)*

What a well-defined why looks like in practice:

> *Army SMDCOE:* Six-person team. Started with one use case — unit testing and security scanning pushed right in the sprint. Target phase — test. Scoped tightly. Added additional use cases only after the first was validated.
>
> *Navy Forge:* Defined nine use cases upfront, each targeting a different phase or workflow. Answered the four questions for each. Ran evaluations independently. A model for teams with multiple adoption candidates.
>
> **Author notes — Section 5.1:**
>
> - [NIST] AI RMF MAP function — identify context, categorize AI use, assess risk
> - [CDAO] Pathway: Responsible AI dimension starts here
> - Question 4 — add "reduce cognitive load" to the motivation classification list in the prose version
> - *agentic* fifth question: "Does this use case involve agentic or autonomous workflows? If yes, apply Pattern 3/4 readiness criteria from Section 3 before proceeding." [AI4SDLC:ai-autonomy_continuum_play]
> - [FIELD:Army] additional: team added framework-specific rules to shared context (e.g., no plain HTML controls where Telerik expected; no var where team preferred const/let). These were version-controlled like source code.
> - [FIELD:Navy] additional: IDE plugin tested for smaller file-scoped tasks; CLI for multi-file context. Team documented successful prompts and prompt sequences to build a foundation for repeatable patterns.

---

### 5.2 SDLC Phase Adoption Sequencing

*Start where risk is lowest and return is clearest. Sequence based on evidence, not enthusiasm.*

| Risk Tier | SDLC Targets | What the Evidence Shows |
| --- | --- | --- |
| **Start Here** | Documentation generation, test scaffolding from specs (not from code), requirements summarization, code review assistance | Army SMDCOE: infrastructure diagram in 5 minutes vs. ~1 week manually. Navy Forge: inline documentation in ~1 minute plus review vs. 6–7 hours. |
| **Approach Carefully** | Code refactoring, greenfield code generation, architecture decision support | Navy Forge: Java scaffolding in 5–6 hours vs. an estimated 40 hours — but algorithmic logic still needed significant human review. |
| **Wait or Gate Heavily** | Autonomous test generation from code (known anti-pattern), autonomous deployment, security remediation without review, production incident response | Army: generated tests reflected the current implementation and the quality of the original requirements — validates the bug, not the intent. Navy: test generation needed thorough review due to truncation, hallucinated method names, and shallow coverage. |

> **Note:** Autonomous test generation directly from code is flagged as an anti-pattern throughout this play. Generate tests from specs or requirements — not from existing code.
>
> **Author notes — Section 5.2:**
>
> - [AI4SDLC:code-gen-play] [AI4SDLC:testing-play] [AI4SDLC:documentation-play] [AI4SDLC:requirements_engineering_play]
> - [FIELD:Army] additional: AI-assisted refactoring cut a JS function's execution time from ~60s to ~2s. Used heavily for .NET Framework to .NET Core 10 migration across ~120 projects — AI handled most of the migration work under developer supervision.
> - [FIELD:Army] spec generation: [AI4SDLC:documentation-play] — note in the Start Here prose; spec generation ~6x faster with iterative prompting (Navy Forge data)
> - [FIELD:Navy] additional: requirements verification was inconsistent — tool described functionality but could not reliably infer or trace the requirement. Bug analysis and fixes were unreliable. Dependency mapping inconsistent enough to require repeated review and re-prompting. Code segmentation and re-architecture severely limited by context window.
> - *agentic*: Pattern 3 orchestrated pipelines — do not introduce until P1/P2 patterns are proven and governance is in place [AI4SDLC:ai-autonomy_continuum_play]

---

### 5.3 Autonomy Scoping

*Declare the pattern before the pilot begins. Hold it constant for the pilot duration — mid-pilot changes invalidate the before/after comparison. These are patterns, not stages. A team can run Pattern 1 for sensitive code and Pattern 2 for test scaffolding simultaneously.*

| Starting Condition | Recommended Pattern |
| --- | --- |
| DevSecOps baseline incomplete | Pattern 1 — AI suggests; human decides every output |
| Baseline present, governance forming | Pattern 1–2 — AI assists; human reviews all outputs |
| Baseline and review gates confirmed | Pattern 2 — AI generates bounded workflow; human reviews before merge |
| Governance mature, metrics established | Pattern 2–3 — AI coordinates across phases; human monitors at gate |

Default to the lowest pattern that is operationally useful. A higher pattern is appropriate when evidence and governance justify it — not when the capability is available.

> *Army SMDCOE stayed in Patterns 1–2. The team did not hand over merge or deploy authority. Governance was enforced through shared context files in source control, locked org-level rules, and a phased rollout — not through trust in the model.*
>
> **Author notes — Section 5.3:**
>
> - [AI4SDLC:ai-autonomy_continuum_play] [AI4SDLC:ai-autonomy-implementation-guide] — full framework and pattern-specific readiness checklists
> - P1/P2/P3/P4 = Assistive Tools, Delegated Agents, Orchestrated Systems, Adaptive Ecosystems
> - "Patterns not stages" — important to call out explicitly in prose; teams often misread this as a progression ladder where P4 is the goal
> - [FIELD:Army] accountability quote to embed in prose: "I don't care if Claude wrote it. You are responsible for that. When it comes time for code review, don't tell me Claude said it was good." — this is a governance posture, not a limitation
> - [FIELD:Navy] IDE plugin vs. CLI maps to P1 vs. P2: plugin for file-scoped tasks; CLI for multi-file context. Pattern selection affects which interface fits.
> - *agentic*: Pattern 3 and Pattern 4 require demonstrated P1/P2 success, dedicated governance infrastructure, and shadow AI prevention controls. Pattern-specific readiness checklists in [AI4SDLC:ai-autonomy-implementation-guide].

---

## SECTION 6 — Decision Framework: Go / Slow / No-Go

*The eight dimensions below are the AI-SWEC pre-event (B-xx) assessment categories, structured as Go / Slow / No-Go signals. Working through this table IS the AI-SWEC pre-event assessment. The outcome — and the rationale behind it — becomes the first recorded entry in the ADR. Each dimension is assessed independently; one No-Go in Governance, Policy, or Environment is sufficient to pause.*

| Dimension | Go | Slow | No-Go |
| --- | --- | --- | --- |
| GAI Strategic Alignment | Plan exists; aligned to documented goal | Plan in development | No plan; purely reactive to mandate |
| Disruption Tolerance | Defined and concurred by leadership | Implied, not documented | Undefined; any failure will escalate |
| SDLC Phase Targeting | Specific phase plus measurable threshold | Phase identified; threshold vague | No phase targeted |
| Baseline Metrics | DORA plus quality baselines captured | Partial baselines; gaps acknowledged | No baselines — no before/after possible |
| Governance and Policy | IL-level confirmed; data handling in place | Policy review in progress; interim controls defined | Conflicts unresolved; no approved hosting path |
| Human Oversight Model | Approval gates defined for all AI outputs | Planned but not operationalized | No review gates defined |
| Team Readiness | Failure modes discussed; trust-building in place | Curious but unprepared | Resistant or overconfident |
| Environment Readiness | Approved tooling in delivery environment | Being provisioned; timeline known | Only shadow tools available |

**Interpreting the result:**

- **Mostly Go** — proceed to bounded pilot; the Go decision and rationale are recorded in the ADR now
- **Mixed Go/Slow** — constrain scope; address Slow signals in the pilot plan; document the gaps and the conditional Go in the ADR
- **Any No-Go in Governance, Policy, or Environment** — pause; resolve before piloting; document the No-Go and what must change in the ADR
- **Mostly Slow** — readiness sprint; revisit in 30–60 days; document current state in the ADR so the revisit has a starting point

**The Go/Slow/No-Go decision initiates the ADR.** In all cases — Go, conditional Go, or No-Go — the assessment outcome and rationale are recorded at this point. The pilot adds post-event (P-xx) data. The decision record starts here.

### 6.1 Record the Decision — ADR Jumpstart

*The Go/Slow/No-Go outcome is the first entry. The ADR is then carried through the pilot and completed with post-event data. See the [ADR template](../resources/ArchitecturalDecisionRecord.md) in docs/resources.*

- **Context:** current SDLC state, pain points, mission context
- **Go/Slow/No-Go Outcome:** the result of the assessment above and the rationale — first recorded entry
- **Decision:** specific SDLC phase, use case, and autonomy pattern being piloted
- **Alternatives Considered:** other phases or use cases evaluated and why they were deprioritized
- **Consequences:** expected benefits, known risks, success threshold
- **Pre-Pilot Baselines:** metric values captured before tooling was introduced *(added at Measure phase)*
- **Post-Pilot Outcomes:** results against the success threshold *(added at After phase)*

The AI-SWEC B-xx question set maps directly to the Context, Decision, and Consequences fields. The P-xx post-event data completes the record. Together they form the complete before/after evidence base.

> **Author notes — Section 6.1:**
>
> - [MITRE AMM] Strategy and Resources pillar helps populate Alternatives Considered and Context — job aid, not the core framework
> - [CDAO] DoD RAI S&I Pathway: defensibility requires documented context, not compliance checkboxes
> - Write into prose: "The AI-SWEC B-xx questions are the interview that builds the ADR. The P-xx questions are the evaluation that completes it."
> - **AI-SWEC question IDs require verification before prose draft.** B-xx/P-xx prefix convention confirmed. Specific question numbers must be verified against the current AI-SWEC decision tool before writing into prose.
>
> **Author notes — Section 6:**
>
> - [NIST] AI RMF MAP function — categorize, classify risk, determine context
> - [CDAO] Risk Management dimension governs the No-Go criteria; Responsible AI dimension governs policy alignment
> - [MITRE AMM] Team Readiness row aligns to MITRE AMM Workforce Development pillar — job aid diagnostic
> - *agentic* conditional row (add when pilot includes agentic components): **Agentic Governance Readiness** — Go: skills file governance, agent registration, and sandbox all in place. Slow: one or two gaps; scoped pilot scope won't require missing elements. No-Go: no skills file governance, no agent registry, no sandbox — stop before any P2+ agentic pattern.
> - Workgroup note: the No-Go criteria for Governance, Policy, and Environment are intentionally strict. "Slow" in those three dimensions does not mean "proceed with caution" — it means those gaps need resolution before the pilot scope is finalized.

---

## SECTION 7 — Implementation Guidance: How to Start

*The full arc, step by step.*

### Before — Define and Decide

1. Scope the why — delivery team, not just leadership; answer the four questions in Section 5.1; write it down; disagreement is useful data
2. Assess readiness — walk the four-dimension checklist in Section 3; document gaps explicitly
3. Make the Go/Slow/No-Go decision — document the decision and its rationale using the ADR Jumpstart fields in Section 6.1; if No-Go, define what needs to change before revisiting

### Measure — Baseline and Design

1. Capture pre-pilot baselines *(the most frequently skipped step — if omitted, the ADR cannot be produced with integrity)*
2. Select the autonomy pattern and scope the pilot — one phase, one tool, one sprint or delivery increment; declare the success threshold before starting

### After — Pilot and Record

1. Run the bounded pilot — hold scope and pattern constant; require human review gates for all AI outputs
2. Evaluate the adoption health composite — document the delta, not just the sentiment
3. Make and record the decision — continue, adjust, or pause; document the rationale and pre/post-event data in an ADR; share with the AI4SDLC Working Group as an early adopter story
4. Iterate or expand — each expansion needs a new why, new baselines, and a new ADR

> *Army SMDCOE rollout is a worked example of this arc: GitLab Duo pilot with 5 users → cloud engineer validates → dev lead validates → expand to full dev team → expand beyond. Phase expansion gated on demonstrated trust, not on a schedule.*
>
> *Navy Forge is a worked example of the Measure phase done well: nine use cases, explicit quality criteria per use case, time savings measured only when quality criteria were met — results tabulated before any tool selection decision.*
>
> **Author notes — Section 7:**
>
> - [FIELD:Army] additional: daily CloudWatch-to-S3-to-SES report showed usage by individual, leaderboard, and cost visibility for planning. Managers used low-adoption signals to identify and coach hesitant developers directly. This is "freed-up time reinvestment" and "trust calibration" metrics in practice.
> - [FIELD:Navy] next steps from evaluation: build prompt playbooks, set mandatory human review points for higher-risk use cases, collect productivity benchmarks over time so future decisions reflect program-specific evidence.
> - Emphasize: expanding scope mid-pilot invalidates the before/after comparison. One of the most common errors in early adopter stories.
> - *agentic*: if pilot involves agentic components, verify agentic governance readiness row in Section 6 before starting. Hold the declared autonomy pattern constant — the pattern declaration IS the scope.

---

## SECTION 8 — Risks, Metrics, and Adoption Health Composite

*Metrics map to the arc: baseline metrics = Measure phase; post-pilot signals and health composite = After phase. Neither has value without the other.*

**Common Risks:**

- Accelerating entropy — faster production of lower-quality code, tests, or documentation; speed without quality is not a win
- Baseline omission — "it felt faster" is not evidence; Army SMDCOE acknowledged missing several baseline metrics after the fact
- Anti-pattern amplification — AI reinforces problems already in the codebase; autonomous test generation from code is the canonical example
- Shadow tool usage — teams without approved tooling use personal accounts and browser access; data security and compliance risk
- Overconfidence after a single successful sprint — one sprint does not validate cross-SDLC AI use

**Delivery Flow — DORA Four Key Metrics:**

| Metric | Capture Pre-Pilot | Track Post-Pilot |
| --- | --- | --- |
| Deployment Frequency | How often the team deploys to production | Did frequency change, and in which direction? |
| Lead Time for Changes | Code commit to production deployment | Did lead time shrink? Did quality hold? |
| Change Failure Rate | % of changes causing production failure | Did AI-assisted changes fail more or less? |
| Mean Time to Recovery | Average recovery time from failure | Was recovery faster or slower with AI-generated code? |

**Software Quality Metrics:**

| Category | Pre-Pilot Baseline | Post-Pilot Signal |
| --- | --- | --- |
| Quantitative Code Quality | Coverage %, cyclomatic complexity, defect density, code churn | Delta in each metric |
| Qualitative Code Quality | Readability, maintainability, testability, documentation completeness | Team-assessed change using a consistent rubric |
| Code Security | Vulnerability count, severity, time to remediate | Did AI tooling introduce new vulnerabilities? |

**Selected Field Results — Documentation:**

| What Was Measured | Result | Source |
| --- | --- | --- |
| Infrastructure diagram from IaC artifacts | 5 minutes vs. approximately 1 week manually | Army SMDCOE |
| Inline documentation, full class | ~1 minute plus 30 minutes review vs. 6–7 hours | Navy Forge |
| Open API specification generation | Approximately 6x faster with iterative prompting | Navy Forge |

**Human-Machine Teaming Indicators** *(directional signals — no hard thresholds yet):*

- **Trust Calibration** — is reliance aligned with demonstrated capability in this context?
- **Oversight Burden Shift** — more or less oversight than expected?
- **Prompt Hygiene Signal** — prompts and skills files versioned and shared, or siloed with individuals?
- **Freed-Up Time Reinvestment** — time saved reinvested in quality and learning, or absorbed into faster delivery?

**Adoption Health Composite — evaluated at pilot close; structured narrative, not a score:**

| Dimension | Key Questions |
| --- | --- |
| Effectiveness and Performance | How appropriate was the AI for this SDLC task? Did it meet requirements and performance expectations? |
| Transparency and Trust | How explainable was the output? Did trust increase, decrease, or hold? |
| Risk Profile Delta | What disruption occurred? Vulnerabilities introduced? Did it stay within defined tolerance? |
| Human Impact | More or less oversight than expected? Effect on team communication, satisfaction, and workload? |
| Value Realization | Were the outcomes defined in the why actually achieved? Did success metrics move as intended? |

> **Author notes — Section 8:**
>
> - [NIST] AI RMF MEASURE function underpins the health composite evaluation
> - [CDAO] Oversight dimension = governance and trust monitoring at pilot close
> - [MITRE AMM] Risk Profile Delta row aligns to MITRE AMM risk governance pillar — flag as a job aid for teams using MITRE AMM as a diagnostic
> - [NIST.AI.600-1] GenAI-specific risks to add to Common Risks: hallucination, data leakage, homogenization of outputs, confabulation of citations — all amplified in agentic workflows
> - *agentic* additional risks: skills file injection (malicious or poorly written agent instruction files corrupt agent behavior pipeline-wide); shadow agent activity (unapproved agents bypass audit trails) [AI4SDLC:ai-autonomy_continuum_play] [AI4SDLC:ai_sdlc_workflows_play]
> - *agentic* health composite additions (for pilots with agentic components): Was the declared autonomy pattern held constant? Were all agent actions logged and attributable to a human trigger? Were any shadow agent or skills file incidents observed?
> - [FIELD:Army] additional: Army tracked line coverage and cybersecurity metrics; also tracked adoption via CloudWatch leaderboard. Did not track: human vs. AI test case ratio, manual changes after AI suggestions, code churn delta — these are the gaps they acknowledged and opportunities for future tracking.
> - [FIELD:Navy] additional: quality judged by SME review per use case; time savings measured only when quality was met. Still in evaluation phase — no DORA baselines formally tracked yet.
> - **AI-SWEC health composite mapping (needs verification before prose):** Effectiveness and Performance corresponds to P-12 and P-13; Transparency and Trust to P-16 and P-17; Risk Profile Delta to P-01, P-02, P-31, P-32; Human Impact to P-19, P-22, P-23, P-35; Value Realization to P-08, P-09, P-36. Verify all P-xx numbers against the current AI-SWEC decision tool before writing into prose.

---

## SECTION 9 — Key Takeaways

1. **Before, measure, after — in that order.** Skipping any phase breaks the evidence chain and makes the ADR impossible to produce with integrity.
2. **The why is not optional.** Without it, you cannot measure success, justify risk, or learn from the experience.
3. **Premature adoption costs more than delayed adoption.** Speed without readiness introduces security risk, erodes trust, and produces results that cannot be evaluated or repeated.
4. **Sequencing matters more than speed.** Documentation and test scaffolding from specifications are the right starting points — not code generation, not autonomous pipelines.
5. **Baselines are the most skipped and most critical step.** No baseline means no ADR. No ADR means no organizational learning.
6. **Declare the autonomy pattern before the pilot. Hold it constant.** Mid-pilot scope changes invalidate the comparison.
7. **The decision is the goal. The ADR is the record.** A deliberate, evidence-based decision to continue, adjust, or pause — documented so it is traceable, justifiable, and defensible. Not a presentation, not a dashboard.
8. **Human oversight is a design requirement.** Review gates are how trust is built, not how delivery is slowed. The goal is earning the right to expand autonomy with evidence.
9. **The cycle repeats.** Each new tool, phase, or autonomy expansion needs its own why, its own baselines, and its own ADR.

> **Author notes — Section 9:**
>
> - Takeaway 7: [CDAO] DoD RAI S&I Pathway — defensibility requires documented context, not compliance checkboxes
> - Takeaway 8: [FIELD:Army] "Whatever they post in for a merge request, that person is responsible." That is a governance posture.
> - *agentic* addition if needed: "Agentic workflows are not exempt from this play. The cost of skipping the before/measure/after discipline scales with the autonomy level." [AI4SDLC:ai-autonomy_continuum_play]

---

## SECTION 10 — Companion Plays and References

**Related Plays:**

- [AI Autonomy Continuum](ai-autonomy_continuum_play.md) — pattern selection, autonomy health index, shadow AI governance
- [AI Autonomy Implementation Guide](ai-autonomy-implementation-guide.md) — pattern-specific readiness checklists, health metrics
- [Fundamentals for Designing an AI-Augmented Tool Chain](fundamentals-play.md) — hosting models, trust architecture
- [AI Workflow Design and Governance](ai_sdlc_workflows_play.md) — workflow governance, review gates, skills file governance
- [Requirements Engineering with AI Assistance](requirements_engineering_play.md) — maturity indicators, traceability
- [AI-Augmented Testing](testing-play.md) — test coverage metrics, defect escape rate, test-from-code anti-pattern
- [Code Completion and Generation](code-gen-play.md) — DevSecOps baseline requirements, code generation risk
- [AI-Assisted Documentation](documentation-play.md) — documentation use case metrics, field results
- AI Agentic Workflow Design and Skills File Governance *(forthcoming)*
- [MITRE AI-SWEC Assessment](aiswec_play-outline.md) — full B-xx/P-xx question set, structured assessment walkthrough, and ADR field mapping; companion online tool in development

**Key References:**

- MITRE AI-SWEC Framework — ArchAITecture Research Collaborative *(core framework)*
- MITRE POV: *Healthy Software, Lasting Value — Engineering the AI-Augmented Software Lifecycle*
- CDAO Pathway to AI Readiness — ai.mil
- DoD Responsible AI Strategy and Implementation Pathway (2022)
- CDAO Responsible AI Toolkit: SHIELD + DAGR
- NIST AI Risk Management Framework 1.0 (2023)
- NIST.AI.600-1 GenAI Profile (2024)
- MITRE AI Maturity Model (2023) — job aid for organizational diagnostic
- DORA 2025 State of AI-Assisted Software Development
- Geoffrey Moore, *Crossing the Chasm* (1991) — Technology Adoption Life Cycle

---

## Architectural Intent

*Internal section — 1–2 sentences. Defines how this play connects to AI4SDLC software health and the autonomy continuum. Consistent with template and ai_sdlc_workflows_play convention.*

This play is the discipline behind every AI adoption decision — first use or fifth expansion. It contributes to software health by anchoring each adoption cycle in defined intent, measurable baselines, appropriate human oversight, and a traceable decision record. The cycle repeats: define the why, pilot with intention, measure the delta, then decide deliberately to continue, adjust, or pause.

---

## DISCUSSION POINTS FOR THE WORKGROUP

1. **Repeating cycle:** The play is framed as a repeating discipline with two re-entry triggers — adopting something new, or revisiting a prior pause when conditions have changed. A "No-Go" is not permanent. This framing is set; workgroup feedback welcome on whether additional re-entry triggers should be named.

2. **MITRE AI-SWEC:** The [AI-SWEC Assessment play](aiswec_play-outline.md) holds the full B-xx/P-xx question set, structured assessment walkthrough, and ADR field mapping. A companion online tool providing visualization and ADR generation is in development. If the workgroup has AI adoption-specific considerations that should be captured in the AI-SWEC play before it moves to prose, flag them now.

3. **Autonomy pattern terminology:** This play uses Pattern 1–4 consistent with the Autonomy Continuum play. Flagging for workgroup awareness — if there is any inconsistency with how other plays use this terminology, it should be resolved before this play moves to prose.

4. **Field examples — contributions welcome:** Army SMDCOE and Navy Forge are the primary examples used throughout. If the workgroup can identify additional DoW early adopter stories before this play moves to prose draft, they will strengthen the evidence base.

5. **ADR template:** The ADR template exists at [docs/resources/ArchitecturalDecisionRecord.md](../resources/ArchitecturalDecisionRecord.md). The play will reference it directly rather than reproduce it. Workgroup input welcome on whether the existing template covers what an AI adoption decision record needs, or whether AI-specific fields should be added.

---

## AUTHORING NOTES

- **ADR link path:** `../resources/ArchitecturalDecisionRecord.md` — play is in docs/plays/; path is correct.
- **Core framework is MITRE AI-SWEC.** MITRE AMM and CDAO Pathway are job aids — cite them as such; do not position them as the framework.
- **AI-SWEC play is available.** Link to [aiswec_play-outline.md](aiswec_play-outline.md) for the full B-xx/P-xx question set. Do not reproduce the full question set here. The AI-SWEC play describes the planned online visualization and ADR generation tool.
- **AI-SWEC IDs require verification before prose.** B-xx/P-xx prefix convention is confirmed (B = before pilot, P = post pilot). Specific question numbers referenced in Section 8 author notes must be verified against the current AI-SWEC decision tool before writing into prose.
- SEI AI Adoption Maturity Model: NOT published; do not reference.
- CDAO realigned under USD(R&E) August 2025 — frameworks remain in force; note in a footnote if mentioning CDAO organizational structure.
- Agentic AI content is woven into existing risk posture framing throughout — NOT a standalone section. The Autonomy Continuum play owns agentic architecture.
- Both early adopter stories target May 2026 publish. Verify publication status before citing.
- "Skeptics" is preferred over "Laggards" (Moore's own term). In DoW contexts, add: *(often constraint-driven — policy, accreditation, or mission risk — rather than posture-driven)*.
- Voice: No em-dashes. No "ensure." No "leverage." No "transformative." Specific over vague. Direct over hedged.

---

*AI4SDLC Play Series — Department of War CIO Guidance. Draft. Not for distribution beyond the AI4SDLC Working Group.*
