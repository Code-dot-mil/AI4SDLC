---
lifecycle: rc
last_updated: "2026-08-12"
---

# **Play: From Protovibe to Production**

> *Groundbreaking potential with limitations and challenges.*

---

## **Executive Summary**

Vibe coding puts the ability to build working software in the hands of people who could never build it before. A logistics analyst, a contracting officer, a clinician, a program manager: anyone who understands a problem deeply can now turn an idea into something that runs, in hours, without waiting in a development queue. This play is about keeping that value while handling what comes with it.[^OWASP]

The artifact that comes out of a vibe coding session is what we call a **protovibe**: working software that demonstrates intent but has not yet been fully understood by a human steward. Both halves of that sentence are true at once. A protovibe is genuinely valuable, but its implementation and operational consequences remain unknown.

Every protovibe ends with a recorded outcome, chosen through a deliberate decision. Outcomes 1–5 preserve value without putting code into sustained use. Outcomes 6–8 allow selected code to enter sustained use only after it passes the baseline transition gate.

📌 **Key takeaway:** Vibe freely. Then make a conscious decision and record an outcome for every protovibe. Sustained-use outcomes can be achieved and recorded only after passing the gate.

---

## **1. What Vibe Coding Is and Why It Matters**

Vibe coding is the use of AI to generate runnable software from natural-language instructions, with the person directing it evaluating results rather than reading and understanding the generated code. The term comes from Andrej Karpathy in February 2025.[^Karpathy] Academic work has since framed it as a genuine shift in how intent reaches a machine, moving the mediation of developer intent from deterministic instruction to probabilistic inference.[^Meske]

This play uses **proto-vibing** as its term for that practice — a rename that keeps the activity (proto-vibing) and its artifact (protovibe) in the same word family, rather than two unrelated terms. Vibe coding remains the term of record in the literature cited throughout; proto-vibing is this play's label for it, used from here forward.

**The appeal is real and measurable.** A systematic review of 101 practitioner sources found speed and efficiency to be the dominant motivation at 62 percent of coded accounts, followed by accessibility and empowerment at 14 percent and learning and experimentation at 11 percent.[^Fawzy] Non-developers in that review described building automations and public-facing forms without waiting on an IT department. For DoW programs, that is the headline: subject matter expertise can now be expressed directly as working software.

This capability is most powerful exactly where traditional development is slowest. Requirements that are hard to write down but easy to demonstrate. Ideas that need a stakeholder's reaction before they deserve funding. Design questions with three plausible answers and no cheap way to compare them. Proto-vibing turns one-way doors into reversible experiments.

### 1.1 The Protovibe

A **protovibe** is the artifact a proto-vibing session leaves behind: working software that demonstrates intent but has not yet been fully understood by a human steward.

Naming the artifact matters because governance begins with what happens after a session ends. A program can track protovibes and record their dispositions. It cannot usefully govern the abstract question of whether proto-vibing is good.

### 1.2 Comprehension Is Its Own Axis

Proto-vibing is not a point on the task-level autonomy scale in the [AI & Agentic Workflow Design and Governance](ai_sdlc_workflows-play.md) play. That scale governs what an agent is *permitted* to do. Comprehension is a separate condition: it describes what a human understands about the output. A developer at Level 1 who applies a draft without reading it has completed the manual-application step but has not meaningfully performed the required review. That play already flags the same hazard when it warns that approving outputs without meaningful evaluation turns human-in-the-loop review into a formality.

Agent count is likewise orthogonal. One agent whose output nobody reads produces a protovibe. Twelve agents orchestrated against a spec, with an engineer understanding the generated code, do not. Scaling the mechanism does not change the comprehension question, though it does raise the containment requirement (Section 6).

This matches what the empirical record shows about professionals. A study of 13 field observations and 99 experienced developers found that experienced developers retain agency over software design and implementation, planning before implementing and validating agent output, with expertise superseding vibes among the participants.[^Huang] Protovibes are therefore not primarily a senior engineering problem. They arise where domain expertise is high and software engineering expertise is thin, which is precisely where the capability is most valuable.

> ⚠️ **Relationship to Code Generation.** The [Leading Practices for Code Completion and Generation](code-gen-play.md) play introduces VIBE Programming and directs that it be bounded by policy with strict gating before promotion. This play operationalizes that direction. It does not replace or amend it. A cross-reference should be added when that play next revises.

---

## **2. Purpose and Scope**

This play governs what happens after a proto-vibing session ends: how a program names, contains, and records the disposition of the resulting protovibe. It does not decide whether to permit proto-vibing in the first place — that boundary is set by the [Code Generation play](code-gen-play.md)'s VIBE Programming guidance — and it does not cover model selection, IDE tooling, or prompt technique.

