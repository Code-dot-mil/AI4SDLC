---
# automatic badge generation
lifecycle: rc
last_updated: "2025-05-10"
---

# 🧱 **AI4SDLC Play: [Title of Play]**

*Working title — be explicit, not clever. Plays are meant to be teachable, repeatable, and usable.
Titles should describe **what** this play teaches (e.g., “AI-Augmented Testing,” “Navigating the AI Autonomy Continuum”).*

----

## **Executive Summary (The Play in Brief)**

> *Purpose:* Provide a concise, 150–250-word overview so the reader immediately understands the **mission impact** and **key takeaway**.
> *Why:* Most DoW, DoD, and industry readers will skim before diving in. This section is the “elevator pitch” for the play.
> *Healthy Divergence:* Style and tone can vary — use story, analogy, or key quote if it clarifies the play’s essence.

**Example:**

> Generative AI is reshaping [phase of SDLC]. This play outlines secure, auditable practices to integrate AI augmentation while preserving mission assurance and traceability.
> **TL;DR:** AI augments, not replaces. Focus on calibrated trust, governance, and measurable improvement.

----

## **1. Why It Matters and How to Use This Play**

> *Purpose:* Establish **relevance** and **scope**. Describe the problem being solved and how this play connects to others.
> *Why:* Anchors the play in mission context and helps readers self-select.
> *Healthy Divergence:* Plays dealing with higher autonomy (e.g., Autonomy Continuum) may expand this into a short narrative or conceptual framing rather than a procedural “why.”

**Include:**

* Operational or mission driver (e.g., “AI adoption is accelerating testing risk”).
* Audience (roles, stakeholders).
* When this play is relevant (phase or situation).
* Relationship to other plays (Fundamentals, Requirements, etc.).

----

## **2. Prerequisites and Foundations** *(optional but recommended)*

> *Purpose:* Define readiness conditions (maturity, toolchain, data, or workforce).
> *Why:* AI amplification fails without disciplined foundations. Clarifying prerequisites protects credibility and security.
> *Healthy Divergence:* Optional for conceptual plays like Autonomy or Fundamentals; mandatory for tooling or execution-focused plays (Code, Testing, Operate).

**Include:**

* Expected DevSecOps maturity (e.g., CI/CD automation, IaC baseline).
* Data hygiene and model readiness.
* Policy alignment (e.g., Zero Trust, SSDF, DoD AI RMF).
* Workforce skills or minimum fluency required.

----

## **3. Guardrails: Security, Compliance, and Trust**

> *Purpose:* Define non-negotiable safety and security boundaries for AI use.
> *Why:* Aligns with DoD Zero Trust, RMF, and AI containment principles; establishes accountability.
> *Healthy Divergence:* The depth of this section may vary — detailed for high-risk plays (Testing, Secure, Deploy), summarized for conceptual ones (Autonomy, Prompt Engineering).

**Include:**

| Risk Area        | Guardrail                                              |
| ---------------- | ------------------------------------------------------ |
| Data Handling    | No classified or export-controlled data in AI prompts. |
| Model Provenance | Record model name, version, and hosting environment.   |
| Human Oversight  | All AI-generated artifacts require validation.         |
| Containment      | Execute AI actions within approved boundaries.         |

📌 *Guardrails protect not just data, but decision integrity.*

----

## **4. Patterns and Practices**

> *Purpose:* Present the **core architecture or workflow patterns** for AI use in this phase.
> *Why:* This is the instructional heart of the play — how teams implement the capability safely and repeatably.
> *Healthy Divergence:* Encourage creativity here. The “pattern” could be prompt templates, autonomy levels, process maps, or architecture patterns — depending on topic.

**Structure:**

* Short intro paragraph (“This play introduces five trusted patterns…”)
* Subsections for each pattern, e.g.:

  * 4.1 Pattern: [Name]
  * Description
  * Example / Prompt
  * Benefits / Risks
  * Alignment to Autonomy Pattern Level

📌 *Plays-as-code concept: each pattern should be independently reusable.*

----

## **5. Decision Framework: When and How to Apply**

> *Purpose:* Help teams decide **if and when** to use AI in this context.
> *Why:* Supports mission-aligned decision-making and calibrated autonomy — “just because you can, doesn’t mean you should.”
> *Healthy Divergence:* May take the form of a matrix, flowchart, or checklist depending on the play’s complexity.

**Include:**

* Use-case table: AI benefits, risks, recommended autonomy pattern.
* Maturity or risk thresholds (e.g., “Requires IL5 model,” “Not suitable for safety-critical systems”).
* Criteria for scaling from pilot to production.

---

## **6. Examples and Quick-Start Patterns**

