---
# automatic badge generation
lifecycle: alpha
last_updated: "2026-08-12"
---

# **AI Governance Rulebook**

## **A Cross-Cutting Index to the AI4SDLC Play Series**

> *One stop for what's required, why, and where to go deeper.*

---

## **Executive Summary**

Every play in this series goes deep on one part of AI-augmented software delivery — hosting models, code generation, testing, requirements, documentation, agent autonomy. Each carries its own guardrails, and by design those guardrails are scattered across the plays that need them.

This guide is the index. It distills the governance obligations already established across the AI4SDLC series — together with federal and DoD AI policy — into fifteen inspectable rules: what's required, what evidence proves it, who owns it, and which play carries the full guidance. It exists so a program manager, authorizing official, or lead can answer "are we covering this" in one pass, without reading eight plays first. It does not replace those plays, and it does not replace law, regulation, component policy, contract terms, classification guidance, ATO conditions, or authorizing official direction.

📌 **Key takeaway:** Use this guide to check coverage fast. Use the linked plays to actually implement.

---

## **1. Why This Guide Matters**

AI can help software teams move faster across requirements, architecture, coding, testing, documentation, deployment, sustainment, and compliance work. That speed only holds up if mission assurance, cybersecurity, legal compliance, acquisition integrity, data rights, human accountability, and the trustworthiness of delivered software hold up with it.[^AI4SDLC][^DoDEthics][^OMB-M2521]

Several plays in this series have already flagged the gap this guide fills. The Code Generation play defers governance explicitly: *"Governance is essential, but beyond the scope of this play."* The same gap is named in the site index, the risk reference companion, and the emerging-practices roadmap. This guide is the answer to that gap — not as another deep-dive play, but as the index that ties the existing ones together.

Where this guide cites OMB M-25-21, it aligns to that memo's governance and risk-management practices as a policy reference point. M-25-21 does not cover AI used as a component of a National Security System — DoD/NSS use follows applicable DoD, intelligence community, and system-specific authorities instead.[^OMB-M2521]

---

## **2. Purpose and Scope**

**In scope:** the fifteen governance rules below, their evidence and ownership, and their mapping back to the play that covers each in depth. Applies to government personnel, military personnel, and support contractors using AI-enabled tools anywhere in the SDLC — requirements, design, code generation, testing, security analysis, DevSecOps, and acquisition support — across public AI services, government cloud services, vendor-hosted tools, IDE assistants, AI agents, self-hosted models, RAG systems, and AI-enabled DevSecOps tools.

**Out of scope:** implementation detail for any single rule — that's what the linked plays are for — and anything requiring legal, contracting, or authorizing-official judgment specific to a system or contract.

**Audience:** program managers, product owners, software factory leads, developers, testers, security teams, authorizing officials, and acquisition professionals.

---

## **3. How to Use This Rulebook**

Treat each rule as a baseline expectation, not a finished compliance argument. For each one, a team should be able to point to evidence, not just intent — and evidence can be lightweight, but it has to be inspectable.

Each rule carries:

- **Requirement** — what the team does.
- **Evidence** — what a reviewer should be able to inspect.
- **Owner** — the role normally accountable.
- **Full guidance** — the play that covers implementation in depth.

---

## **4. Quick Rules**

1. Operate AI tools inside an approved governance and authorization path.
2. Match the AI tool to the data boundary.
3. Do not put sensitive data into unauthorized AI services.
4. Keep humans accountable for AI-assisted work.
5. Review, test, and scan AI-generated code before merge.
6. Verify AI outputs before using them as evidence or authority.
7. Preserve traceability from AI use to delivered artifact.
8. Protect prompts, prompt libraries, retrieval sources, and AI logs.
9. Control AI agents and autonomous workflows.
10. Monitor AI-assisted SDLC outcomes continuously.
11. Preserve government data rights and avoid vendor lock-in.
12. Use AI in acquisition with care.
13. Train the workforce before scaling AI use.
14. Share reusable AI assets when allowed.
15. Inventory AI development tools as supply chain components.

---

## **5. The Rules**

### R1. Operate Inside an Approved Governance and Authorization Path

**Requirement:** AI tools used for SDLC work must be approved for the system, environment, data, and mission use — for DoW proprietary development, through applicable RMF and cybersecurity processes, with auditing and monitoring available to designated authorities. If a tool changes system behavior, security posture, data flow, pipeline, or operational risk, coordinate with the authorizing official or risk owner before use.

