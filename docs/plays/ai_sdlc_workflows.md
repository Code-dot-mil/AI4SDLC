---
title: "Play: AI Workflow Design and Governance"
lifecycle: alpha
last_updated: "2026-03-04"
---

# **Play: AI Workflow Design and Governance**

### *"Autonomy is earned, not assumed"*

> "The question isn't whether to automate — it's where the human stays in the loop." — AI4SDLC Working Group

---

> **Outline for Workgroup Review — March 2026**
>
> This is a structured outline for working group feedback. Section descriptions indicate intended content; they are not final prose. Sections marked **[FEEDBACK REQUESTED]** are where your input is most needed before authoring begins.
>
> **Feedback focus areas:**
> - Is the scope right? What's missing, what's out of place?
> - Does the autonomy level framework (Section 5.1) align with your team's practice?
> - Are the security risks in Section 8 complete for your context?
> - Are there additional patterns in Section 5 we should capture from the field?

---

## **Executive Summary (The Play in Brief)**

*Planned content: 2–3 paragraphs.*

- **Paragraph 1 — The problem**: AI is entering the DoW software workflow at multiple levels simultaneously — inside the IDE, within CI/CD pipelines, and through coordinated agent workflows. Teams are adopting faster than governance can keep pace. The result is inconsistent practice, expanding attack surfaces, and uncertainty about where human accountability begins and ends.

- **Paragraph 2 — What this play does**: Bridges the strategic framing of the [AI Autonomy Continuum](ai-autonomy_continuum_play.md) to the day-to-day decisions developers and DevSecOps teams actually face. Introduces a task-level autonomy framework that complements the Continuum's architectural patterns. Covers agent persona definition, guardrail design, synchronous vs. asynchronous workflow governance, and a reference implementation path.

- **Paragraph 3 — Key takeaway callout**: AI workflow automation is most effective when autonomy is explicitly defined, guardrails are precisely specified, and humans retain clear accountability for all outcomes.

📌 **Key takeaway:** *[Final phrasing TBD after workgroup review]*

---

## **1. Why This Play Matters**

*Planned content: 3–5 short paragraphs. Opens with the problem, closes with the opportunity.*

- **Speed-quality tension**: Velocity gains from AI-assisted development are real and measurable. So is the observable decline in code quality and security posture when human oversight is insufficient. The DORA 2025 report found increased code duplication and declining quality indicators in teams with high AI output volume and thin human review capacity.

- **Governance lag**: Tooling adoption is beginning to outrun governance across the DoW. Teams are establishing practices in the absence of policy and those practices calcify. This play provides a starting vocabulary that teams can adopt now, without waiting for comprehensive policy.

- **Expanded attack surface**: Agent behavior files, system prompts, and persona definitions are active components of the development environment. Adversaries have demonstrated the ability to weaponize these artifacts by directing agents to perform unauthorized actions, exfiltrate data, or execute lateral movement. These are documented, not theoretical, risks.

- **The gap this play fills**: The Autonomy Continuum play addresses architectural patterns at the system and team level. This play addresses the operational layer and what happens within a workflow, on a specific task, with a specific agent. Both are needed.

---

## **2. Purpose and Scope**

*Planned content: In-scope / out-of-scope lists, target audience.*

**Five workflow modes this play addresses:**

The [Human-Machine Interaction Patterns](human-machine-patterns.md) play defines five ways humans interact with AI tools — from standalone web interfaces to agentic platforms. This play builds on that foundation by addressing governance and autonomy *within* those interactions. The five workflow modes below form a progression from human-present to governance-by-exception, aligning with the four Continuum patterns.

**Important**: The workflow modes describe *how the human engages*, not *where the workflow runs*. Synchronous and asynchronous execution can both occur within an IDE. Tools like GitLab Duo and Claude Code support background agent execution that runs while the developer is doing other work. That execution is asynchronous regardless of the tooling surface. The HMT interaction patterns are the surfaces; the workflow modes are the governance dimensions. The relationship is many-to-many, not one-to-one.

1. **Synchronous workflows** — Real-time AI assistance where the developer is present, issuing prompts, and evaluating each response in sequence. *(HMT: any surface; Continuum: Pattern 1)*
2. **Asynchronous workflows** — AI-driven execution triggered by events or initiated as a background task. The human reviews outputs after the fact. Occurs in CI/CD pipelines *and* increasingly within IDE-embedded agent tools. *(HMT: any surface supporting background execution — Custom API Integrations most commonly, but also AI-First IDEs and Agentic Platforms; Continuum: Pattern 2)*
3. **Agentic workflows** — Multi-step, goal-directed behavior where a single agent selects and sequences actions toward a defined objective. Human checkpoints must be explicitly designed in. *(HMT: Agentic Platforms; Continuum: Pattern 2–3)*
4. **Orchestrated / swarm workflows** — Multiple AI agents working in coordination toward a shared objective. The human's role shifts from task reviewer to team lead: setting objectives, monitoring outcomes, and handling exceptions. Governance operates at the orchestration layer, not the individual action layer. *(HMT: Agentic Platforms — advanced deployment; Continuum: Pattern 3; see Section 5.5)*
5. **Continuous / adaptive workflows** — Always-on AI systems that monitor, analyze, and act without explicit human triggers. Human oversight operates by exception: defining success criteria, reviewing anomaly reports, and intervening when systems diverge from intent. Requires formal governance and executive authorization before deployment. *(HMT: Agentic Platforms — advanced deployment; Continuum: Pattern 4)*

**In Scope:**