**In scope:** the protovibe as a named artifact, its eight possible outcomes, the baseline transition gate, and the environment controls that make exploration safe before that gate.

**Out of scope:** agent orchestration mechanics (see [AI & Agentic Workflow Design and Governance](ai_sdlc_workflows-play.md)), requirements elicitation technique (see [Requirements Engineering](requirements_engineering_play.md)), and tool- or vendor-specific guidance.

**Audience:** engineers and subject matter experts running proto-vibing sessions; technical leads deciding a protovibe's disposition; and program leaders responsible for resourcing any follow-on work.

---

## **3. Prerequisites and Readiness**

A team does not need a mature DevSecOps pipeline to start proto-vibing — that is the point. These are the minimum conditions for running this play safely:

- An environment that can be made ephemeral and isolated, with no path to production credentials or systems
- Synthetic or approved non-sensitive data available for exploration
- A place to record protovibe inventory, expiration dates, and recorded outcomes, even a simple tracker
- A named engineer able to own comprehension review before code selected for Outcomes 6–8 crosses the gate in Section 5
- Familiarity with the Code Generation play's [high-risk use-case boundaries](code-gen-play.md#high-risk-use-cases), echoed in Section 6

Teams without an established DevSecOps baseline can still vibe code under Section 6's controls. They cannot yet place code in sustained use through Outcomes 6–8, because Step 3 of the gate assumes CI/CD, SSDF or SLSA practice, and SBOM, MLBOM, and AIBOM tracking are already in place.

---

## **4. Eight Outcomes**

Every protovibe ends with a recorded outcome, chosen through a deliberate decision. Outcomes 1–5 do not place code in sustained use. Outcomes 6–8 can be achieved and recorded only after selected code passes the baseline transition gate in Section 5.

| **Outcome** | **What survives** | **Sustained use** |
| --- | --- | --- |
| **1. Insight** | A design decision, a stakeholder alignment, a changed opinion | No |
| **2. Negative verdict** | A documented "no": infeasible, not worth it, wrong requirement | No |
| **3. Lessons learned** | Knowledge about the attempt; artifact deliberately disposed | No |
| **4. Requirements and production vision** | Spec, interface contracts, acceptance criteria, ADRs | No |
| **5. Reference oracle** | The running protovibe as a behavioral specification for a clean rebuild | No |
| **6. Selective salvage** | Named fragments; the remainder disposed | Yes—after passing the gate |
| **7. Debt-baselined transition** | The full codebase, with a funded and owned debt register | Yes—after passing the gate |
| **8. Bounded sustainment** | The full codebase, used as-is inside hard constraints | Yes—after passing the gate |

**Insight and negative verdict are wins.** A protovibe that proves an idea will not work, in two days, has saved a program a year and a program office a great deal of money. Record it as a deliverable, because a kill decision that nobody writes down gets re-litigated in twelve months.

**Lessons learned preserves what the code taught without preserving the code.** Record the question explored, assumptions, key results, and reason for disposal in the protovibe inventory. This makes failed experiments reusable and prevents later teams from repeating the work or rediscovering the same insight.

**Requirements and production vision is the highest-leverage outcome for DoW work.** The specification, interface contracts, and Architectural Decision Records[^ADR] that result from this disposition are the same artifacts an authorization package demands anyway, so the governance is not pure overhead. Pair this outcome with the [Requirements Engineering](requirements_engineering_play.md) play. One caveat carried from industry practice: the specification informs the rebuild, it does not become the source of truth. Executable code remains the source of truth requiring maintenance.[^TWSDD]

**Reference oracle** keeps the protovibe running but never ships it. Access is limited to the clean-rebuild team; it may not acquire an operational user base. Engineers run it alongside the clean build and compare behavior, letting the protovibe define expected results for cases nobody wrote down. This is the right answer when domain logic is tacit, living in an expert's head rather than in any document.

### 4.1 The Undecided Protovibe

One ending falls outside the eight defined outcomes: the protovibe quietly acquires users and obligations while no outcome is deliberately selected.

Nobody chooses this. Teams arrive at it because recording an outcome requires a deliberate decision, while drift is free. Practitioner accounts show what accumulates in the meantime: only 29 percent of coded quality assurance behaviors involved manual testing or edits, while 36 percent skipped quality assurance entirely, 18 percent reflected uncritical trust in the output, and 10 percent delegated verification back to the AI that wrote the code.[^Fawzy]

---

## **5. The Baseline Transition Gate**