**Evidence:** approved tool list, AI use register, ATO boundary notes, risk acceptance record, ADR.
**Owner:** program manager, system owner, authorizing official representative, security lead.
**Full guidance:** [Fundamentals for Designing an AI-Augmented Tool Chain](../plays/fundamentals-play.md) — hosting/usage model decision framework.[^OMB-M2521][^NIST-AIRMF][^DoDI851001][^DoDI8430AA][^CSKS]

### R2. Match the AI Tool to the Data Boundary

**Requirement:** Choose the AI hosting and access model based on data sensitivity, classification, impact level, contractual restrictions, and need-to-know. Use only DoW-authorized models in accredited environments matched to the data's Impact Level. Non-public DoW information may only reach a generative AI application when that application resides on and is authorized for approved DoW information systems, and authorized models must not retain, train on, or expose that data beyond what's explicitly authorized.

**Evidence:** data handling matrix, tool and model authorization record, Impact Level evidence, no-retention/no-training/no-exposure contract terms.
**Owner:** data owner, system owner, security lead, privacy official, contracting officer (vendor tools).
**Full guidance:** [Fundamentals for Designing an AI-Augmented Tool Chain](../plays/fundamentals-play.md) — hosting model comparison matrix.[^DoDZT][^NIST80053][^DoDI8430AA][^DoDI850001][^DoDI858201]

### R3. Do Not Put Sensitive Data into Unauthorized AI Services

**Requirement:** Never submit classified information, CUI, export-controlled data, source-selection information, credentials, secrets, proprietary data, PII, code, configuration, or mission-sensitive source into an AI service unless it resides on approved DoW systems and is explicitly approved for that data. When AI processes information about individuals, apply applicable privacy, civil liberties, and data-minimization requirements.

**Evidence:** acceptable-use policy, data classification guidance, training record, prompt logging policy, privacy review where applicable.
**Owner:** every user; enforced by product owners, security leads, data owners, supervisors.
**Full guidance:** echoed as a guardrail throughout every play in this series — see especially [Fundamentals](../plays/fundamentals-play.md) §3 (Public SaaS risk) and the Data Protection row in each play's guardrail table.[^OMB-M2521][^NIST-GenAI][^DoDI8430AA][^PrivacyAuth]

### R4. Keep Humans Accountable for AI-Assisted Work

**Requirement:** A named human stays accountable for AI-assisted requirements, designs, code, tests, documentation, deployment changes, compliance claims, and risk decisions. AI output can inform the work; it doesn't replace accountable human judgment.

**Evidence:** PR reviewer, approval record, test acceptance, ADR, deployment approval, risk acceptance.
**Owner:** product owner, technical lead, security lead, authorizing official representative, contracting officer (acquisition work).
**Full guidance:** Calibrated Trust and Oversight play (forthcoming) — until published, see the human-accountability guardrail repeated in every play's guiding principles table.[^DoDEthics][^OMB-M2521]

### R5. Review, Test, and Scan AI-Generated Code Before Merge

**Requirement:** AI-generated or AI-modified code, scripts, and tests meet the same engineering bar as human-authored code — peer review, automated tests, static analysis, dependency review, SCA, secret scanning, and applicable dynamic/integration testing. Review explicitly covers security vulnerabilities, safety implications, logical errors, IP infringement, and license obligations. High-impact code, security controls, IaC, and policy logic get elevated review.

**Evidence:** PR review, CI/CD results, SAST/DAST results, dependency scan, SBOM entry, license/provenance review, threat model update.
**Owner:** developer, reviewer, software lead, security engineer, DevSecOps lead.
**Full guidance:** [Leading Practices for Code Completion and Generation](../plays/code-gen-play.md) §6 (Trust, Verification, and DevSecOps Pipeline Integration).[^NIST800218A][^NIST80053][^DoDI8430AA]

### R6. Verify AI Outputs Before Using Them as Evidence or Authority

**Requirement:** AI-generated summaries, vulnerability explanations, compliance mappings, citations, and technical recommendations get verified against authoritative sources before use — they're never the final authority.

**Evidence:** linked authoritative source, reviewer note, citation check, compliance review record.
**Owner:** artifact author, reviewer, security lead, compliance lead, acquisition official (acquisition artifacts).
**Full guidance:** [AI-Augmented Requirements Engineering](../plays/requirements_engineering_play.md) — "GenAI outputs must be validated, traceable, and never treated as final authority."[^AI4SDLC][^NIST-AIRMF][^NIST-GenAI]

### R7. Preserve Traceability from AI Use to Delivered Artifact

**Requirement:** When AI materially affects a deliverable, record enough to reconstruct what tool was used, who used it, what context or prompt pattern was given, what changed, who reviewed it, and what decision resulted.