- Task-level autonomy framework for individual and team use
- Agent persona definition using version-controlled behavior files
- Guardrail design, specification, and validation
- Synchronous vs. asynchronous governance design
- Security considerations specific to agent-driven workflows
- Reference implementation path for DoW adoption

**Out of Scope:**

| Topic | Where to Look | Status |
| --- | --- | --- |
| AI tool and vendor selection | [Fundamentals](fundamentals-play.md) | Published; forthcoming expansion covers agentic platform selection |
| Multi-agent platform architecture: inter-agent communication, agent state management, runtime infrastructure | [AI Autonomy Continuum](ai-autonomy_continuum_play.md) Patterns 3 and 4 | Published; inter-agent communication and state management detail is forthcoming |
| CI/CD pipeline design | Your software factory documentation | N/A |
| Governance policy framework | **Governance Play** *(cs_governance-play.md)* | Forthcoming |

Governance and human accountability *within* multi-agent workflows is in scope. See Section 5.5.

**Target Audience:** Developers and DevSecOps engineers, technical leads defining team-level AI policy, security engineers assessing agent workflow risk, program managers establishing adoption boundaries.

---

### 2.1 DoW Deployment Context

*Planned content: A brief orientation that surfaces the DoW-specific constraints shaping this play. A practical framing for the audience that includes relevant policy references. These factors appear throughout the play; they are consolidated here so readers understand why certain recommendations are scoped or weighted as they are.*

#### RMF and Authorization to Operate

- AI tools used in DoW workflows require authorization under the Risk Management Framework (DoDI 8510.01). AI-generated code artifacts, agent behavior files, and automated pipeline steps must fit within the system's ATO boundary — or trigger a formal ATO update.
- The emerging cATO (continuous Authorization to Operate) model creates new possibilities for iterative AI tool integration, but also requires continuous monitoring capability that many teams are still building.
- This play assumes AI tooling is operating within an authorized boundary. Teams that have not yet authorized their AI tools should begin there before implementing any pattern in Section 5.

#### Impact Level Governance

- Autonomy levels do not carry the same risk at all Impact Levels. Autonomy level 3 (Sandbox Execute) in an IL-2 environment is materially different from the same autonomy level in an IL-5 or IL-6 environment.
- Recommended starting posture: teams operating above IL-2 should default to autonomy levels 0–1 until governance is established and tested against their specific data categories. AI-generated outputs should not interact with classified or CUI data until guardrails are validated for that data type.
- *Author note: Section 6.1 should include a supplementary column or table mapping recommended autonomy level ceilings by impact level.*

#### Government / Contractor Accountability

- The principle that "the human holds accountability" takes a specific form in DoW. For government-contractor teams, the accountability chain runs through contracting officers, Contracting Officer's Representatives (CORs), program managers, and government technical leads and not the individual contractor developer.
- Review gate design should reflect this: approval authority for AI-generated artifacts should align with the program's government oversight structure, not just the nearest available technical reviewer.
- Programs should establish explicit policy specifying which approval roles must be filled by government personnel vs. contractor personnel for AI-generated work at each autonomy level.

#### Software Supply Chain and AIBOM

- AI-generated code complicates software bill of materials (SBOM) attestation. When an agent generates or substantially modifies code, the provenance of that code differs from human-authored code. DoDI 8430.AA (draft) introduces AI Bill of Materials (AIBOM) requirements that will apply to AI-augmented software factories.
- Teams adopting AI code generation workflows should begin tracking AI tool involvement in their software supply chain records now, before formal AIBOM requirements are codified. The reference implementation in Section 5.6 should include a starter AIBOM record template.
- AIBOM scope extends beyond code. AI-generated test artifacts, configuration, infrastructure definitions, and documentation are also SDLC deliverables subject to provenance tracking. Teams should treat any AI-generated artifact that enters a controlled repository or delivery package as a supply chain event.

#### Mission Criticality

- The speed-quality inversion risk (Sections 1 and 8) is documented in commercial data. In DoW, quality failures in mission-critical software carry consequences that differ fundamentally from commercial failures: operational impact, personnel safety, mission degradation. The emphasis on human accountability and guardrail precision throughout this play is calibrated to that context and not just to software quality metrics.

#### Software Factory Integration

- Most DoW development teams operate within an established software factory. Examples span the services and DISA: Platform One and Kessel Run (USAF), Kobayashi Maru (USSF), Army Software Factory, Black Pearl and The Forge (Navy), Marine Corps Software Factory, and DISA Citadel. The DoD Software Factory Coalition includes nearly 50 factories department-wide. This play is tool-agnostic by design, but the reference implementation in Section 5.6 should be validated against a factory's approved toolchain and ATO before adoption.

> **[FEEDBACK REQUESTED]**: Is this scope framing correct? Are there workflow types teams are actively using that fall outside these five modes? Are there additional DoW-specific constraints — coalition/partner nation concerns, ITAR implications, or program-of-record constraints — that should be addressed in this section?

---

## **3. Prerequisites and Readiness**

*Planned content: Pre-flight checklist format, consistent with other plays.*

