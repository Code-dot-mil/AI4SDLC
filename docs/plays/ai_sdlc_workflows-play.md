---
title: "Play: AI & Agentic Workflow Design and Governance"
lifecycle: ga
last_updated: "2026-03-15"
---

# **Play: AI & Agentic Workflow Design and Governance**

> *"Autonomy is earned, not assumed"*
>
> "The question isn't whether to automate: it's where the human stays in the loop." — AI4SDLC Working Group

---

## **Executive Summary**

AI is entering DoW software workflows at multiple levels simultaneously: inside the IDE, within CI/CD pipelines, and through coordinated agent workflows where multiple AI systems operate toward shared objectives. Teams are adopting faster than governance can keep pace. The result is inconsistent practice, expanding attack surfaces, and genuine uncertainty about where human accountability begins and ends. A developer using an AI coding assistant operates in one governance context. A CI/CD pipeline invoking an AI agent to generate tests and open merge requests operates in a different one. A multi-agent system coordinating analysis, code generation, and security review across a sprint operates in a third. Each context requires different governance, different oversight design, and a different definition of what it means for a human to remain in the loop.

This play provides the operational governance layer that teams need to deploy AI workflows responsibly. It introduces a task-level autonomy framework (a six-level scale for defining what any AI agent is permitted to do on any specific task) and connects that framework to the architectural patterns in the [AI Autonomy Continuum](ai-autonomy_continuum_play.md). The Continuum answers the architectural question: "What kind of AI workflow have we designed?" This play answers the operational question: "What is this agent permitted to do right now, on this specific task?" The two frameworks are designed to be used together. The Continuum establishes the architectural envelope; this play governs the behavior within it. It also addresses agent persona design (a version-controlled behavior definition file specifying an AI agent's role, behavioral constraints, output format, escalation conditions, and data handling rules) as the primary mechanism for translating autonomy decisions into enforceable agent behavior.

The governance approach in this play is calibrated to DoW operational context. AI workflow failures in mission-critical software carry consequences that differ fundamentally from commercial failures. Speed is not the measure of success here; mission assurance is. That means human accountability remains at the center, guardrails require precision and testing, and autonomy levels are selected per task rather than assumed organization-wide. Teams that deploy AI workflows without this governance layer are not moving faster; they are accumulating risk they will pay for later, often at the moment it is most expensive.

📌 **Key takeaway:** Define what each AI agent is permitted to do before it does anything. Governance is not overhead; it is the condition that makes AI augmentation trustworthy and sustainable in a mission-critical environment.

---

## **1. Why This Play Matters**

Software teams across DoW are deploying AI assistants, agents, and automated workflows at a rate that outpaces governance. This creates a specific, measurable risk: when autonomy is undefined, it defaults to whatever the tool does by default. That is not a governance posture.

Research on large-scale repository data reveals a speed-quality inversion: organizations that adopt AI code generation at high velocity without corresponding oversight show increased code duplication and declining quality indicators over time.[^GitClear25] The DORA 2025 State of AI-Assisted Software Development confirms that quality outcomes from AI workflow adoption correlate with oversight design, not with adoption volume.[^DORA25] Teams that adopt faster without better governance get more output and more risk.

[^GitClear25]: GitClear, "Coding on Copilot: 2023–2025 GitHub Copilot's Impact on Code Quality," GitClear Research, 2025. Analysis of over 200 million lines of code finds that AI-assisted repositories show increased code churn and duplication rates correlated with reduced human review intensity.

[^DORA25]: DORA Team, *2025 State of AI-Assisted Software Development*, Google, 2025. Research on AI adoption patterns, quality impacts, and team performance across thousands of engineering organizations.

In DoW, quality failures in mission-critical software carry consequences that differ fundamentally from commercial failures: operational impact, personnel safety, mission degradation. The emphasis on human accountability and guardrail precision throughout this play is calibrated to that context, not only to software quality metrics.

This play provides the governance vocabulary and tooling that teams need to adopt AI workflows deliberately, with explicit autonomy decisions, precise behavioral constraints, and oversight designs matched to actual interaction patterns.

---

## **2. Purpose and Scope**

This play defines how DoW software teams should design and govern AI workflows across the software development lifecycle. It establishes a task-level autonomy framework, defines patterns for agent persona design, provides guidance for synchronous and asynchronous workflow governance, and connects to the architectural patterns in the AI Autonomy Continuum.

**In Scope:**

- Task-level autonomy governance (what an AI agent may do on a specific task)
- Agent persona design: structure, contents, and version control practices
- Synchronous and asynchronous workflow governance
- Multi-agent workflow governance and the human team lead role
- Decision frameworks for autonomy level and workflow mode selection
- Security risks specific to AI workflow deployment

**Out of Scope:**

- Specific AI tool selection or vendor recommendations
- Model training, fine-tuning, or foundational model governance
- Workforce development and AI skills curricula (see forthcoming companion plays)

**Target Audience:**

- Software engineering teams in DoW software factories
- Technical leads and DevSecOps practitioners responsible for AI workflow design
- Government technical leads and CORs responsible for oversight of AI-augmented contractor work
- Program managers evaluating AI adoption risk

This play addresses task-level and workflow-level autonomy governance. Component-level, subsystem-level, and enterprise architecture-level autonomy governance is a program and acquisition responsibility. Teams operating components that integrate into a larger system of systems should surface their task-level autonomy decisions upward for program-level architecture review. Future AI4SDLC guidance will address that layer as governance policy matures.

### 2.1 DoW Deployment Context

DoW teams operate under specific constraints that shape how this play's guidance applies in practice.

**RMF and ATO boundaries**: AI tools used in DoW workflows require authorization under the Risk Management Framework (DoDI 8510.01).[^DoDI8510] AI-generated code artifacts, agent behavior files, and automated pipeline steps must fit within the system's ATO boundary, or trigger a formal ATO update. The emerging continuous ATO model creates new possibilities for iterative AI tool integration, but also requires continuous monitoring capability that many teams are still building. This play assumes AI tooling is operating within an authorized boundary. Teams that have not yet authorized their AI tools should begin there before implementing any pattern in this play.

[^DoDI8510]: Department of Defense, "Risk Management Framework (RMF) for DoD Systems," DoDI 8510.01, July 2017 (change 3, May 2023). [Online]. Available: <https://www.esd.whs.mil/Portals/54/Documents/DD/issuances/dodi/851001p.pdf>

**Impact level governance**: Autonomy levels do not carry the same risk at all Impact Levels. A Level 3 autonomy grant in an IL-2 environment is materially different from the same level in an IL-5 or IL-6 environment. Recommended starting posture: teams operating above IL-2 should default to autonomy levels 0–1 until governance is established and tested against their specific data categories. AI-generated outputs should not interact with classified or CUI data until guardrails are validated for that data type.

Teams should also verify whether the deployment environment's AI policy imposes constraints on the development environment: not just tool authorization, but data handling and artifact provenance requirements. A team developing at IL-2 and deploying to IL-5 faces IL-5 constraints on the artifacts it produces, regardless of where it produces them.

**SBOM and AIBOM chain of custody**: AI-generated code complicates software bill of materials attestation. When an agent generates or substantially modifies code, the provenance of that code differs from human-authored code. DoDI 8430.AA (draft) introduces AI Bill of Materials (AIBOM, a record of AI models, training data, and behavioral definitions used in software development) requirements that will apply to AI-augmented software factories.[^AIBOM] AIBOM scope extends beyond code. AI-generated test artifacts, configurations, infrastructure definitions, and documentation are SDLC deliverables subject to provenance tracking. Teams should treat any AI-generated artifact that enters a controlled repository or delivery package as a supply chain event. SBOM and AIBOM chain of custody applies from the point of AI artifact generation at the lower IL, not only at the point of deployment.

[^AIBOM]: Department of Defense, "Software Bill of Materials and AI Bill of Materials," DoDI 8430.AA (draft), 2025. Introduces AIBOM tracking requirements alongside traditional SBOM for AI-augmented software development contexts.

**Government and contractor accountability**: The principle that the human holds accountability takes a specific form in DoW. For government-contractor teams, the accountability chain runs through contracting officers, Contracting Officer's Representatives (CORs), program managers, and government technical leads, not the individual contractor developer. Review gate design should reflect this. Approval authority for AI-generated artifacts should align with the program's government oversight structure, not just the nearest available technical reviewer. Programs should establish explicit policy specifying which approval roles must be filled by government personnel versus contractor personnel for AI-generated work at each autonomy level.

Autonomy decisions at the product or sub-component level may need to be tracked in the program's enterprise architecture and surfaced in a risk management dashboard, not just at the task or team level. Future AI4SDLC guidance will address that layer as governance policy matures.

**Software factory integration**: Most DoW development teams operate within an established software factory. Examples span the services and DISA: Platform One and Kessel Run (USAF), Kobayashi Maru (USSF), Army Software Factory, Black Pearl and The Forge (Navy), Marine Corps Software Factory, and DISA Citadel. The DoD Software Factory Coalition includes nearly 50 factories department-wide. This play is tool-agnostic by design, but the reference implementation in Section 5.6 should be validated against a factory's approved toolchain and ATO before adoption.

---

## **3. Prerequisites and Readiness**

Before applying AI workflow patterns, teams should establish the following foundational conditions:

- [ ] **Approved AI environment**: AI tools and models approved for the relevant impact level (for example, AWS Bedrock on GovCloud for IL-4)
- [ ] **Foundational plays reviewed**: Team has read [Fundamentals for Building an AI-Augmented Toolchain](fundamentals-play.md), [AI Autonomy Continuum](ai-autonomy_continuum_play.md) (RC), and [Human-Machine Interaction Patterns](human-machine-patterns.md) (GA). The AI Autonomy Implementation Guide (Beta) provides pattern-specific deployment checklists for teams ready to operationalize individual Continuum patterns.[^AIAutoImpl]
- [ ] **Data classification boundaries defined**: Clear rules established on what data categories can be shared with AI systems, and which require redaction or exclusion
- [ ] **CI/CD pipeline established**: Event-driven automation infrastructure exists for asynchronous workflow integration
- [ ] **Version control for behavior files**: Agent personas and system prompts stored in version-controlled repositories, with access controls and change review processes applied
- [ ] **Human review gates defined**: Approval checkpoints established for AI-generated artifacts before they reach production, with explicit criteria for what constitutes an acceptable output
- [ ] **Guardrail policy drafted**: Initial data handling and output restrictions documented in writing; automated enforcement mechanisms should be included where possible. Relying solely on human discipline to prevent runaway token consumption or unauthorized data exposure is insufficient as a sole control.
- [ ] **Approved AI tools deployed in end-user baselines**: AI tools and models are licensed and provisioned in developer workstations or end-user environments, not only in cloud environments. Teams should verify that tool access extends to the environments where work actually happens.
- [ ] **Access and capacity governance defined**: API key access controls are established and token consumption limits are set per user, team, and workflow. Unlimited token access without oversight is an operational and budget risk, not just a governance concern.

[^AIAutoImpl]: AI4SDLC Working Group, "AI Autonomy Implementation Guide" (Beta), 2026. Provides pattern-specific deployment checklists for Continuum Patterns 1 and 2. Multi-pattern operations guidance covering Parts II and III is forthcoming.

---

## **4. Guiding Principles**

| **Principle** | **Expected Practice** |
| ------------- | --------------------- |
| **Human Accountability** | The human who reviews and approves AI-generated work is accountable for it. AI does not hold accountability. The reviewer does. |
| **Explicit Autonomy** | Autonomy granted to AI agents should be explicitly defined per task, not assumed. Default to the most restrictive level appropriate for the work. |
| **Guardrail Precision** | Vague behavioral instructions fail. Guardrails should use specific, mandatory language rather than conditional or approximate guidance. |
| **Behavior as Code** | Agent persona files, behavior contracts, and system prompts should be version-controlled and reviewed with the same care as application code. |
| **Data Protection** | Do not expose classified, CUI, or sensitive system metadata (IP addresses, hostnames, API keys) to AI systems. Validator agents should enforce redaction programmatically. |
| **Sync vs. Async Awareness** | Synchronous workflows require real-time human judgment. Asynchronous workflows require pre-defined review gates. Design governance to match the interaction mode, not just the capability. |
| **Approved Environments** | Use only AI tools approved for your classification level. Verify model availability within approved services before deploying workflows. |
| **Attack Surface Awareness** | Agent behavior files are attack vectors. Treat them as security artifacts: restrict write access, validate provenance, and monitor for anomalous behavior. |
| **Tenure-Aware Review** | Do not assume that code review experience transfers automatically to AI output review. Establish explicit review criteria applicable regardless of reviewer seniority. |
| **Human as Team Lead** | In multi-agent and swarm contexts, the human's role shifts from reviewer of individual outputs to leader of a coordinated AI workforce. The skills required (objective-setting, outcome evaluation, and exception handling) are distinct from those needed for single-agent oversight. |
| **Outcome-Level Governance** | At the swarm level, governance cannot operate action-by-action. Define expected outcomes, observable success criteria, and escalation conditions before execution begins. Govern by what the agent team produces, not by every step it takes. |
| **Mixed-Mode Review** | Human review of AI-generated artifacts at scale requires AI-assisted sense-making tools targeted to the domain. Reviewing hundreds of AI-generated tests by inspection alone does not constitute meaningful assurance. Teams should define the metrics and tools they will use to evaluate AI output quality before deploying high-volume AI workflows. See Appendix A for AI-generated test quality metrics. |

---

## **5. Core Patterns and Practices**

### 5.1 The Task-Level Autonomy Framework

The [AI Autonomy Continuum](ai-autonomy_continuum_play.md) answers the architectural question: "What kind of AI workflow have we designed?" This task-level framework answers the operational question: "What is this agent permitted to do right now, on this specific task?" They operate at different levels and are designed to be used together. The Continuum establishes the envelope; this framework governs the behavior within it.

The six-level framework gives teams a shared vocabulary for making autonomy decisions explicit and communicating them across roles. An autonomy level is not a team-level or system-level setting. It is a per-task designation that can change within a single developer session. A developer can invoke Level 0 for sensitive refactoring and Level 2 for routine test generation in the same session. The key requirement is that the decision is made explicitly, not assumed or deferred.

| **Level** | **Label** | **Execution Mode** | **What the AI May Do** | **Human Role** |
|-----------|-----------|-------------------|------------------------|----------------|
| 0 | Suggest | Synchronous | Provide advice only. No file edits, no command execution. | Reads; decides independently. |
| 1 | Draft Artifacts | Synchronous | Draft text, code snippets, plans, checklists. Human applies changes. | Reviews draft; applies changes manually. |
| 2 | Propose Changes | Either | Produce a diff or patch proposal plus a test plan. Human reviews and applies. | Reviews diff and test plan; approves or rejects. |
| 3 | Bounded Execution | Either | May execute commands within an explicitly defined, constrained environment (sandbox, isolated namespace). Scope boundaries and rollback conditions must be defined before execution. | Monitors execution; reviews outputs; holds rollback authority. |
| 4 | Create PR/MR | Asynchronous | May open a pull or merge request with changes and supporting evidence. Cannot merge. | Reviews PR/MR; holds merge authority. |
| 5 | Autonomous Merge/Deploy | Asynchronous | Not recommended for most environments. Requires formal governance and executive authorization. | Oversight via monitoring and exception handling. |

**Recommended baseline**: Level 1–2 for broad adoption, with mandatory human review before changes are applied. Teams operating above IL-2 should default to levels 0–1 until governance is established and tested against their specific environment.

> **Working Group Note:** The task-level autonomy framework in this section is under active review. A working group discussion is in progress to evaluate whether the six-level scale should be restructured to better reflect asynchronous AI workflows and align with comparable industry frameworks. The current framework is suitable for adoption; teams should expect refinement in a future revision.

### 5.2 Agent Persona Patterns

An agent persona is a version-controlled behavior definition file specifying an AI agent's role, behavioral constraints, output format, escalation conditions, and data handling rules. It is not a prompt. It is a standing behavioral contract: written in human-readable Markdown, stored in version control, and designed to be tool-agnostic. Persona files work across GitLab Duo, Claude Code (CLAUDE.md files), Roo/Cline, Continue.dev, and other compliant AI development toolchains.

A well-constructed persona file should include: a clear role description, behavioral constraints written in mandatory language ("you must not" rather than "try to avoid"), a defined output format, explicit escalation conditions, and data handling rules including redaction requirements. Vague persona definitions produce inconsistent agent behavior. The precision requirement for guardrails applies equally to persona design.

Common SDLC persona types include:

| **Persona** | **Purpose** |
|-------------|-------------|
| **Code Reviewer** | Analyzes code for defects, standards violations, and security risks |
| **Software Developer** | Generates, refactors, or modifies code within explicitly defined scope boundaries. Autonomy level should be set per task context. Code generation without human review before commit is not recommended. Pair with Code Reviewer and Validator personas in a sequential generation-review-validation workflow before human approval. |
| **DevSecOps Control Mapper** | Maps code and configurations to applicable security controls |
| **Requirements Analyzer** | Decomposes requirements into acceptance criteria or user stories |
| **Test Planner** | Generates test strategies, test cases, and coverage analysis |
| **Security Analyst** | Identifies vulnerabilities, threat vectors, and misconfigurations |
| **Red Team Agent** | Generates adversarial test cases and boundary conditions |
| **Validator** | Cross-checks outputs from other personas for accuracy and policy compliance |
| **Orchestrator** | Coordinates task sequencing across multiple personas |

The Validator persona deserves dedicated attention. It serves as a quality and policy gate on outputs from other personas before those outputs reach human review. In a well-designed multi-persona workflow, no output from a generating persona reaches the human reviewer without first passing a Validator check. This creates a layered defense: behavioral guardrails in each generating persona, Validator review of outputs, and human approval at the gate. No single layer is sufficient alone.

**Scenario:** A developer invokes a Test Planner persona at Level 2 to generate a test plan for a new module. The Test Planner produces a diff with a proposed test suite and a coverage summary. A Validator persona reviews the output, checks that no IP addresses or credentials appear in test fixtures, verifies that coverage claims are internally consistent, and flags one test case as out of scope per the Test Planner's scope constraint. The developer receives the Validator-reviewed output and makes the final decision on what to apply.

The AI4SDLC Working Group is developing a reference set of agent persona templates, skill files, and guardrail templates as part of an upcoming resource library. When available, these artifacts will be published for DoW teams to adapt and deploy. See the AI4SDLC resources section for availability updates.

### 5.3 Synchronous vs. Asynchronous Workflow Design

Synchronous workflows keep the developer present in real time. The interaction happens in an IDE or chat interface, with a tight human-AI loop and a natural checkpoint at every response. Autonomy levels 0–2 are appropriate for most synchronous interactions. The primary governance risk is cognitive overload: as AI output volume increases, developers under time pressure may approve outputs without meaningful evaluation. Volume is not assurance.

Asynchronous workflows run while the developer is absent. They are triggered by CI/CD events, operate autonomously through a defined workflow, and surface results for human review after execution completes. Levels 1–4 may apply, depending on task type and governance maturity. The primary governance requirement is explicit review gate design. Review gates that are assumed rather than designed are review gates that will fail.

The five Human-Machine Interaction (HMT) patterns from the [Human-Machine Interaction Patterns](human-machine-patterns.md) play are interaction surfaces. Workflow mode is a governance dimension. Any surface can support multiple workflow modes. The governance requirement is determined by the human's presence during execution, not by the interface surface. When an IDE-embedded tool runs a background agent task, the same asynchronous governance requirements apply (defined review gates, audit trails, approval criteria) even though the developer is still "in the IDE."

| **HMT Interaction Pattern** | **Primary Mode** | **Also Supports** | **Typical Autonomy Levels** | **Primary Governance Concern** |
| --------------------------- | ---------------- | ----------------- | --------------------------- | ------------------------------ |
| Standalone Web Interfaces | Synchronous | N/A | 0–1 | No traceability; out-of-band use; data exposure risk |
| IDE Plugins and Adapters | Synchronous | N/A | 0–2 | Prompt versioning; limited shared oversight |
| AI-First IDEs / Workspaces | Synchronous | Asynchronous (background agent tasks) | 1–4 | Human may not be present during agent execution; review gates required even within the IDE |
| Custom API Integrations | Asynchronous | N/A | 2–4 | Review gate design; audit trail completeness |
| Agentic Platforms | Agentic / Orchestrated | Asynchronous; Continuous (advanced) | 3–5 (governed) | Emergent behavior; trust calibration; formal governance required |

| **Factor** | **Synchronous** | **Asynchronous** |
|------------|-----------------|------------------|
| Human presence | Real-time | Post-hoc |
| Oversight model | Continuous | Checkpoint-based |
| Appropriate autonomy levels | 0–2 | 1–4 (with defined gates) |
| Primary risk | Cognitive overload; rubber-stamping | Insufficient review gates; audit trail gaps |
| Governance priority | Guardrail precision | Gate design and approval criteria |

As asynchronous AI output volume increases, human review capacity becomes the constraint. Monitor review-to-output ratios. If reviewers cannot keep pace with output volume, the human-in-the-loop provides compliance theater, not assurance.

### 5.4 Designing Guardrails That Hold

A guardrail is a behavioral constraint in an agent persona or system prompt defining what an AI agent may and may not do. The effectiveness of a guardrail depends almost entirely on its specificity. Vague or conditional language produces inconsistent results. Teams that believe they have guardrails because they have written behavioral guidance have not necessarily built guardrails that hold under operational conditions.

The difference between effective and ineffective guardrails is precision:

- **Fails:** "Try not to include sensitive information in your outputs."
- **Works:** "You must not output IP addresses, hostnames, or user credentials under any circumstances. If any token matches the pattern `\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}`, replace it with `[REDACTED-IP]` before outputting."

Categories of guardrails that every persona should address: data exposure prevention (PII, credentials, hostnames, API keys), output format constraints, scope boundaries (what topics, systems, or actions are out of bounds), and escalation triggers (conditions under which the agent must surface a decision to the human rather than proceeding).

Guardrails require testing. Create synthetic test artifacts containing known-sensitive patterns and verify agent behavior before deploying to production workflows. Build these as pipeline tests alongside application code, not as one-time manual checks. A guardrail that has not been tested against adversarial input is a guardrail whose effectiveness is unknown.

The layered approach is: persona-level guardrails (behavioral constraints in the definition file) plus Validator persona review of outputs plus pipeline-level automated scanning. No single layer is sufficient. Defense in depth applies to agent behavior as much as to network architecture.

### 5.5 Multi-Agent Systems and the Human Team Lead

This section addresses governance and human accountability in multi-agent workflows. Architectural design of orchestration platforms (technology selection, inter-agent communication, agent state management) is out of scope here. For that layer, see [AI Autonomy Continuum](ai-autonomy_continuum_play.md) Patterns 3 and 4.

In single-agent workflows, the human evaluates individual outputs: code diffs, test plans, security findings. In multi-agent systems, a coordinating orchestrator decomposes a goal into subtasks, assigns them to specialized agents, and aggregates results. The human interacts primarily at the objective and outcome layers, not at every intermediate step. This is not a reduction in accountability. It is a shift in where that accountability is exercised.

Two coordination patterns are in use:

| **Pattern** | **Description** | **Human Role** | **Autonomy Levels** | **Governance Approach** |
| ----------- | --------------- | -------------- | ------------------- | ----------------------- |
| **Orchestrated swarm** | A designated Orchestrator agent decomposes tasks and coordinates specialized agents. Workflow is explicit and auditable. | Sets objective; reviews orchestrator plan; approves outcomes; handles escalations. | Levels 3–4 with defined gates | Orchestrator persona contract; outcome-level review; full audit trail |
| **Emergent swarm** | Agents self-organize around a shared goal without a fixed orchestrator. Coordination arises from agent interaction. | Sets objective and boundaries; monitors for anomalous behavior; intervenes when agents diverge. | Levels 4–5; requires formal governance | Observable outcome criteria; behavioral monitoring; defined intervention triggers |

Orchestrated swarm is the recommended default. Emergent swarm patterns should be reserved for teams with established multi-agent governance and demonstrable calibrated trust in agent behavior at scale.

Leading an agent team is a distinct capability from using an AI tool or reviewing individual AI output. The human team lead role requires: translating mission requirements into agent-executable goals with clear success criteria and explicit boundaries; assessing whether the agent team achieved the objective correctly (not just whether individual outputs look plausible); recognizing when agents have diverged, are looping, or have produced outputs requiring course correction; knowing when to expand agent autonomy and when to tighten it based on observed performance; and reading multi-agent execution logs to reconstruct what happened and why.

Governance at the orchestration layer requires more than individual agent guardrails. The Orchestrator persona requires additional governance: define acceptable task decomposition boundaries (what the orchestrator may and may not assign to sub-agents); establish escalation triggers specifying conditions under which the orchestrator must surface a decision to the human; require outcome summaries in human-readable form; and require that every agent action, sub-agent spawn, and tool call is logged with the originating objective and orchestrator intent.

**Scenario:** A technical lead assigns a feature development objective to an agent team. The Orchestrator decomposes it into requirements analysis, code generation, test planning, and security review subtasks. Specialized agents execute each in sequence. The Orchestrator aggregates results and flags one security finding for human decision. The human evaluates the summary and approves the PR with one modification. Total human interaction: objective-setting and one exception decision.

These skills are the subject of the forthcoming companion play: **Leading Teams of AI Agents** *(ai-agent-team-leadership.md)*.

### 5.6 The Reference Implementation

Teams beginning AI workflow adoption face a blank-page problem: standing up governance from scratch is time-consuming, and most teams do not know what a well-structured persona file or autonomy contract looks like until they have one in front of them.

The reference implementation addresses this directly. It is a starter set of governance artifacts teams can download, adapt, and deploy: an autonomy levels contract (the Level 0–5 framework as a standalone Markdown file), starter persona files (Validator, Code Reviewer, and Test Planner as a minimum viable set), a sample orchestrator configuration, and data protection guardrail templates.

These artifacts are designed to be tool-agnostic Markdown files that work with any compliant AI development toolchain. The publishing path is via code.mil as a versioned, open-contribution repository. Teams that develop effective persona files or improve the reference set should submit refinements through the AI4SDLC Working Group, applying the same open-source collaboration model DoW already applies to software.

The AI4SDLC Working Group is developing a reference set of agent persona templates, skill files, and guardrail templates as part of an upcoming resource library. When available, these artifacts will be published for DoW teams to adapt and deploy. See the AI4SDLC resources section for availability updates.

**Scenario:** A team downloads the reference implementation, adapts the Validator persona to add their organization's specific data handling rules and redaction patterns, runs guardrail tests against a synthetic test corpus, and deploys the Validator to their pipeline as a pre-review gate in under a sprint.

---

## **6. Decision Framework**

### 6.1 Selecting the Right Autonomy Level

| **Task Type** | **Recommended Level** | **Rationale** |
|---------------|-----------------------|---------------|
| Exploratory code review or learning | 0: Suggest | Developer needs context, not action. No changes should be applied. |
| Test case generation | 1–2: Draft or Propose | High volume output; human validates coverage and logic before applying. |
| Security vulnerability analysis | 1–2: Draft or Propose | High stakes; human validates every finding before acting on it. |
| Documentation generation | 2: Propose | Structured output; human validates accuracy and applies. |
| Dependency update (routine) | 4: Create PR/MR | Well-defined task with observable outcome; human holds merge authority. |
| Automated pipeline test execution | 3: Bounded Execution | Defined inputs and outputs in isolated environment; human reviews results. |
| Production deployment | 4 at most; prefer human-driven | Human authorization required for production changes. |
| Multi-agent orchestrated workflow | 3–4 with defined gates | Orchestrator plan requires human review; outcomes require human approval. |

### 6.2 Workflow Mode Selection

Use this table to determine whether a workflow should be synchronous or asynchronous, and what governance elements are required:

| **Use Case** | **Recommended Mode** | **Autonomy Levels** | **Key Governance Requirement** | **When to Escalate** |
| ------------ | -------------------- | ------------------- | ------------------------------ | -------------------- |
| Developer using AI for code completion in IDE | Synchronous | 0–2 | Prompt versioning; peer review of AI-accepted suggestions | When suggested changes touch authentication, encryption, or mission-critical paths |
| CI/CD pipeline running AI-assisted test generation on merge request | Asynchronous | 2–3 | Explicit review gate before merge; Validator persona check on outputs | When test coverage claims cannot be independently verified or when coverage drops |
| Nightly AI-assisted security scan with findings routed to backlog | Asynchronous | 1–2 | Defined triage criteria; findings require human disposition before closure | When a critical or high finding is generated; when scan anomalies occur |
| AI agent opening dependency update PRs | Asynchronous | 4 | Human holds merge authority; defined approval criteria in PR template | When dependency change touches a security library or introduces a license conflict |
| Multi-agent sprint workflow: requirements to code to test to review | Asynchronous | 3–4 with gates | Orchestrator plan reviewed by human before execution; outcome-level human approval | When any agent flags an exception; when orchestrator deviates from approved plan |
| Developer querying AI for architecture guidance in a chat interface | Synchronous | 0 | No traceability requirement at Level 0; recommend keeping AI-influenced decisions in ADRs | When the guidance will be applied to a decision with security or compliance implications |
| Automated documentation generation from source code on commit | Asynchronous | 2–3 | Spot-check review process; defined exception handling for anomalous outputs | When generated documentation contains content not traceable to the source artifact |

### 6.3 Continuum Pattern to Autonomy Level Mapping

Teams arriving from the AI Autonomy Continuum play can use this table to identify the task-level autonomy levels and governance requirements appropriate for each architectural pattern.

| **Continuum Pattern** | **Description** | **Task-Level Autonomy Levels** | **Governance Requirements** |
| --------------------- | --------------- | ------------------------------ | --------------------------- |
| **Pattern 1: Assistive Tools** | AI provides suggestions and completions at the developer's request. Human applies all changes. No autonomous execution. | Levels 0–1 | Approved tool authorization; prompt logging recommended; human applies all changes manually. No automated pipeline integration at this pattern. |
| **Pattern 2: Delegated Agents** | AI agents execute bounded tasks with human-defined scope and review gates. Agents may propose changes, open PRs, or execute in sandboxes with explicit authorization. | Levels 1–4 with defined gates per task | Persona files required for each agent role; Validator pattern recommended; explicit review gates for each gate point; audit trail required for all agent actions; escalation conditions documented. |
| **Pattern 3: Orchestrated Systems** | Multiple coordinated agents execute complex workflows. An Orchestrator decomposes objectives, delegates to specialized agents, and aggregates results. | Levels 3–4 for sub-agents; Level 4 for orchestrator PR creation | Orchestrator persona with defined decomposition boundaries; human review of orchestrator plan before execution; outcome-level human approval; full audit trail including sub-agent interactions; defined escalation triggers. |
| **Pattern 4: Adaptive Ecosystems** | Agents adapt behavior based on context and history. Emergent coordination may occur. Reserved for teams with demonstrable multi-agent governance and executive authorization. | Levels 4–5 only, with formal governance and executive authorization | Formal governance documentation required; executive authorization on record; continuous behavioral monitoring; observable outcome criteria defined in advance; defined intervention triggers and rollback conditions; regular governance review cadence. |

---

## **7. Implementation Guidance**

Teams new to AI workflow governance should adopt an incremental approach. The goal is to establish governance practices before expanding autonomy, not to build a comprehensive framework before deploying anything.

**Step 1: Define your autonomy baseline.** Before deploying any AI workflow tooling, establish the organizational default level. Start with Level 1–2 for all developers. Document exceptions and the governance conditions that apply to each exception. This is a one-page document, not a multi-month policy process.

**Step 2: Build your first persona file.** Start with the Validator persona. It provides immediate value by checking outputs for data exposure and policy compliance, requires no system integration beyond a text file in version control, and is testable before it touches any production workflow. Write it with mandatory language. Test it against synthetic inputs before relying on it.

**Step 3: Identify a bounded pilot workflow.** Select one workflow (test case generation and code review are common starting points) and implement AI assistance at Level 1 or 2. Establish your baseline metrics for code quality, review time, and defect rate before expanding. You cannot measure improvement without a baseline.

**Step 4: Design review gates explicitly.** For any asynchronous AI step in your pipeline, document: what outputs require human approval, what criteria define an acceptable output, and who holds approval authority. These gates should appear in pipeline configuration and team working agreements. Assumed gates are not gates.

**Step 5: Validate your guardrails.** Test persona guardrails with synthetic artifacts containing known-sensitive patterns before relying on them in practice. Add guardrail validation tests to your pipeline alongside application tests. A guardrail that has not been tested is a guardrail whose reliability is unknown.

**Step 6: Expand and contribute.** Once the pilot workflow is stable and measured, expand to additional personas or higher autonomy levels where risk tolerance and governance support it. Share effective persona patterns with the AI4SDLC Working Group.

---

## **8. Key Considerations**

### Risks

**Prompt Injection via Agent Behavior Files**
Prompt injection is an attack vector in which malicious instructions embedded in input data redirect an AI agent's behavior outside its intended boundaries.[^ATLAS] Adversaries can embed malicious instructions in agent behavior files, tool outputs, or data sources that redirect AI actions outside intended boundaries. Documented attacks have used agent instruction sets to orchestrate unauthorized system access, lateral movement, and data exfiltration.[^Anthropic26] Treat persona files as security artifacts: restrict write access, validate file provenance on load, and monitor agent behavior against expected patterns.

[^ATLAS]: MITRE Corporation, "ATLAS: Adversarial Threat Landscape for AI Systems," MITRE ATLAS Knowledge Base. [Online]. Available: <https://atlas.mitre.org/>. Documents adversarial attack patterns targeting AI systems including prompt injection, model evasion, and supply chain attacks.

[^Anthropic26]: Anthropic, "Disrupting the First Reported AI-Orchestrated Cyber Espionage Campaign," Anthropic Security Research, 2025. [Online]. Available: <https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf>. Documents cases of weaponized agent instruction sets used for unauthorized system access and lateral movement in AI-augmented development environments.

**Governance Lag**
AI tooling adoption is outpacing governance development across DoW. Teams establish practices in the absence of policy, and those informal practices become the de facto standard. The autonomy level framework in Section 5.1 provides a lightweight starting point teams can formally adopt without waiting for comprehensive policy.

**Review Capacity Saturation**
As AI-generated output volume increases, human review capacity becomes the binding constraint. Approving outputs without meaningful evaluation converts the human-in-the-loop into compliance theater. Monitor review-to-output ratios. Establish review SLAs. Apply Validator personas to automate secondary checks where appropriate.

**Access Control to AI-Augmented Systems**
AI tools with repository, pipeline, or operational system access inherit significant privilege. Define access boundaries for AI agents the same way you would for service accounts: least privilege, explicitly scoped, regularly reviewed, and audited.

**Speed-Quality Inversion**
Observable trends in large-scale repository data show increased code duplication and decreased quality indicators alongside AI adoption, particularly when human oversight is thin.[^GitClear25] Monitor quality metrics alongside velocity when evaluating AI workflow impact. Speed without quality is not a mission outcome.

**Shadow AI**
Developers using unapproved AI tools outside organizational visibility introduce data exposure and governance risks without leaving an audit trail. The autonomy framework only works if the tools themselves are governed. See [AI Autonomy Continuum](ai-autonomy_continuum_play.md) for Pattern 1 shadow AI risk discussion.

**Emergent Behavior in Swarm Systems**
Multi-agent systems can produce behaviors that no individual agent was designed to produce. When agents interact, share state, or spawn sub-agents, the collective behavior can diverge from intent in ways not apparent until outcomes are reviewed. Prefer orchestrated over emergent swarm patterns. Define observable success criteria before execution begins.

**Cascading Failures in Orchestrated Workflows**
A flawed output from one agent can propagate through the workflow as downstream agents treat it as valid input. Unlike a single-agent failure caught by one human reviewer, a cascading failure may compound across multiple steps before surfacing. Design inter-agent validation checkpoints. Require the Orchestrator to flag low-confidence handoffs for human review. Test multi-agent workflows against known-bad inputs before production deployment.

**Audit Trail Complexity at Scale**
In swarm systems, the audit trail grows with the number of agents and interactions. Actions taken by sub-agents spawned by other agents may be difficult to trace back to originating human intent, creating accountability gaps and compliance risk. Require all agent actions to log the originating objective, the instructing agent, and the tool or resource accessed. Treat the audit trail as a first-class artifact alongside code.

**Human Skill Gap in the Team Lead Role**
As the human role shifts from individual output reviewer to agent team lead, the required skills change substantially. Teams that expand to multi-agent workflows without developing objective-setting, outcome evaluation, and exception-handling capabilities risk governance failures that task-level review would have caught. Develop human skills in parallel with expanding agent autonomy. See the forthcoming companion: **Leading Teams of AI Agents**.

### Key Metrics

Establish baselines before AI workflow adoption, then track trends. Measure quality and security alongside velocity from the start.

#### Workflow Health

- Autonomy level distribution: what levels are teams actually using in practice?
- Review-to-AI-output ratio: are humans reviewing in proportion to output volume?
- PR/MR rejection rate for AI-generated changes
- Time from AI output generation to human disposition

#### Code Quality

- Defect escape rate before and after AI workflow adoption
- Security finding rate per 1,000 lines from static and dynamic analysis
- Code duplication percentage over time
- Post-merge rework rate on AI-assisted contributions
- AI-generated test quality: assertion density (meaningful assertions per test line), hallucinated reference rate (tests referencing functions or classes that do not exist), and mutation score delta (change in mutation test coverage) are practical early signals. Establish baselines before scaling AI test generation. See [AI-Augmented Testing](testing-play.md) for deeper test quality guidance.

#### Security

- Guardrail validation pass rate from synthetic test artifacts
- Anomalous agent behavior incidents per quarter
- Percentage of AI-generated artifacts passing through a Validator persona before reaching human review
- AIBOM completeness rate for AI-generated artifacts entering controlled repositories

> **Series note:** A series-wide metrics framework covering all AI4SDLC play domains is a candidate for a future resource document. The metrics above are scoped to workflow governance. Plays addressing testing, documentation, and code generation each carry domain-specific metrics; a future resource will provide a unified measurement approach across the full lifecycle.

---

## **9. Key Takeaways**

- **Autonomy is a choice, not a default.** Define what agents are permitted to do before deploying them. An undefined autonomy level defaults to whatever the tool does, which is not a governance posture.
- **Guardrails work when they are precise.** Vague behavioral instructions produce inconsistent results. Write guardrails with mandatory language and test them against adversarial inputs before relying on them.
- **Persona files are security artifacts.** Apply version control, access control, and provenance validation to agent behavior definitions with the same rigor applied to application code.
- **The human holds accountability.** AI can draft, propose, execute, and generate. Responsibility for what reaches production belongs to the human who reviewed and approved it, not to the AI that produced it.
- **Synchronous and asynchronous workflows need different governance.** The interface does not determine the governance requirement; the human's presence during execution does. Design oversight for the interaction mode.
- **Speed is not the measure of success.** Monitor quality and security metrics alongside velocity to catch speed-quality inversions before they compound into mission risk.

---

## **10. Companion Plays and References**

### Related Plays

- [Fundamentals for Building an AI-Augmented Toolchain](fundamentals-play.md)
- [AI Autonomy Continuum](ai-autonomy_continuum_play.md): Strategic architectural patterns (Patterns 1–4); this play is the operational complement governing autonomy within those patterns
- [Human-Machine Interaction Patterns](human-machine-patterns.md): Defines the five ways humans interact with AI tools. This play governs autonomy and behavior within those interaction modes; the HMT play governs the interaction design itself. Read together.
- [AI-Augmented Testing](testing-play.md): Applying AI to test generation and analysis
- [AI-Assisted Documentation Across the SDLC](documentation-play.md): Documentation workflows and agentic orchestration
- [Leading Practices for Code Completion and Generation](code-gen-play.md)
- [Prompt Engineering](prompt-engineering.md)
- **Leading Teams of AI Agents** *(ai-agent-team-leadership.md, Forthcoming)*: Companion play addressing human skills required to lead coordinated AI agent teams: objective-setting, outcome evaluation, exception handling, audit trail literacy, and trust calibration at the team level.
- **Calibrated Trust and Oversight in GenAI-Augmented Systems** *(Forthcoming)*

### Key References

- DORA Team, *2025 State of AI-Assisted Software Development*, Google, 2025.[^DORA25]
- GitClear, "Coding on Copilot: 2023–2025 GitHub Copilot's Impact on Code Quality," 2025.[^GitClear25]
- MITRE Corporation, ATLAS: Adversarial Threat Landscape for AI Systems.[^ATLAS]
- Department of Defense, DoDI 8510.01, Risk Management Framework for DoD Systems.[^DoDI8510]
- National Institute of Standards and Technology, SP 800-218A, "Recommendations for Mitigating the Risk of AI Code Generation," Jun. 2024.[^NIST218A]
- Anthropic, "Disrupting the First Reported AI-Orchestrated Cyber Espionage Campaign," 2025.[^Anthropic26]

[^NIST218A]: National Institute of Standards and Technology, "Recommendations for Mitigating the Risk of AI Code Generation," NIST SP 800-218A, Jun. 2024. [Online]. Available: <https://csrc.nist.gov/pubs/sp/800/218/a/final>. Provides secure development practices for GenAI-assisted code generation and agent workflows.

### Emerging Guidance

- **DoDI 8430.AA (draft)**: Introduces AIBOM requirements alongside traditional SBOMs. As AI-assisted workflows become embedded in software factories, the models, training data, and behavioral definitions used will require traceable provenance documentation. Teams should begin tracking AI tool involvement in their software supply chain records before formal AIBOM requirements are codified.
- **DoD AI Cybersecurity Risk Management Tailoring Guide, v2 (2025)**: Maps AI-specific attack vectors using MITRE ATLAS; addresses the assistant-only model category and provides risk management tailoring guidance for AI tools operating within RMF boundaries.
- **12-Factor methodology for agentic systems**: Adapting established 12-factor application design principles (explicit dependency management, environment-specific configuration, stateless process design) to agent workflows is an emerging practitioner discussion. Teams building durable agent workflows should track this pattern.

---

<!--
### Architectural Intent

This play contributes to software health by establishing the governance layer that makes AI workflow adoption sustainable in mission-critical environments. By providing explicit autonomy vocabulary, precision guardrail design, and oversight patterns matched to interaction modes, it prevents governance-by-default (where autonomy is whatever the tool does) and replaces it with deliberate, accountable, auditable AI workflow practice. The decision frameworks, persona patterns, and reference implementation directly reduce the cost of adoption while preserving the human accountability that mission assurance requires.
-->