**Evidence:** AI use register, commit message, PR template field, prompt template version, model/service version, reviewer identity.
**Owner:** developer, product owner, software lead, security lead, records owner.
**Full guidance:** AI Supply Chain Transparency Guide (forthcoming); prompt/output logging covered today in [Code Generation](../plays/code-gen-play.md) §6.[^DoDEthics][^OMB-M2521]

### R8. Protect Prompts, Prompt Libraries, Retrieval Sources, and AI Logs

**Requirement:** Treat prompt templates, libraries, retrieval indexes, embeddings, model configuration, and AI logs as governed SDLC assets — versioned, access-controlled, reviewed, and protected to the sensitivity of what they expose or influence.

**Evidence:** repository record, configuration baseline, access control list, audit log, retention rule, change approval.
**Owner:** tool owner, repository owner, security lead, data owner.
**Full guidance:** [Prompt Engineering](../plays/prompt-engineering.md); PromptOps pattern in [Fundamentals](../plays/fundamentals-play.md) §2.[^NIST80053][^NIST-AIRMF]

### R9. Control AI Agents and Autonomous Workflows

**Requirement:** Any AI agent that can edit repositories, run commands, create tickets, open PRs, change infrastructure, or act in CI/CD needs documented scope, permissions, logging, a rollback path, and human approval gates proportionate to risk. Autonomous action against production, mission systems, authorization boundaries, or high-impact environments needs explicit approval.

**Evidence:** agent workflow design, permission model, approval gate, tool allowlist, audit log, rollback plan, risk assessment.
**Owner:** software lead, DevSecOps lead, security lead, system owner, authorizing official representative.
**Full guidance:** [AI & Agentic Workflow Design and Governance](../plays/ai_sdlc_workflows_play.md) — the six-level task-autonomy framework is the direct implementation of this rule.[^NIST-AIRMF][^DoDEthics]

### R10. Monitor AI-Assisted SDLC Outcomes Continuously

**Requirement:** Monitor AI-assisted workflows for defects, insecure code, hallucinated citations, privacy incidents, data leakage, review-depth erosion, model drift, and mission impact. For each AI-enabled capability: determine use-case-specific risk, define measurable benchmarks, verify performance before deployment, monitor continuously in operation, and address detected bias through existing risk management. Suspend or redesign unsafe or ineffective use.

**Evidence:** metrics dashboard, defect trend, incident report, benchmark definition, pre-deployment verification, bias monitoring record, corrective action.
**Owner:** product owner, software lead, security lead, quality lead, program manager.
**Full guidance:** [Fundamentals](../plays/fundamentals-play.md) §7 (Measures and Success Indicators); [AI-Augmented Testing](../plays/testing-play.md) §8 for defect-escape and coverage metrics.[^OMB-M2521][^NIST-AIRMF][^DoDI8430AA]

### R11. Preserve Government Data Rights and Avoid Vendor Lock-In

**Requirement:** Contracts and task orders for AI-enabled SDLC tools preserve government rights in code, data, prompts, logs, generated artifacts, and metadata needed for oversight, migration, continuity, and competition. Vendors may not train on or improve commercial offerings using federal information unless expressly authorized.

**Evidence:** contract clause, data rights assertion, terms-of-service review, exit plan, interoperability requirement, post-award monitoring plan.
**Owner:** contracting officer, program manager, legal counsel, data owner, technical lead.
**Full guidance:** no dedicated play yet — apply directly from cited policy.[^OMB-M2521][^OMB-M2522]

### R12. Use AI in Acquisition with Care

**Requirement:** AI may assist acquisition research, drafting, comparison, and summarization, but acquisition officials validate facts, preserve competition, protect procurement-sensitive information, and avoid overreliance on opaque vendor outputs. Software systems designed or implemented with AI align with applicable software acquisition and digital engineering policy.

**Evidence:** source verification, acquisition review, market research record, procurement-sensitive data handling note, contracting officer approval.
**Owner:** contracting officer, program manager, requirements owner, technical evaluator.
**Full guidance:** no dedicated play yet — apply directly from cited policy.[^OMB-M2522][^OMB-M2521][^DoDI8430AA][^DoDI500087][^DoDI500097]

### R13. Train the Workforce Before Scaling AI Use

**Requirement:** Teams get role-based training before scaling AI-assisted workflows — approved tools, data handling, secure prompt engineering, common AI-generated vulnerability patterns, verification, IP/license risk, traceability, and incident escalation.