- [ ] **Approved AI environment**: AI tools and models approved for the relevant impact level (e.g., AWS Bedrock on GovCloud for IL-4)
- [ ] **Foundational plays reviewed**: Team has read [Fundamentals](fundamentals-play.md), [AI Autonomy Continuum](ai-autonomy_continuum_play.md), and [Human-Machine Interaction Patterns](human-machine-patterns.md)
- [ ] **Data classification boundaries defined**: Clear rules on what data categories can be shared with AI systems
- [ ] **CI/CD pipeline established**: Event-driven automation infrastructure exists for asynchronous workflows
- [ ] **Version control for behavior files**: Agent personas and system prompts stored in version-controlled repositories *(Note: The AI4SDLC Working Group will publish reference agent personas, skills, and prompt templates in the coming weeks.)*
- [ ] **Human review gates defined**: Approval checkpoints established for AI-generated artifacts before they reach production
- [ ] **Guardrail policy drafted**: Initial data handling and output restrictions documented in writing

> **[FEEDBACK REQUESTED]**: What readiness conditions are your teams finding are actually blocking adoption? Are there prerequisites missing here?

---

## **4. Guiding Principles**

*Planned content: Principles table, 8–10 rows. Consistent with documentation and testing play format.*

Proposed principles — subject to workgroup refinement:

| **Principle / Risk Area** | **Expected Practice** |
| -------------------------- | --------------------- |
| **Human Accountability** | The human who reviews and approves AI-generated work is accountable for it. AI does not hold accountability. The reviewer does. |
| **Explicit Autonomy** | Autonomy granted to AI agents should be explicitly defined per task, not assumed. Default to the most restrictive level appropriate for the work. |
| **Guardrail Precision** | Vague behavioral instructions fail. Guardrails should use specific, mandatory language rather than conditional or approximate guidance. |
| **Behavior as Code** | Agent behavior definitions — personas, contracts, system prompts — should be version-controlled and reviewed with the same care as application code. |
| **Data Protection** | Do not expose classified, CUI, or sensitive system metadata (IP addresses, hostnames, API keys) to AI systems. Validator agents should enforce redaction. |
| **Sync vs. Async Awareness** | Synchronous workflows require real-time human judgment. Asynchronous workflows require pre-defined review gates. Design governance to match the interaction mode. |
| **Approved Environments** | Use only AI tools approved for your classification level. Verify model availability within approved services before deploying workflows. |
| **Attack Surface Awareness** | Agent behavior files are attack vectors. Treat them as security artifacts: restrict write access, validate provenance, and monitor for anomalous behavior. |
| **Tenure-Aware Review** | Do not assume that code review experience transfers automatically to AI output review. Establish explicit review criteria applicable regardless of reviewer seniority. |
| **Human as Team Lead** | In multi-agent and swarm contexts, the human's role shifts from reviewer of individual outputs to leader of a coordinated AI workforce. The skills required — objective-setting, outcome evaluation, exception handling — are distinct from those needed for single-agent oversight. *(See forthcoming companion: Leading Teams of AI Agents)* |
| **Outcome-Level Governance** | At the swarm level, governance cannot operate action-by-action. Define expected outcomes, observable success criteria, and escalation conditions before execution begins. Govern by what the agent team produces, not by every step it takes. |

> **[FEEDBACK REQUESTED]**: Are there additional principles your teams have found essential? Anything here that conflicts with your environment's constraints?

---

## **5. Core Patterns and Practices**

*Planned content: The "how." Six subsections, each describing a repeatable pattern with rationale, guidance, and examples.*

### 5.1 The Task-Level Autonomy Framework

*This is the centerpiece section.*

**Purpose**: Introduce a six-level autonomy framework as the operational complement to the AI Autonomy Continuum's architectural patterns. Provide a shared vocabulary developers can apply task-by-task.

**Planned content:**

- **Framing paragraph**: The Continuum play answers "What kind of AI workflow have we architected?" This framework answers "What is this agent permitted to do right now, on this task?" They operate at different levels and are used together.
- **The six-level table** (full version):

| **Level** | **Label** | **What the AI May Do** | **Human Role** |
|-----------|-----------|------------------------|----------------|
| 0 | Suggest | Provide advice only. No file edits, no command execution. | Reads, decides independently. |
| 1 | Draft Artifacts | Draft text, code snippets, plans, checklists. Human applies changes. | Reviews draft; applies changes manually. |
| 2 | Propose Changes | Produce a patch/diff proposal plus a test plan. Human reviews and applies. | Reviews diff and test plan; approves or rejects. |
| 3 | Sandbox Execute | May run commands in an isolated, non-production sandbox with explicit approval. | Monitors execution; reviews outputs. |
| 4 | Create PR/MR | May open a pull or merge request with changes and supporting evidence. Cannot merge. | Reviews PR/MR; holds merge authority. |
| 5 | Autonomous Merge/Deploy | Not recommended for most environments. Requires formal governance and executive authorization. | Oversight via monitoring and exception handling. |

- **Recommended baseline**: Level 1–2 for broad adoption, with mandatory human review before changes are applied.
- **Reconciliation with Continuum patterns**: Brief table mapping levels to patterns (Pattern 1 → Levels 0–1, Pattern 2 → Levels 1–4, Pattern 3 → Levels 3–4, Pattern 4 → Levels 4–5 with governance).
- **Key design principle**: These levels apply per task, not per team or system. A developer can invoke Level 0 for sensitive refactoring and Level 2 for routine test generation in the same session.

> **[FEEDBACK REQUESTED]**: Does this level framework match how your teams are thinking about and communicating autonomy limits? Are the labels and descriptions accurate to field experience?

---

### 5.2 Agent Persona Patterns

**Purpose**: Define what agent personas are, how to write them, and what roles are most useful in an SDLC workflow.

**Planned content:**

- **What a persona file is** (Not a prompt.  A behavior definition written in human-readable Markdown, stored in version control, tool-agnostic)
- **Common SDLC persona types (not exhaustive)** (table):

