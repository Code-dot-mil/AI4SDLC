---
# automatic badge generation
lifecycle: beta
last_updated: "2025-12-31"
---

# Play: AI-Augmented Requirements Engineering

## Executive Summary (The Play in Brief)

Requirements engineering is the bedrock of secure, mission-aligned software delivery—yet it remains one of the least modernized phases of the SDLC. Generative AI (GenAI) can augment this work by accelerating the authoring, analysis, and refinement of requirements while improving testability and traceability. For the DoD, this means turning high-level artifacts like Capability Needs Statements (CNS), CONOPS, or stakeholder transcripts into actionable, auditable requirements more quickly and consistently.

This play helps DoD teams experiment responsibly with GenAI in requirements workflows. It outlines where AI can add value, guardrails to avoid risk, and prompt patterns that enhance—but never replace—human judgment.

* **TL;DR**: Use GenAI to **accelerate requirement clarity and traceability**, not to shortcut rigor. Start with structured artifacts (e.g., CNS), apply AI for decomposition and cleanup, then validate outputs with human engineers.

* **Intended Audience**: Requirements Engineers, Product Owners, Software Architects, Cybersecurity Analysts, and Program Managers.

📌 **Key takeaway**: Requirements are strategic assets. Treat AI assistance as a force multiplier for quality, traceability, and mission assurance—not a substitute for disciplined engineering.

----

## Why This Play Matters

Requirements are the **foundation of software quality and security**. Poorly written or ambiguous requirements lead to technical debt, rework, and operational risk. AI-augmented tooling enables:

* Faster generation of requirement drafts
* Early identification of gaps, contradictions, and assumptions
* Enhanced testability and traceability

BUT this requires skilled human oversight and disciplined engineering.

📌 **Key takeaway**: Requirements are strategic assets—high quality, traceability, and clarity directly impact mission success and software assurance.

----

## Purpose & Scope

This play provides actionable guidance on where and how GenAI can assist with the **authoring, analysis, refinement, decomposition, and reuse** of software requirements in a secure, Zero Trust-aligned DevSecOps pipeline. It supports:

* Translating capability needs and CNS into formal or Agile artifacts.
* Identifying duplicate, contradictory, or incomplete requirements.
* Enhancing stakeholder alignment through GenAI-assisted “views.”
* Seeding user acceptance criteria and test conditions.
* Creating traceability artifacts and roadmap suggestions.

This play **does not** address full requirements lifecycle management, model-based systems engineering (MBSE), or automated requirements validation—these may be addressed in future plays.

----

## Roles and Personas

* **Requirements Engineers / Analysts** – leverage GenAI to support backlog grooming, requirement quality checks, traceability analysis.
* **Software Architects** – assess architectural implications of AI-generated requirements, ensure feasibility.
* **Product Owners / Mission Leads** – collaborate with AI tools to synthesize user needs and mission threads into clear, prioritized requirements.
* **Security Analysts** – ensure AI-assisted requirement generation and use respects classification, security posture, and SBOM/MLBOM traceability.

----

## Prerequisites and Readiness

Before applying AI to requirements engineering, ensure the following foundational elements are in place:

- [ ] **AI Tool Access**: Approved GenAI tool available at appropriate classification level (commercial cloud, government cloud, or local-hosted)
- [ ] **Version Control**: Requirements artifacts stored in version control system (e.g., Git, Azure DevOps)
- [ ] **Data Classification Understanding**: Team awareness of data classification levels and handling restrictions for PII, PHI, and export-controlled information
- [ ] **Human Review Process**: Established workflow for human validation of AI-generated content
- [ ] **Traceability Framework**: Capability to trace requirements to sources (CNS, stakeholder inputs) and downstream artifacts (tests, code)
- [ ] **Baseline Requirements Competency**: Team familiarity with requirements engineering fundamentals (writing clear, testable requirements)

**Maturity Indicators**:
* **Starting**: Team has ad-hoc requirements documentation; GenAI used for exploration only
* **Progressing**: Requirements in version control; AI assists with drafting and refinement
* **Mature**: AI-augmented requirements integrated into continuous delivery pipeline with automated traceability

----

## Guardrails: Enabling Acceleration with Appropriate Controls

GenAI is a **force multiplier** for requirements engineering—these guardrails ensure you can move quickly while maintaining security and accountability:

* **Security & Compliance**: Match your AI tool to your data classification. Most DoD personnel can use commercial cloud services approved for their classification level, government cloud instances, or local-hosted options. The goal is to enable access to AI capabilities appropriate for your work—just ensure you don't expose sensitive data (PII, PHI, export-controlled) to unapproved tools.
* **Accountability**: Human reviewers validate AI-generated content. AI accelerates your work as a **copilot**, while you remain the authority.
* **Traceability**: Version-control AI-generated requirements and trace them to sources and validation criteria—this enables both speed and auditability.

> **Additional Guidance**: Follow the [DoD AI Cybersecurity Risk Management Tailoring Guide](https://dodcio.defense.gov/Portals/0/Documents/Library/AI-CybersecurityRMTailoringGuide.pdf) for cybersecurity assessment of AI tools—this is the primary standard for risk assessment, not play-specific security requirements. DoD personnel should also consult CDAO guidance on GenAI governance available through internal channels.

----

## Design Considerations for AI-Augmented Requirements Workflows

| Area               | Design Choice                                  | Tradeoff                                |
| ------------------ | ---------------------------------------------- | --------------------------------------- |
| **Tooling**        | Local-hosted model (e.g., Codestral, Ollama)   | Greater control vs. setup overhead      |
| **Storage**        | Structured doc store w/ embeddings (e.g., RAG) | Easy recall vs. higher compute/storage  |
| **Access Control** | Role-based prompts and review workflows        | Adds friction, but ensures traceability |

For AI integration patterns, refer to the [Fundamentals Play](fundamentals-play.md).

----

## When and Where to Use AI in Requirements Engineering

Consider GenAI during:

1. **Early Elicitation**:

   * Drafting mission-need summaries
   * Parsing unstructured interviews and documents
   * Decomposing a Capability Needs Statement (CNS) into EPICs, features, or user stories

2. **Refinement**:

   * Clarifying ambiguous statements
   * Suggesting measurable criteria (e.g., latency thresholds)

3. **Testability Review**:

   * Assessing if a requirement is verifiable
   * Suggesting test-case outlines

4. **Traceability Support**:

   * Aligning with capability taxonomies (e.g., DoDAF, UJTL)
   * Mapping to architecture decisions

### Understanding GenAI's Role: Good Candidates vs. Risks

Treat GenAI like a **new apprentice**: valuable for drafting, summarizing, and initial analysis—but not for final decisions, security definitions, or mission-critical judgments.

**Good candidates for GenAI assistance:**

* Drafting initial requirement text from structured inputs (CNS, transcripts)
* Identifying ambiguities, duplicates, or missing details
* Suggesting test conditions or acceptance criteria
* Generating traceability mappings or architectural considerations for review

**Risk areas requiring human validation:**

* **High-stakes decisions**: Prioritization, feasibility assessments, and architectural choices require human context and accountability
* **Security-critical content**: Security constraints, controls, and sensitive data handling must be human-validated; do not use unapproved commercial tools for PII, PHI, or export-controlled data
* **Authority and rigor**: GenAI outputs must be validated, traceable, and never treated as final authority

📌 **Key takeaway**: GenAI can accelerate early requirements workflows, but human engineers remain accountable for feasibility, mission alignment, and secure-by-design outcomes.

----

## Quick Start Patterns

> **Note**: The examples below are **prompt fragments**—they illustrate what to include but are not full, production-ready prompts. In a secure and auditable DoD environment, complete prompts should include role framing, constraints, formatting guidance, and context. Refer to the [Prompt Engineering Play](prompt-engineering.md) for full patterns and examples.

Try these prompt fragments with your GenAI tools approved for your classification environment:

* "Summarize the mission need expressed in these stakeholder interviews."
* "Rewrite these requirements for clarity, conciseness, and testability."
* "Suggest measurable success criteria for this performance requirement."
* "What architecture or security implications emerge from these technical requirements?"
* "Propose test cases based on the following requirement."

### CNS-Specific Prompt Examples

* “Draft Agile EPICs and Features based on this Capability Needs Statement.”
* “Highlight capability gaps based on this CNS.”
* “Create stakeholder use-case narratives or personas from this CNS.”

📌 **Key takeaway**: Prompt fragments should be embedded in structured patterns. Use the full-stack patterns from the Prompt Engineering Play to create safe, consistent outputs.

----

## Example Use Case: DoD Logistics System

**Before AI:** Stakeholders provide a Capability Needs Statement (CNS), hundreds of interview transcripts, and legacy documents. Requirements engineers manually extract, reconcile, and draft initial backlog items—often taking weeks.

**With AI Assistance:**

* GenAI summarizes key needs
* Highlights ambiguous terms
* Suggests backlog item structures (e.g., EPIC > FEATURE > STORY)
* Engineers curate, validate, and refine into deployable requirements

📌 **Key takeaway**: Documents like CNS and stakeholder transcripts are ideal inputs for GenAI decomposition—but engineering teams must ensure traceability and validation remain intact.

----

## Requirement Types and AI Support

| Requirement Type          | AI Contribution                                        |
| ------------------------- | ------------------------------------------------------ |
| **Product Requirement**   | Clarify mission outcomes, decompose capability goals   |
| **Technical Requirement** | Suggest architectural drivers, analyze feasibility     |
| **Security Requirement**  | Highlight missing controls, reference STIGs/Zero Trust |
| **Interface Requirement** | Draft interface contracts, suggest validation steps    |

This taxonomy reflects common categories used in government and industry AI SDLC references[^1].

📌 **Key takeaway**: Different audiences need different requirement views—engineers need technical clarity, while stakeholders benefit from narratives, personas, or use-case framing.

[^1]: DEFRA, "Generating Requirements | Defra AI in the SDLC Playbook," *Department for Environment, Food & Rural Affairs*, United Kingdom, Apr. 2023. \[Online]. Available: [https://defra.github.io/defra-ai-sdlc/pages/generating-requirements/](https://defra.github.io/defra-ai-sdlc/pages/generating-requirements/). \[Accessed: Sep. 16, 2025].

----

## Key Considerations: Risks and Metrics

### Common Risks

* **Over-reliance without review**: Teams accepting GenAI outputs without sufficient human validation
* **Data exposure**: Sensitive data (PII, PHI, export-controlled) inadvertently included in prompts to unapproved tools
* **Traceability gaps**: GenAI-generated content not linked to source documents or validation criteria
* **Skill atrophy**: Over-dependence reducing team competency in requirements engineering fundamentals

### Key Metrics to Track

**Quality Indicators:**

* **Requirement defect rate**: Ambiguous, incomplete, or contradictory requirements detected in review or downstream phases
* **Traceability coverage**: Percentage of requirements traced to sources and downstream artifacts (tests, code)

**Calibrated Trust Indicators:**

* **Human edit rate**: Percentage of GenAI-generated requirements modified by reviewers (target: 20-40% suggests healthy collaboration)
* **Review time efficiency**: Time spent validating GenAI-assisted vs. manual requirements
* **Team confidence score**: Periodic assessment of team trust in GenAI outputs (1-5 scale)

**Process Efficiency:**

* **Time to baseline**: Cycle time from elicitation to approved requirement set
* **Requirements throughput**: Number of requirements processed per cycle with GenAI vs. without

Healthy GenAI-human collaboration shows stable or improving quality metrics while review time decreases and team confidence increases.

----

## Next Steps

* Pilot with one project using controlled AI prompting
* Evaluate requirement quality delta (pre/post)
* Share patterns, gaps, and feedback to refine this play

----

## Related Plays

* [Code Generation Play](code-gen-play.md)
* [Fundamentals Play](fundamentals-play.md)
* [Prompt Engineering Play](prompt-engineering.md)
* [AI Autonomy Continuum](ai-autonomy_continuum_play.md)
* [IDE Security Play](ide-security.md)

----

## Future Enhancements

This play is part of a living series and will evolve as adoption patterns and field feedback mature. Future iterations may address:

* **Mission Thread Traceability**: Aligning Agile-style requirements with DoD acquisition artifacts like MNS, ORD, and KPPs.
* **MBSE Integration**: Supporting model-based requirements via SysML, DoDAF views, and Digital Engineering artifacts.
* **AI-Aided Risk Identification**: Using GenAI to classify and surface requirements risks (e.g., ambiguity, criticality, testability gaps). This would require coordination with CISO and USD(R&E) T&E stakeholders to establish governance and validation criteria.
* **Cybersecurity Hooks for DevSecOps**: Integrating AI-assisted prompts into DevSecOps automation workflows to support security scanning, SCA inputs, and continuous assessment as part of the value stream flow. This complements broader DevSecOps plays on automated security integration.
* **Export Control & Releasability**: Identifying requirements that may introduce ITAR or coalition-sharing constraints.
* **Support for Non-RE Experts**: Patterns and training wheels for mission leads or product owners new to formal RE.

Teams are encouraged to contribute feedback, new prompt strategies, or integration examples via the AI4SDLC Working Group.

----

**End of Play**