Outcomes 6, 7, and 8 place code in sustained use. Each can be achieved and recorded only after the selected code passes the baseline transition gate. Its three steps must occur in order because each depends on the preceding step.

The gate is what turns a promising protovibe into something a program can fund, staff, defend at a milestone review, and hand to a different team in the future.

**Step 1: Comprehension.** A named engineer must be able to explain what the code does and why it is structured as it is to a reviewer who did not write the prompt. “The team looked at it” is not enough. That engineer is recorded as the code’s owner, by name and date. This step has no tooling or shortcut and must not be skipped.

**Step 2: Architecture baseline.** Document the as-built system: data model, trust boundaries, authorization model, external dependencies, deployment topology, and data sensitivity. Then record the delta between as-built and as-should-be. Nothing is remediated here. The delta *is* the debt register, and every entry gets an owner.

**Step 3: DevSecOps entry.** With comprehension and an architecture baseline established, bring the code into the existing [DevSecOps baseline](code-gen-play.md#devsecops-baseline) and apply its [verification practices](code-gen-play.md#verification-practices). Then add the protovibe-specific controls in [Section 5.2](#52-what-to-add-for-ai-generated-code).

> ⚠️ **Order matters.** Running a protovibe through the pipeline before Steps 1 and 2 produces a clean scan report on code nobody understands. That is worse than no report, because it manufactures confidence.

If a gate transition is deferred or does not pass, the protovibe remains subject to the controls in [Section 6](#6-working-safely-before-the-gate) until the team remediates and retries or records an Outcome 1–5.

### 5.1 Applying the Gate to Sustained-Use Outcomes

The gate applies differently depending on the proposed sustained-use outcome:

- **Selective salvage** identifies the fragments of the protovibe to be sustained, scopes all three steps to those fragments, and records the disposal of the remaining codebase in the protovibe inventory.
- **Debt-baselined transition** applies all three steps to the full protovibe codebase. It carries the resulting debt register forward, with an owner, a date, and a funding line for every entry.
- **Bounded sustainment** accepts the debt of the full protovibe codebase permanently and substitutes hard usage constraints for remediation: a named user population, no sensitive data, no production integration, an inventory entry, and a set review interval. Note that these constraints are only enforceable because Step 2 documented the boundaries they refer to.

### 5.2 What to Add for AI-Generated Code

A standard pipeline validates code. These controls address what is distinctive about protovibes.

| **Risk area** | **Control** |
| --- | --- |
| Configuration posture | Scan IaC, storage ACLs, and database access policy, not code alone |
| Authorization paths | Discrete human review gate; verify server-side enforcement |
| Static and dynamic analysis | Run SAST/DAST against the protovibe as if it were freshly written code; "it already worked" is not an exemption |
| Test coverage | Require tests for edge cases and failure paths, not just the happy-path demo; treat weak error handling as a blocking finding |
| Duplicated code and logic | Enforce thresholds for duplicated code and copy-pasted logic as a quality gate |
| Provenance and dependencies | Validate dependency names, sources, and integrity; check for nonexistent or typosquatted packages and known vulnerabilities |
| Attribution | Record AI authorship at commit, feeding SBOM and AIBOM |

The emphasis on configuration and authorization is deliberate. Research across large enterprise codebases found 322 percent more privilege escalation paths and 153 percent more design flaws in AI-generated code, gaps automated scanners miss because they reflect architectural decisions rather than discrete code-level defects.[^Apiiro]

Other controls address risks that model capability alone does not resolve. Syntax pass rates for generated code rose from roughly 50 percent to 95 percent between 2023 and 2026 while security pass rates stayed flat between 45 and 55 percent.[^Veracode] Maintainability signals move the same way, with refactored code falling from 21 percent of changed lines in 2022 to 3.8 percent in 2026 and duplicated blocks rising 81 percent.[^GitClear]

---

## **6. Working Safely Before the Gate**

Explore freely. The controls below are what make that freedom safe to grant, which is why they belong to the environment rather than to the prompt. They apply to every protovibe from the moment a session starts.

| **Risk area** | **Control** |
| --- | --- |
| Credentials | No production credentials in a protovibe session, without exception |
| Data | Synthetic or approved non-sensitive data only |
| Infrastructure | Ephemeral and isolated, with automatic teardown |
| Network | Egress restricted to approved destinations |
| Lifespan | A recorded expiration date on every protovibe |
| Visibility | An inventory entry created at session start |
| Multi-agent | Parallel agents require isolated infrastructure and no shared credentials |

**Where to start high, not low.** Some work must not begin as a protovibe. Authentication and authorization logic, cryptographic implementation, safety- or mission-critical software, and work involving classified, CUI, or export-controlled material must begin under standard engineering and DevSecOps controls.

This boundary is consistent with concerns experienced developers report about AI agents in security-critical code, high-stakes or privacy-sensitive tasks, core business logic, and complex architectural refactoring.[^Huang]

For work above IL-2, isolated infrastructure and synthetic data are mandatory.

> ⚠️ **Promotion rule.** A protovibe is no longer a prototype when it acquires its first non-author user. The threshold is not production deployment or access to sensitive data; it is the point at which someone else’s expectations depend on code that has not been fully understood by a human steward.

---

## **7. Key Takeaways and Next Steps**

- **Vibe freely, then decide.** Make a conscious decision and record an outcome for every protovibe. Outcomes 1–5 do not place code in sustained use; sustained-use outcomes can be achieved and recorded only after passing the gate.
- **Name the artifact.** A protovibe is working software that demonstrates intent but has not yet been fully understood by a human steward. Both halves are true.
- **Insight and negative verdicts are wins.** Record them. A documented "no" delivered in two days is a real deliverable.
- **Comprehension comes first, then architecture, then the pipeline.** Reversing that order manufactures confidence.
- **Controls live in the environment.** The mode is a team decision; the guardrails are an organizational standard.

**Next steps.** Pilot on one team. Track three measures: protovibes with a recorded outcome, expiration dates honored, and gate transitions passed, deferred, or failed. Frame results using [AI-SWEC](code-gen-play.md#ai-swec-grounding-your-why) and submit lessons learned to the AI4SDLC Working Group.

Proto-vibing is groundbreaking potential with limitations and challenges. Run the play, and the potential is what your program keeps.

### Related Plays and Resources

- [Leading Practices for Code Completion and Generation](code-gen-play.md)
- [AI & Agentic Workflow Design and Governance](ai_sdlc_workflows-play.md)
- [AI Autonomy Continuum](ai-autonomy_continuum_play.md)
- [Requirements Engineering](requirements_engineering_play.md)
- [Architectural Decision Records](../resources/ArchitecturalDecisionRecord.md)

**End of Play**

---

[^OWASP]: OWASP Foundation, "X03:2025 Inappropriate Trust in AI-Generated Code ('Vibe Coding')," *OWASP Top 10:2025 Next Steps*, 2025. [Online]. Available: https://www.owasp.org/Top10/2025/X01_2025-Next_Steps/#x03-2025-inappropriate-trust-in-ai-generated-code-vibe-coding

[^Karpathy]: A. Karpathy, post on X, Feb. 2025. Describes a workflow in which the developer accepts generated code without reviewing it.

[^Meske]: C. Meske, T. Hermanns, E. von der Weiden, K.-U. Loser, and T. Berger, "Vibe Coding as a Reconfiguration of Intent Mediation in Software Development: Definition, Implications, and Research Agenda," *IEEE Access*, vol. 13, pp. 213242-213259, 2025. doi:10.1109/ACCESS.2025.3645466

[^Fawzy]: A. Fawzy, A. Tahir, and K. Blincoe, "Vibe Coding in Practice: Motivations, Challenges, and a Future Outlook - a Grey Literature Review," in *2026 IEEE/ACM 48th International Conference on Software Engineering: Software Engineering in Practice (ICSE-SEIP '26)*, Rio de Janeiro, Brazil, Apr. 2026. doi:10.1145/3786583.3786866. Systematic review of 101 practitioner sources yielding 518 coded behavioral units.

[^Huang]: R. Huang, A. Reyna, S. Lerner, H. Xia, and B. Hempel, "Professional Software Developers Don't Vibe, They Control: AI Agent Use for Coding in 2025," arXiv:2512.14012, Dec. 2025. Thirteen field observations and 99 survey responses from developers with three or more years of professional experience.

[^ADR]: AI4SDLC, "Architectural Decision Records," [../resources/ArchitecturalDecisionRecord.md](../resources/ArchitecturalDecisionRecord.md)

[^TWSDD]: Thoughtworks, *Technology Radar*, Vol. 34, Apr. 2026. Spec-driven development is placed in the Assess ring, with executable code retained as the source of truth requiring maintenance.

[^Apiiro]: Apiiro research across large enterprise codebases, 2026, comparing AI-generated and human-authored code. *Confirm against the primary publication before promotion beyond draft.*

[^Veracode]: Veracode, "GenAI Code Security Report," 2025, and Spring 2026 update.

[^GitClear]: GitClear, "The Maintainability Gap," 2026. Analysis of code-change data spanning 2023-2026.