| **Persona** | **Purpose** |
|-------------|-------------|
| Code Reviewer | Analyzes code for defects, standards violations, and security risks |
| DevSecOps Control Mapper | Maps code and configurations to applicable security controls |
| Requirements Analyzer | Decomposes requirements into acceptance criteria or user stories |
| Test Planner | Generates test strategies, test cases, and coverage analysis |
| Security Analyst | Identifies vulnerabilities, threat vectors, and misconfigurations |
| Red Team Agent | Generates adversarial test cases and boundary conditions |
| Validator | Cross-checks outputs from other personas for accuracy and policy compliance |
| Orchestrator | Coordinates task sequencing across multiple personas |

- **What a persona file should include**: Role description, behavioral constraints (mandatory language), output format, escalation conditions, data handling rules
- **The Validator pattern**: Deserves dedicated callout. Serves as a quality and policy gate on outputs from other personas. Standard component of multi-persona workflows.
- **Tool portability**: Markdown-based persona files work across GitLab Duo, Claude Code (CLAUDE.md files), Roo/Cline, Continue.dev, and other AI development toolchains. The current lingua franca of agent behavior definition is Markdown.
- **Scenario example**: *[Planned : a developer invokes a Test Planner persona at Level 2, Validator confirms output meets guardrails before presenting to human for review.]*

> **[FEEDBACK REQUESTED]**: What personas are your teams actually using? Are there roles missing from this table? What has the Validator pattern looked like in practice?

---

### 5.3 Synchronous vs. Asynchronous Workflow Design

**Purpose**: Help teams design governance to match the interaction mode, not just the capability.

**Planned content:**

- **Synchronous workflow characteristics**: Developer present in real time; IDE or chat interface; tight human-AI loop; natural checkpoint at every response. Autonomy levels 0–2 appropriate for most interactions. Key risk: cognitive overload leading to rubber-stamp review of high-volume output.

- **Asynchronous workflow characteristics**: Developer absent during execution; triggered by CI/CD events; human reviews after the fact. Levels 1–4 may apply. Key design requirement: review gates must be explicitly designed in, not assumed. Audit trails are essential.

- **HMT pattern mapping** — The five HMT interaction patterns are *surfaces*; the workflow modes are *governance dimensions*. Any surface can support multiple workflow modes. The table below shows the most common mode per surface and flags where background/async execution is possible within the tool itself:

| **HMT Interaction Pattern** | **Primary Workflow Mode** | **Also Supports** | **Typical Autonomy Levels** | **Primary Governance Concern** |
| --------------------------- | ------------------------- | ----------------- | --------------------------- | ------------------------------ |
| Standalone Web Interfaces | Synchronous | — | 0–1 | No traceability; out-of-band use; data exposure risk |
| IDE Plugins and Adapters | Synchronous | — | 0–2 | Prompt versioning; limited shared oversight |
| AI-First IDEs / Workspaces | Synchronous | Asynchronous (background agent tasks) | 1–4 | Human may not be present during agent execution; review gates needed even within the IDE |
| Custom API Integrations | Asynchronous | — | 2–4 | Review gate design; audit trail completeness |
| Agentic Platforms | Agentic / Orchestrated | Asynchronous; Continuous (advanced) | 3–5 (governed) | Emergent behavior; trust calibration; formal governance required |

  > **Key implication**: When an IDE-embedded tool (GitLab Duo, Claude Code) runs a background agent task, the same asynchronous governance requirements apply — defined review gates, audit trails, and approval criteria — even though the developer is still "in the IDE." The interface does not determine the governance requirement; the human's presence during execution does.

- **Design guidance table**:

| **Factor** | **Synchronous** | **Asynchronous** |
|------------|-----------------|-----------------|
| Human presence | Real-time | Post-hoc |
| Oversight model | Continuous | Checkpoint-based |
| Appropriate autonomy levels | 0–2 | 1–4 (with defined gates) |
| Primary risk | Cognitive overload; rubber-stamping | Insufficient review gates; audit trail gaps |
| Governance priority | Guardrail precision | Gate design and approval criteria |

- **Volume warning**: As asynchronous AI output volume increases, human review capacity becomes the constraint. Monitor review-to-output ratios. If reviewers cannot keep pace, the human-in-the-loop provides compliance theater, not assurance.

---

### 5.4 Designing Guardrails That Hold

**Purpose**: Close the gap between "we have guardrails" and "our guardrails actually work."

**Planned content:**

- **The precision problem**: Guardrail effectiveness depends almost entirely on specificity. Vague or conditional language produces inconsistent results.

- **What works vs. what fails** (side-by-side examples):
  - *Fails*: "Try not to include sensitive information."
  - *Works*: "You must not output IP addresses, hostnames, or user credentials under any circumstances. If any token matches the pattern `\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}`, replace it with `[REDACTED-IP]`."

- **Categories of guardrails to define**: Data exposure (PII, credentials, hostnames), output format constraints, escalation triggers, scope boundaries.

- **Guardrail testing**: Create synthetic test files containing known-sensitive patterns and verify agent behavior before deploying. Build these as pipeline tests alongside application code, not as one-time checks.

- **Layered approach**: Persona-level guardrails (behavioral) + Validator persona (output check) + pipeline-level scanning (automated) = defense in depth.

---

### 5.5 Multi-Agent Systems and the Human Team Lead

**Purpose**: Address the emerging paradigm where multiple AI agents coordinate toward shared objectives — and where the human's role is no longer task reviewer but team lead to a digital workforce.