**Evidence:** training record, role-based curriculum, onboarding checklist, refresher cadence.
**Owner:** program manager, software factory lead, training lead, security lead, supervisors.
**Full guidance:** workforce-focused play not yet published (flagged as forthcoming in [Code Generation](../plays/code-gen-play.md) §9) — apply directly from cited policy in the meantime.[^OMB-M2521][^DoDRAIStrategy][^DoDI8430AA]

### R14. Share Reusable AI Assets When Allowed

**Requirement:** Share reusable AI code, prompts, evaluation harnesses, models, and lessons learned across government — and as open source — when law, classification, national security, privacy, contract, and operational security constraints allow.

**Evidence:** repository link, reuse catalog entry, release approval, data rights review, or a decision record explaining why release is restricted.
**Owner:** program manager, repository owner, legal counsel, data owner, security lead.
**Full guidance:** no dedicated play yet — the AI4SDLC series is itself an example of this practice.[^OMB-M2521][^AI4SDLC]

### R15. Inventory AI Development Tools as Supply Chain Components

**Requirement:** Code completion plugins, AI test generators, documentation assistants, and other AI-enabled dev tools get inventoried like any other SDLC dependency — assessed, approved, sourced from authorized DoW repositories, and covered by SCRM and an accurate SBOM. Track AI service endpoints, models, versions, significant datasets, and prompt templates as SBOM extensions or an emerging AIBOM. Treat AI-generated artifacts entering a controlled repository as a supply chain event from the point of generation, including at lower Impact Levels.

**Evidence:** AI tool inventory cross-referenced with SBOM/AIBOM entries, authorized repository provenance, SCRM/security assessment evidence, current SBOM, lifecycle documentation referencing AI tool use.
**Owner:** software factory architects, tool chain managers, program managers.
**Full guidance:** AI Supply Chain Transparency Guide (forthcoming).[^DoDI8430AA][^DoDI500082]

> **Note:** DoDI 8430.AA is draft/emerging guidance. Treat its AIBOM provisions as forward-looking practice, not a currently binding requirement, unless confirmed otherwise by an authoritative current version.

---

## **6. Prohibited and High-Risk Practices**

The following are prohibited unless an explicit, documented exception is approved by the appropriate authority:

- Using unauthorized AI tools to process mission data, source code, CUI, credentials, secrets, or procurement-sensitive information.
- Using unassessed or unapproved third-party software components — including OSS — outside authorized DoW repositories.
- Accepting AI-generated code, tests, documentation, vulnerability analysis, or compliance claims without qualified human review.
- Allowing AI agents to act on production, mission systems, authorization boundaries, or infrastructure without scoped permissions, logging, rollback, and human approval gates.
- Citing AI-generated references, legal claims, or security control mappings without checking authoritative sources.
- Using AI outputs as the sole basis for authorization, risk acceptance, or compliance attestation.
- Allowing vendors to reuse federal data, prompts, logs, or generated artifacts to improve commercial offerings without explicit authorization.
- Treating prompt libraries, retrieval content, embeddings, or AI logs as unmanaged personal productivity artifacts.

---

## **7. Minimum Implementation Checklist**

Each program or software factory should maintain:

- **AI use register** — approved use cases, tools, owners, data types, environments, risk level.
- **Approved tool list** — authorized tools, model/service versions where available, permitted data categories, prohibited uses.
- **Data handling matrix** — what data may reach public, government cloud, controlled cloud, self-hosted, air-gapped, and hybrid AI tools.
- **AI model/environment approval evidence** — DoW-authorized model list, Impact Level evidence, no-retention/no-training/no-exposure terms.
- **Review checklist** — required human review, source verification, testing, scanning for AI-assisted artifacts.
- **Traceability mechanism** — PR field, ticket field, commit convention, or register entry for material AI use.
- **Prompt and log policy** — storage, access control, retention, and protection rules.
- **Agent control plan** — permission model, tool allowlist, approval gates, audit logs, rollback plan.
- **Acquisition checklist** — data rights, vendor reuse restrictions, interoperability, exit plan, monitoring.
- **AI dev-tool inventory** — SBOM/AIBOM entries and lifecycle documentation.
- **Training plan** — role-based training before tool access, refreshers on change.
- **Monitoring plan** — metrics and review cadence for quality, security, privacy, and unsafe or ineffective use.

---

## **8. Key Takeaways**

