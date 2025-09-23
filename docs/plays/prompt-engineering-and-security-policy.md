---
# automatic badge generation
lifecycle: rc
last_updated: "2025-08-15"
---
# Mapping Prompt Engineering to DoD Cyber and AI Governance

To reflect these key frameworks—**Zero Trust**, **DoD AI Ethics**, **NIST RMF**, and **DoDI 5000.82**—this guidance maps principles to prompting behaviors. Rather than just listing them, it operationalizes these frameworks for prompt authors, reviewers, and acquisition leads in the DoD.

###  Zero Trust Architecture (DoD ZT Strategy)

**Core Principle:** *Never trust, always verify.*
**Application to Prompt Engineering:**

| Zero Trust Tenet      | Prompt Engineering Implication                                                                             |
| --------------------- | ---------------------------------------------------------------------------------------------------------- |
| Assume breach         | Prompts must not leak mission, user, or credential information. Treat every prompt as potentially exposed. |
| Least privilege       | Restrict model access by role. Ensure prompts don’t exceed user’s authorized data boundaries.              |
| Explicit verification | Prompt results should not be trusted blindly. Require human review or automated validation against policy. |
| Continuous monitoring | All prompt usage and LLM interactions must be logged, auditable, and monitored for anomalies.              |

✅ *Play Recommendation:* Integrate prompt review gates in CI/CD pipelines and AI platform workflows that enforce access controls, classify inputs/outputs, and detect unsafe behavior.

----

### DoD AI Ethical Principles

**Core Principle:** *Responsible, equitable, and governable AI.*

| Ethical Pillar  | Prompt Engineering Implication                                                                                       |
| --------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Responsible** | Ensure prompts avoid hallucinations, overreliance, or opaque results. Outputs should be attributable and verifiable. |
| **Equitable**   | Avoid prompts that reinforce bias (e.g., role stereotypes in generated content). Include fairness checks in review.  |
| **Traceable**   | Prompt inputs and model configurations must be versioned and reproducible.                                           |
| **Reliable**    | Design prompts to encourage safe defaults, testability, and structured outputs that reduce downstream fragility.     |
| **Governable**  | Include prompts in artifact traceability. Audit prompt history, usage context, and contributor identity.             |

✅ *Play Recommendation:* Use structured metadata around prompts and outputs (e.g., who wrote, when/why, what model was used) to enhance traceability and ethics oversight.

----

###  NIST Risk Management Framework (SP 800-37, 800-53)

**Core Principle:** *Security controls and lifecycle risk mitigation.*

| RMF Phase      | Prompt Engineering Mapping                                                                                               |
| -------------- | ------------------------------------------------------------------------------------------------------------------------ |
| **Prepare**    | Include prompt engineering in system-level risk assessment and threat modeling.                                          |
| **Categorize** | Prompts must be evaluated for potential impact (e.g., if they generate IaC or policy, categorize as moderate/high risk). |
| **Implement**  | Apply security controls like logging, constrained output formats, and red-teaming during prompt implementation.          |
| **Assess**     | Periodically review prompt libraries and usage patterns for security and compliance gaps.                                |
| **Authorize**  | Include prompt artifacts and model behavior evidence in ATO packages.                                                    |
| **Monitor**    | Continuously track prompt-related risks: drift, policy violations, adversarial behavior.                                 |

✅ *Play Recommendation:* Treat prompts and prompt libraries as software configuration artifacts subject to security control baselines and risk assessments.

---

### DoDI 5000.82: Acquisition of Digital Capabilities

**Core Principle:** *Software is continuously acquired, delivered, and evolved.*

| Acquisition Phase        | Prompt Engineering Tie-In                                                                                       |
| ------------------------ | --------------------------------------------------------------------------------------------------------------- |
| **Needs Definition**     | Use structured prompts to analyze requirements, summarize JCIDS artifacts, and translate operational intent.    |
| **Design & Development** | Prompt engineering used to co-generate artifacts like user stories, architecture descriptions, test cases, IaC. |
| **Testing & Evaluation** | Prompts guide automated test generation, requirements validation, and cyber red-teaming.                        |
| **Fielding**             | Ensure prompts used to generate fieldable code or documentation are part of digital review/audit packages.      |
| **Sustainment**          | Prompt libraries should be versioned and sustainment teams trained in their secure use and evolution.           |

✅ *Play Recommendation:* Prompt engineering should be included as a discrete practice in software acquisition lifecycle documentation and vendor expectations.