**Scope note for author**: This section addresses the governance and human accountability dimensions of multi-agent workflows. Architectural design of orchestration platforms — technology selection, inter-agent communication, agent state management, infrastructure provisioning — is out of scope here. For that layer, see the [AI Autonomy Continuum](ai-autonomy_continuum_play.md) — Patterns 3 and 4. Open the section with a sentence that makes this boundary explicit.

**Planned content:**

- **What changes at the multi-agent layer**: In single-agent workflows, the human evaluates individual outputs: code diffs, test plans, security findings. In multi-agent systems, a coordinating orchestrator decomposes a goal into subtasks, assigns them to specialized agents, and aggregates results. The human interacts primarily at the objective and outcome layers, not at every intermediate step. This is not a reduction in accountability. It is a shift in *where* that accountability is exercised.

- **Two coordination patterns** (table):

| **Pattern** | **Description** | **Human Role** | **Autonomy Level** | **Governance Approach** |
| ----------- | --------------- | -------------- | ------------------ | ----------------------- |
| **Orchestrated swarm** | A designated Orchestrator agent decomposes tasks and coordinates specialized agents. Workflow is explicit and auditable. | Sets objective; reviews orchestrator plan; approves outcomes; handles escalations. | Levels 3–4 with defined gates | Orchestrator persona contract; outcome-level review; full audit trail |
| **Emergent swarm** | Agents self-organize around a shared goal without a fixed orchestrator. Coordination arises from agent interaction. | Sets objective and boundaries; monitors for anomalous behavior patterns; intervenes when agents diverge. | Levels 4–5; requires formal governance | Observable outcome criteria; behavioral monitoring; defined intervention triggers |

  **Recommended default**: Orchestrated swarm. Emergent swarm patterns should be reserved for teams with established multi-agent governance and demonstrable calibrated trust.

- **The human team lead role — what it requires**: Leading an agent team is a distinct capability from using an AI tool or reviewing an AI output. Key responsibilities include:
  - **Objective-setting**: Translating mission requirements into agent-executable goals with clear success criteria and explicit boundaries
  - **Outcome evaluation**: Assessing whether the agent team achieved the objective correctly, not just whether individual outputs look plausible
  - **Exception handling**: Recognizing when agents have diverged, are looping, or have produced outputs that require human course correction
  - **Trust calibration**: Knowing when to expand agent autonomy and when to tighten it, based on observed performance over time
  - **Audit trail literacy**: Reading multi-agent execution logs to reconstruct what happened and why

  *These skills are the subject of the forthcoming companion play: **Leading Teams of AI Agents** *(ai-agent-team-leadership.md — Forthcoming)*.*

- **Governance at the orchestration layer**: Individual agent guardrails remain essential but are not sufficient. The Orchestrator persona requires additional governance:
  - Define acceptable task decomposition boundaries (what the orchestrator may and may not assign to sub-agents)
  - Establish escalation triggers: conditions under which the orchestrator must surface a decision to the human rather than proceeding
  - Require outcome summaries: the orchestrator should produce a human-readable account of what the agent team did and why
  - Audit trail requirements: every agent action, sub-agent spawn, and tool call should be logged with the originating objective and orchestrator intent

- **Relationship to the Autonomy Continuum**: Orchestrated swarms align with Pattern 3 (Orchestrated Systems). Emergent swarms approach Pattern 4 (Adaptive Ecosystems). Neither should be implemented without first demonstrating readiness at Patterns 1 and 2 for the relevant workflow. *(Cross-reference: [AI Autonomy Continuum](ai-autonomy_continuum_play.md))*

- **Scenario example** *(planned)*: A technical lead assigns a feature development objective to an agent team. The Orchestrator decomposes it into requirements analysis, code generation, test planning, and security review subtasks. Specialized agents execute each. The Orchestrator aggregates results and flags one security finding for human decision. The human evaluates the summary and approves the PR. Total human interaction: objective-setting and one exception decision.

> **[FEEDBACK REQUESTED]**: Are teams in the working group operating orchestrated or emergent swarm patterns today? What oversight mechanisms are proving effective? What skills gaps are emerging in the human team lead role?

---

### 5.6 The Reference Implementation

**Purpose**: Address the "blank page problem" that stalls adoption. A starter set teams can download, adapt, and deploy.

**Planned content:**

- **What a reference implementation includes**: Autonomy levels contract (the Level 0–5 framework as a standalone Markdown file), starter persona files (Validator, Code Reviewer, Test Planner as minimum viable set), sample orchestrator configuration, data protection guardrail templates.

- **Publishing path**: Files published via code.mil as a versioned, open-contribution repository. Tool-agnostic by design. These are Markdown files that work with any compliant AI development toolchain.

- **Contribution model**: Teams that develop effective persona files or improve the reference set should submit refinements through the working group. Applies the same open-source collaboration model to agent behavior that DoW already applies to software.

- **Scenario example**: *[Planned — a team downloads the reference implementation, adapts the Validator persona for their environment, runs guardrail tests, and deploys to their pipeline in a defined number of steps.]*

> **[FEEDBACK REQUESTED]**: What should be in the minimum viable starter set? Is there anything from current field implementations that could contribute to the reference set?

---

## **6. Decision Framework and Trade-offs**

*Planned content: Two decision tables helping teams select the right autonomy level and the right workflow mode.*

### 6.1 Selecting the Right Autonomy Level