- **This guide is the index, not the depth.** Fifteen rules, each pointing to the play that covers implementation.
- **Evidence beats intent.** Every rule names what a reviewer should be able to inspect — lightweight is fine, invisible isn't.
- **Humans stay accountable.** No rule here shifts accountability onto the tool.
- **Four rules point to forthcoming plays** — human accountability depth (R4), traceability/AIBOM depth (R7), data rights (R11), acquisition use (R12), and workforce training (R13) — apply the cited policy directly until those land.
- **AI use may be used to improve speed, quality, and mission outcomes only when it's authorized, the data fits the tool, outputs are independently reviewed, and traceability, testing, cybersecurity, data rights, and human accountability all hold.** Use that materially affects mission outcomes, safety, rights, security, compliance, or production systems gets elevated review before deployment.

---

## **9. Related Plays and References**

**Related Plays**

- [Fundamentals for Designing an AI-Augmented Tool Chain](../plays/fundamentals-play.md)
- [Leading Practices for Code Completion and Generation](../plays/code-gen-play.md)
- [AI-Augmented Requirements Engineering](../plays/requirements_engineering_play.md)
- [AI-Augmented Testing](../plays/testing-play.md)
- [AI & Agentic Workflow Design and Governance](../plays/ai_sdlc_workflows_play.md)
- [Prompt Engineering](../plays/prompt-engineering.md)

**Key References**

[^AI4SDLC]: Department of War CIO, [AI for the SDLC](https://code.mil/AI4SDLC/). Describes AI use across the software value stream, emphasizing mission readiness, security and compliance, human-AI teaming, and traceability.

[^DoDEthics]: Department of Defense, [DOD Adopts Ethical Principles for Artificial Intelligence](https://www.defense.gov/News/Releases/Release/Article/2091996/dod-adopts-ethical-principles-for-artificial-intelligence/), Feb. 24, 2020. Principles: Responsible, Equitable, Traceable, Reliable, Governable.

[^OMB-M2521]: Office of Management and Budget, *M-25-21, Accelerating Federal Use of AI through Innovation, Governance, and Public Trust*, Apr. 3, 2025.

[^OMB-M2522]: Office of Management and Budget, *M-25-22, Driving Efficient Acquisition of Artificial Intelligence in Government*, Apr. 3, 2025.

[^NIST-AIRMF]: National Institute of Standards and Technology, [Artificial Intelligence Risk Management Framework (AI RMF 1.0)](https://www.nist.gov/itl/ai-risk-management-framework), Jan. 2023.

[^NIST-GenAI]: National Institute of Standards and Technology, [AI RMF: Generative Artificial Intelligence Profile, NIST AI 600-1](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf), Jul. 2024.

[^NIST80053]: National Institute of Standards and Technology, [Security and Privacy Controls for Information Systems and Organizations, NIST SP 800-53 Rev. 5](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final), Sep. 2020, with updates.

[^NIST800218A]: National Institute of Standards and Technology, [Secure Software Development Practices for Generative AI and Dual-Use Foundation Models, NIST SP 800-218A](https://csrc.nist.gov/pubs/sp/800/218/a/final), Jul. 2024.

[^DoDI851001]: Department of Defense, *Risk Management Framework (RMF) for DoD Systems*, DoDI 8510.01, Jul. 19, 2022, incorporating change 3, May 29, 2023.

[^DoDI8430AA]: Department of Defense, *Software Bill of Materials and AI Bill of Materials*, DoDI 8430.AA (draft/emerging guidance), 2025.

[^CSKS]: Department of Defense, *AI Cybersecurity Risk Management Tailoring Guide*; DoD Cybersecurity Knowledge Service, <https://cybersecurityks.osd.mil/>.

[^DoDZT]: Department of Defense CIO, [DoD Zero Trust Strategy](https://dodcio.defense.gov/Portals/0/Documents/Library/DoD-ZTStrategy.pdf), Oct. 21, 2022.

[^DoDI850001]: Department of Defense, *Cybersecurity*, DoDI 8500.01.

[^DoDI858201]: Department of Defense, *Security of Unclassified DoD Information on Non-DoD Information Systems*, DoDI 8582.01.

[^PrivacyAuth]: Privacy authorities include the Privacy Act of 1974, DoDI 5400.11, DoD 5400.11-R Vol. 2 of DoDM 5400.11, DoDD 3115.18, and, where applicable, Intelligence Community Policy Memorandum 504 (01).

[^DoDI500087]: Department of Defense, *Operation of the Software Acquisition Pathway*, DoDI 5000.87.

[^DoDI500097]: Department of Defense, *Digital Engineering*, DoDI 5000.97.

[^DoDI500082]: Department of Defense, *Acquisition of Digital Capabilities*, DoDI 5000.82.

[^DoDRAIStrategy]: Department of Defense, *Responsible Artificial Intelligence Strategy and Implementation Pathway*, Jun. 2022.

---

**End of Guide**