> *Purpose:* Make the play actionable immediately — practical examples, prompts, or scenarios.
> *Why:* Converts abstract guidance into operational practice; improves adoption.
> *Healthy Divergence:* Some plays (Prompt Engineering) may feature detailed prompt fragments; others (Autonomy) may use case narratives or architecture snippets.

**Include:**

* 2–3 short examples in code, pseudo-code, or structured prompt format.
* Each example should demonstrate *role*, *context*, *input*, *expected outcome*.

📌 *Keep examples aligned with the DoW’s operational classification levels — no public API assumptions unless explicitly allowed.*

----

## **7. Next Steps**

> *Purpose:* Give readers a clear, minimal roadmap to pilot, measure, and iterate.
> *Why:* Encourages safe experimentation and feedback loops.
> *Healthy Divergence:* Adjust tone — procedural for technical plays; reflective for conceptual ones.

**Include:**

* “Pilot, Measure, Iterate” guidance.
* Suggested success metrics (software health, defect rate, trust calibration).
* Mechanism to share lessons learned (e.g., submit to AI4SDLC working group).

---

## **8. Key Takeaways**

> *Purpose:* Reinforce learning and close with conviction.
> *Why:* Repetition builds retention. This ensures every play ends with clarity and momentum.
> *Healthy Divergence:* Style can vary — bullet summary, narrative reflection, or quotation.

**Example:**

* AI augments human testers; it does not replace them.
* Requirements—not code—drive trustworthy tests.
* Evidence-as-code supports accreditation and auditability.
* Calibrated trust is earned, not automated.

---

## **9. Companion Plays and References**

> *Purpose:* Connect this play within the larger AI4SDLC ecosystem.
> *Why:* Reinforces the idea that no play stands alone; they form a living framework.
> *Healthy Divergence:* For high-level plays (e.g., Fundamentals), this may become an extended reading section with external standards.

**Include:**

* Links to related plays (Fundamentals, Prompt Engineering, Autonomy, etc.)
* Citations (NIST SSDF, DoD AI RMF, DevSecOps Reference Design, MITRE AI4SDLC corpus).
* Optional “Suggested Next Read” for practitioners expanding their scope.

---

# 📘 **Summary for Authors**

| Section              | Must Include                       | Can Vary                             | Why It Exists                 |
| -------------------- | ---------------------------------- | ------------------------------------ | ----------------------------- |
| Executive Summary    | 1-paragraph overview, TL;DR        | Tone, story style                    | Ensures immediate clarity     |
| Why It Matters       | Mission context, audience, scope   | Narrative depth                      | Anchors relevance             |
| Prerequisites        | Maturity, baseline                 | Optional for conceptual plays        | Defines readiness             |
| Guardrails           | Security, compliance, human review | Depth of detail                      | Protects integrity            |
| Patterns & Practices | Core framework                     | Content type (code, prompt, process) | Drives application            |
| Decision Framework   | Use-case guidance                  | Format (table, checklist)            | Informs autonomy choice       |
| Examples             | 2–3 quick-starts                   | Format and complexity                | Enables experimentation       |
| Next Steps           | Pilot & feedback path              | Level of specificity                 | Ensures iterative improvement |
| Key Takeaways        | Summary bullets                    | Style                                | Reinforces learning           |
| References           | Linked plays, standards            | Scope                                | Encourages traceability       |

---

## ✳️ **When Divergence Is Healthy**

| Divergence Type                             | Justification                                                                         | Example                                              |
| ------------------------------------------- | ------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| **Narrative or Conceptual Expansion**       | For plays like Autonomy or Fundamentals that define principles rather than execution. | Replace “Patterns” with “Frameworks and Principles.” |
| **Depth of Guardrails**                     | When the play’s domain carries higher classification or compliance risk.              | Testing, Secure, Deploy.                             |
| **Pattern Presentation**                    | Plays can choose code, prompt, or process diagrams depending on topic.                | Code Gen vs. Requirements.                           |
| **Absence of Prerequisites**                | Acceptable for plays that establish foundational concepts.                            | Prompt Engineering.                                  |
| **Additional Section (“Roles & Personas”)** | Valuable for cross-functional areas (Requirements, Testing).                          | Add between Sections 2 and 3.                        |

---

## 🧩 **Usage Notes for GitLab/MkDocs Integration**

* Each section corresponds to a Markdown include or template partial.
* Front matter should include:

  ```yaml
  # automatic badge generation
  lifecycle: draft | beta | stable
  last_updated: "YYYY-MM-DD"
  ```
* Keep headings (`##`) consistent for cross-play analytics and automatic index generation.
* Use fenced code blocks for prompt examples to support syntax highlighting and safe copy-paste.
* Limit each play to ~5,000 words max for readability; deeper guidance can live in “Companion Guides.”