| **Task Type** | **Recommended Level** | **Rationale** |
|---------------|----------------------|---------------|
| Exploratory code review, learning | 0 — Suggest | Developer needs context, not action |
| Test case generation | 1–2 — Draft/Propose | High volume; human validates coverage and logic |
| Security vulnerability analysis | 1–2 — Draft/Propose | High stakes; human validates every finding |
| Documentation generation | 2 — Propose | Structured output; human validates accuracy |
| Dependency update | 4 — Create PR/MR | Well-defined task; human holds merge authority |
| Automated pipeline step | 2–3 — Propose/Sandbox | Defined inputs/outputs; human reviews results |
| Production deployment | 4 at most; prefer human-driven | Human authorization required |

### 6.2 Choosing the Workflow Mode

*[Table mapping task characteristics — sensitivity, volume, latency tolerance, consequence of error — to synchronous vs. asynchronous mode selection.]*

### 6.3 Continuum Pattern to Autonomy Level Mapping

*[Cross-reference table bridging the two frameworks for readers who arrive from the Autonomy Continuum play.]*

---

## **7. Implementation Guidance (How to Start)**

*Planned content: 5–6 concrete steps. Consistent with testing and documentation play format.*

1. **Define your autonomy baseline** — Before deploying any AI workflow tooling, establish the organizational default level. Start with Level 1–2 for all developers. Document exceptions and the governance conditions that apply.

2. **Build your first persona file** — Start with the Validator persona. It provides immediate value by checking outputs for data exposure and policy compliance, requires no system integration, and is testable before it touches any production workflow.

3. **Identify a bounded pilot workflow** — Select one workflow (test case generation and code review are common starting points) and implement AI assistance at Level 1 or 2. Establish your baseline metrics before expanding.

4. **Design review gates explicitly** — For any asynchronous AI step in your pipeline, define: what outputs require human approval, what criteria define an acceptable output, and who holds approval authority. Document these gates in pipeline configuration and team practices.

5. **Validate your guardrails** — Test with synthetic artifacts before relying on guardrails in practice. Add guardrail validation tests to your pipeline alongside application tests.

6. **Expand and contribute** — Once the pilot is stable and measured, expand to additional personas or higher autonomy levels where risk and governance support it. Share effective patterns with the AI4SDLC Working Group.

---

## **8. Key Considerations (Risks, Metrics, Pitfalls)**

*Planned content: Organized risks section and metrics section. Consistent with prior plays.*

### Common Risks

**Prompt Injection via Agent Behavior Files**
Adversaries can embed malicious instructions in agent behavior files that redirect AI actions outside intended boundaries. Documented attacks have used agent instruction sets to orchestrate unauthorized system access, lateral movement, and data exfiltration. Mitigations: treat persona files as security artifacts, restrict write access, validate file provenance on load, and monitor agent behavior against expected patterns. *(Reference: MITRE ATLAS; DoD AI Cybersecurity Risk Management Tailoring Guide)*

**Governance Lag**
AI tooling adoption is outpacing governance development. Teams are establishing practices in the absence of policy — and those informal practices become the de facto standard. Mitigation: the autonomy level framework in Section 5.1 provides a lightweight starting point teams can formally adopt without waiting for comprehensive policy.

**Review Capacity Saturation**
As AI-generated output volume increases, human review capacity becomes the binding constraint. Approving outputs without meaningful evaluation due to volume or time pressure converts the human-in-the-loop into compliance theater. Mitigations: monitor review-to-output ratios; establish review SLAs; apply Validator personas to automate secondary checks where appropriate.

**Access Control to AI-Augmented Systems**
AI tools with repository, pipeline, or operational system access inherit significant privilege. Define access boundaries for AI agents the same way you would for service accounts: least privilege, explicitly scoped, regularly reviewed.

**Speed-Quality Inversion**
Observable trends in large-scale repository data show increased duplication and decreased quality indicators alongside AI adoption, particularly when human oversight is thin. Monitor quality metrics, not just velocity, when evaluating AI workflow impact.

**Shadow AI**
Developers using unapproved AI tools outside organizational visibility introduce data exposure and governance risks without leaving an audit trail. The autonomy framework only works if the tools themselves are governed. *(Cross-reference: [AI Autonomy Continuum](ai-autonomy_continuum_play.md) — Pattern 1, Shadow AI Risk)*

**Emergent Behavior in Swarm Systems**
Multi-agent systems can produce behaviors that no individual agent was designed to produce. When agents interact, share state, or spawn sub-agents, the collective behavior can diverge from intent in ways not apparent until outcomes are reviewed. Mitigations: prefer orchestrated over emergent swarm patterns; define observable success criteria before execution begins; monitor for outputs that exceed the intended scope of the objective.

**Cascading Failures in Orchestrated Workflows**
A flawed output from one agent can propagate through the workflow as downstream agents treat it as valid input. Unlike a single-agent failure (caught by a human reviewer at one point), a cascading failure may compound across multiple steps before surfacing. Mitigations: design inter-agent validation checkpoints; require the Orchestrator to flag low-confidence handoffs for human review; test multi-agent workflows against known-bad inputs before production deployment.

**Audit Trail Complexity at Scale**
In swarm systems, the audit trail grows with the number of agents and interactions. Actions taken by sub-agents spawned by other agents may be difficult to trace back to originating human intent, creating accountability gaps and compliance risk. Mitigations: require all agent actions to log the originating objective, the instructing agent, and the tool or resource accessed; treat the audit trail as a first-class artifact alongside code.

**Human Skill Gap in the Team Lead Role**
As the human role shifts from individual output reviewer to agent team lead, the required skills change substantially. Teams that expand to multi-agent workflows without developing objective-setting, outcome evaluation, and exception-handling capabilities risk governance failures that task-level review would have caught. Mitigation: develop human skills in parallel with expanding agent autonomy. *(See forthcoming companion: **Leading Teams of AI Agents**)*

> **[FEEDBACK REQUESTED]**: Are there additional risks your teams have encountered — particularly around swarm governance, cascading failures, audit trail gaps, or the human team lead skill transition — that should be captured here? (The access control challenge raised in the working group is a candidate for expansion.)

### Key Metrics

*Planned: Organized into three categories — workflow health, code quality, and security.*

**Workflow Health**
- Autonomy level distribution (what levels are teams actually using in practice?)
- Review-to-AI-output ratio (are humans reviewing in proportion to output volume?)
- PR/MR rejection rate for AI-generated changes

**Code Quality**
- Defect escape rate before and after AI workflow adoption
- Security finding rate per 1,000 lines (from static and dynamic analysis tools)
- Code duplication percentage over time

**Security**
- Guardrail validation pass rate (results from synthetic test artifacts)
- Anomalous agent behavior incidents per quarter
- Percentage of AI-generated artifacts passing through a Validator persona before reaching human review

---

## **9. Key Takeaways**

*Planned content: 5–7 declarative statements. Final phrasing TBD.*

- Autonomy is a choice, not a default. Define what agents are permitted to do before deploying them.
- Guardrails work when they are precise. Vague behavioral instructions produce inconsistent results.
- Persona files are security artifacts. Apply the same version control and access control discipline as application code.
- The human holds accountability. AI can draft, propose, execute, and generate. Responsibility for what reaches production belongs to the human who reviewed and approved it.
- Synchronous and asynchronous workflows need different governance. Design oversight for the interaction mode, not just the capability.
- Speed is not the measure of success. Monitor quality and security metrics alongside velocity to catch speed-quality inversions early.
- Reference implementations lower the cost of adoption. Genericized persona files reduce the setup burden for teams across the DoW.
- Multi-agent systems change the human role, not just the human workload. Leading a digital workforce requires skills distinct from reviewing individual AI outputs: objective-setting, outcome evaluation, and exception handling.

---

## **10. Companion Plays and References**

*Planned content: Related plays, key references, emerging guidance.*

**Related Plays**

- [Fundamentals for Building an AI-Augmented Toolchain](fundamentals-play.md)
- [AI Autonomy Continuum](ai-autonomy_continuum_play.md) — Strategic architectural patterns (Patterns 1–4); this play is the operational complement governing autonomy within those patterns
- [Human-Machine Interaction Patterns](human-machine-patterns.md) — Defines the five ways humans interact with AI tools (standalone web, IDE plugins, AI-first IDEs, custom API integrations, agentic platforms). This play governs autonomy and behavior *within* those interaction modes; HMT governs the interaction design itself. Read together.
- [AI-Augmented Testing](testing-play.md) — Applying AI to test generation and analysis
- [AI-Assisted Documentation Across the SDLC](documentation-play.md) — Documentation workflows and agentic orchestration
- [Leading Practices for Code Completion and Generation](code-gen-play.md)
- [Prompt Engineering](prompt-engineering.md)
- **Leading Teams of AI Agents** *(ai-agent-team-leadership.md — Forthcoming)* — Companion sub-play addressing the human skills required to lead coordinated AI agent teams: objective-setting in agent contexts, outcome evaluation, exception handling, audit trail literacy, and trust calibration at the team level. Addresses the shift from individual AI output reviewer to digital workforce team lead.

**Key References** *(IEEE citations to be formatted in full draft)*

- DORA Team, *2025 State of AI-Assisted Software Development*, Google, 2025 — Research on AI adoption patterns, quality impacts, and team performance
- DoD CIO, *AI Cybersecurity Risk Management Tailoring Guide*, v2 (2025) — Maps AI-specific attack vectors using MITRE ATLAS; addresses assistant-only model category
- MITRE ATLAS — Adversarial threat landscape for AI systems; documented attack patterns for agent-driven workflows
- Anthropic, *Disrupting AI Espionage*, 2026 — Documented case of weaponized agent instruction sets used for unauthorized system access and lateral movement
- NIST SP 800-218 Secure Software Development Framework (SSDF)
- NIST AI Risk Management Framework (AI RMF)

**Emerging Guidance**

- **DoDI 8430.AA (draft)** — Introduces AIBOM (AI Bill of Materials) requirements alongside traditional SBOMs. As AI-assisted workflows become embedded in software factories, the models, training data, and behavioral definitions used will require traceable provenance documentation.
- **12-Factor methodology for agentic systems** — Adapting established 12-factor application design principles (explicit dependency management, environment-specific configuration, stateless process design) to agent workflows is an emerging practitioner discussion. Teams building durable agent workflows should track this pattern.

### Bibliography for Full Draft

Citation hooks to be placed during authoring — not yet referenced inline.

#### DoW Policy and Governance

- U.S. Deputy Secretary of Defense, *Implementing Responsible Artificial Intelligence in the Department of Defense*, Memorandum, May 27, 2021 — Foundational RAI accountability memo; policy basis for Section 4 human accountability principles
- U.S. DoD, *DoD Ethical Principles for Artificial Intelligence* (Responsible, Equitable, Traceable, Reliable, Governable), Office of the Secretary of Defense, Feb. 2020 — Normative foundation for all DoW AI governance
- U.S. DoD CDAO, *Responsible AI Strategy and Implementation Pathway*, Jun. 2022 — RAI implementation roadmap; accountability chain structure and AI evaluation standards
- U.S. DoD CIO, *DoDI 8510.01: Risk Management Framework for DoD Information Technology*, updated Jul. 2022 — Mandatory RMF/ATO framework; normative basis for Section 2.1 authorization requirements
- U.S. DoD CIO, *DevSecOps Continuous Authorization to Operate (cATO) Implementation Guide*, 2024 — cATO implementation; supports iterative AI tool integration into software factories
- U.S. DoD CIO, *DoD Enterprise DevSecOps Strategy Guide* — Software factory governance patterns; compatibility reference for Section 5.6 reference implementation
- U.S. DoD CIO, *The State of DevSecOps*, Mar. 2025 — Official DoW assessment of DevSecOps adoption; institutional documentation of governance lag claim in Section 1
- U.S. Deputy Secretary of Defense, *DoD Software Modernization Strategy*, Nov. 2021 — Strategic direction for DevSecOps and AI integration as core modernization priorities
- U.S. DoD CDAO, *DoD AI and Machine Learning Scaffolding and AI Assurance*, May 2023 — AI assurance framework; supports Section 5.4 guardrail validation and Section 5.6 reference implementation
- Defense Acquisition University, *Guidebook SWE0058: Overview of Artificial Intelligence in the DoD*, v2.3, Jan. 2024 — Primary acquisition workforce AI reference; supports program manager audience
- DoD Adaptive Acquisition Framework, *Software Acquisition Pathway* — Acquisition policy context for AI workflow governance in programs of record

#### NIST Publications

- NIST AI 600-1, *Artificial Intelligence Risk Management Framework: Generative AI Profile*, Jul. 2024 — GenAI-specific companion to AI RMF; twelve risk categories applicable to Section 5.4 guardrail design and Section 8 risks
- NIST SP 800-218A, *Secure Software Development Practices for Generative AI and Dual-Use Foundation Models*, Jul. 2024 — AI-specific SSDF companion; agent behavior file governance and supply chain integrity
- NIST SP 800-53 Rev. 5, *Security and Privacy Controls for Information Systems*, Dec. 2020 — Control catalog underlying all RMF selections; AC, AU, and SI families apply to agent workflow access and audit requirements
- NIST SP 800-160 Vol. 2 Rev. 1, *Developing Cyber-Resilient Systems*, Dec. 2021 — Cyber resilience engineering principles; supports Section 8 mitigations for cascading failures and emergent behavior
- NIST SP 800-161 Rev. 1, *Cybersecurity Supply Chain Risk Management Practices*, May 2022 — C-SCRM framework; applies to AI model provenance and AIBOM tracking
- NIST SP 800-204D, *Strategies for the Integration of Software Supply Chain Security in DevSecOps CI/CD Pipelines*, Feb. 2024 — DevSecOps pipeline supply chain security; supports Section 2.1 AIBOM discussion and Section 5.3 asynchronous workflow governance

#### Security and Attack Surface

- OWASP GenAI Security Project, *OWASP Top 10 for LLM Applications 2025*, v2025, Nov. 2024 — Practitioner-standard LLM security taxonomy; LLM01 (prompt injection), LLM05 (supply chain), LLM08 (excessive agency) directly map to Section 8 risks
- Y. Liu et al., "Prompt Injection Attack Against LLM-Integrated Applications," *arXiv:2306.05499*, Jun. 2023 — Foundational academic treatment of prompt injection as an attack vector; grounds Section 8 prompt injection risk

#### AI Code Quality

- W. Harding and M. Vander Poel, *Coding on Copilot: 2023 Data Suggests Downward Pressure on Code Quality*, GitClear Research, Jan. 2024 — Empirical analysis of 153M changed lines; primary data citation for speed-quality inversion in Sections 1 and 8

#### Multi-Agent Systems and Autonomy

- S. Cohen, N. Kolt et al., "Multi-Agent Risks from Advanced AI," *arXiv:2502.14143*, Feb. 2025 — Comprehensive treatment of emergent behaviors, cascading failures, and audit complexity in multi-agent AI systems; grounds Sections 5.5 and 8 swarm risks *(preprint)*
- A. Kasirzadeh and I. Gabriel, "Characterizing AI Agents for Alignment and Governance," *arXiv:2504.21848*, Apr. 2025 — Four-dimensional agent characterization framework; autonomy dimension is academic analog to the play's task-level autonomy framework *(preprint)*

#### Software Supply Chain

- B. Zhan et al., "Trust in Software Supply Chains: Blockchain-Enabled SBOM and the AIBOM Future," in *Proc. ACM/IEEE EnCyCriS 2024* — Peer-reviewed academic treatment of AIBOM as SBOM extension; supports Section 2.1 supply chain discussion

> **[FEEDBACK REQUESTED]**: Are there additional references — particularly SEI research on AI security, ITAR guidance for AI tools, coalition/partner nation AI governance sources, or other working group contacts' published work — that should be captured here?

---

### Architectural intent

The Autonomy Continuum establishes what kind of AI workflow a team has architected. This play fills in the operational layer: what agents are permitted to do on a specific task, how guardrails are specified and tested, and who is accountable for what reaches production. The intent is a workable framework teams can adopt now, without waiting for comprehensive policy.

---

*This outline was prepared for AI4SDLC Working Group review — March 2026. Feedback due prior to authoring the full play. Submit comments via MR or to the working group channel.*
