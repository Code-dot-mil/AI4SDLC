# **Play: Leading Practices for Code Completion and Generation**

## **Executive Summary**

The use of Generative AI (GenAI) for code completion and generation is accelerating across public and private sectors. These tools promise faster delivery, improved maintainability, and even enhanced security—but they also introduce new risks. In fact, some leading technology firms that develop GenAI capabilities restrict their own engineers from using these tools for production code, underscoring the need for measured, policy-aligned adoption.

This play provides DoD-aligned guidance on current leading practices, risk mitigation strategies, and governance approaches—with an eye toward emerging agentic platforms and the evolving role of human-machine teaming in software creation.

GenAI-powered code completion and generation tools are transforming software engineering in the DoD. However, ensuring reliability, traceability, compliance, and mission assurance is essential when integrating any AI-generated code into DoD software pipelines. This play outlines secure usage patterns, appropriate boundaries, and validation mechanisms to support intentional, auditable integration.

----

## **1. Introduction**

### **Why this play matters.**

Generative AI (GenAI) tools for code completion and code generation are transforming how DoD software teams design, write, and test code. These tools offer measurable gains in productivity, maintainability, and even security—if used correctly.  Recent studies show gains ranging from **4% in sustained engineering productivity**[^1] to **55% in task completion speed for specific use cases**.[^2] But without structured oversight, they also introduce risks: incorrect or hallucinated code, trust boundary violations, and amplification of bad practices at scale.  Additional risks include security vulnerabilities, data privacy exposures, ethical and legal liabilities, and reduced human accountability for generated outputs—especially when operating at speed and scale within classified or operational environments.

----
📌 **Clarifying Scope:** Human Augmentation, Not Full Automation
> This play focuses on augmenting human developers—not replacing them. While fully autonomous agents may emerge in future workflows, this guidance centers on GenAI tools that assist and accelerate human-led coding practices. Future plays will address autonomous agents explicitly.
----

This play begins to clarify how to securely integrate GenAI capabilities—from autocomplete-style suggestions to full-function code generation—into the SDLC. These tools are not intended to replace human developers, but to augment them—accelerating development and improving maintainability while reinforcing accountability and security. These tools are not intended to replace human developers, but to augment them—accelerating development and improving maintainability while reinforcing accountability and security. .

[^1]: BlueOptima. (2024). *The Impact of Generative AI on Software Developer Performance (v8)*. BlueOptima Ltd. Internal Report.
[^2]: GitHub Copilot Research. (2022). *Quantifying Productivity Gains from AI-Powered Code Completion.* Retrieved from: https://github.blog/2022-09-14-research-how-github-copilot-helps-developers/

### **Purpose of the Play**

Clarify  safe and intentional practices for integrating GenAI-powered code completion and generation into the SDLC:

- Promote responsible use of GenAI-augmented coding tools in DoD environments.
- Establish boundaries between assistive automation and autonomous decision-making.
- Build Establish traceability as a foundation for building trust in AI-augmented software practices.

### **Scope and Applicability**
This play focuses on GenAI-driven capabilities used to assist with authoring and modifying code artifacts, including both inline completions and prompt-driven generation. The term “code” includes application logic, infrastructure scripts, tests, and pipeline definitions—spanning the work of developers, testers, SREs, and platform engineers.

To guide safe and auditable use, this play references two common usage patterns: CHop (Chat-Oriented Programming), a structured approach emphasizing prompt governance, and VIBE programming, a less formal and higher-risk pattern often used during prototyping.  
See [Section 4 – Key Definitions] for full descriptions and associated risk guidance.

**In-Scope**:

- IDE-based code completion tools (e.g., GitHub Copilot, Continue.dev)
- Prompt-driven code generation workflows using LLM-based tools such as TabNine and Codestral
- Test generation and static analysis using GenAI in CI/CD pipelines
- AI-augmented pair programming and assistant models

**Out-of-Scope (for now)**: Fully autonomous agents will be addressed in future plays

>📌 **Scope and Role Diversity**:
>
> While this play focuses on code generation, the term “code” encompasses a wide variety of artifact types beyond traditional software functions. In modern delivery pipelines, infrastructure code, test automation, pipeline definitions, and security scripts are all authored—and increasingly generated—by a variety of roles. This play applies wherever GenAI tools are used to generate structured logic or configuration artifacts, regardless of whether the author identifies as a developer, tester, SRE, or platform engineer. Role-specific guidance appears in future plays.

### **Where We Are Today**

- Experimentation with prompt-based GenAI tools is rapidly advancing across industry and academia, with selected adoption pilots underway in U.S. government and defense organizations.
- Current DoD guidance focuses on augmentation—keeping a human in the loop—while establishing safeguards for responsible use.
- Fully autonomous code agents are an emerging area but are not yet ready for widespread adoption or mission-critical use.

Despite trailing other sectors by 10% in current GenAI adoption (44% in government vs. 54% across industries), public sector interest is rapidly growing. **84% of government organizations plan to invest in GenAI within the next year**, even as strategic readiness, policy clarity, and workforce skills lag.[^3]   For the Department of Defense, this gap reinforces the importance of intentional adoption. This play emphasizes **safe, augmented use** of GenAI for code completion and generation—**prioritizing mission assurance, trust, and traceability** over speed or full automation.

[^3]: SAS and Coleman Parkes, Your Journey to a GenAI Future: A Strategic Path to Success for Government, SAS Institute Inc., 2024. [Online]. Available: https://www.sas.com/en_us/whitepapers/genai-future-government-114056.html

### **What is Generative AI in Software Engineering?**

Generative AI models—such as OpenAI Codex, GitHub Copilot, and Meta’s Code Llama—assist software teams by:

- Suggesting code snippets, functions, or entire scripts.  This is for both core software code as well as for the necessary automated unit testing requisite for strong CI/CD pipeline practices.
- Automating boilerplate coding tasks.
- Enhancing code documentation and suggestions for focused refactoring.

----

## **2. Prerequisites and Foundations**

Before introducing GenAI-based code completion and generation tools into a DoD software environment, teams must establish foundational practices that enable secure, auditable, and mission-aligned integration. This play assumes the presence of key DevSecOps capabilities and an understanding of evolving human-machine interaction patterns, as outlined in Play: Fundamentals.

### **DevSecOps Baseline**

This play builds on the assumption that your software team or organization has achieved baseline DevSecOps capabilities. GenAI tooling must not be treated as standalone or informal—it must be integrated into trusted delivery pipelines with visibility and traceability to enable auditability, reduce risk, and uphold mission assurance.

**Why?** Because ungoverned GenAI use can bypass traditional security gates, propagate unvetted code, and obscure accountability. Without visibility and traceability, teams cannot verify provenance, assess performance, or respond to emerging threats—placing both the mission and the organization at risk.

Key prerequisites include:

- **CI/CD Automation** - Automated pipelines that support linting, testing, artifact signing, and environment promotion—allowing GenAI outputs to flow securely into trusted paths.  Online materials and guidance such as [MinimumCD.org](https://minimumcd.org) provide guidance on the base essentials or the minimums necessary to implement continuous delivery.

- **Secure Software Supply Chain** - Adoption of frameworks like the **Secure Software Development Framework (SSDF)**, as defined in [NIST Special Publication 800-218](https://csrc.nist.gov/publications/detail/sp/800-218/final), or the **Supply-chain Levels for Software Artifacts (SLSA)**, maintained by the Open Source Security Foundation at [slsa.dev](https://slsa.dev), enables provenance, dependency control, and vulnerability scanning—even for AI-generated code.

- **Infrastructure-as-Code (IaC)** - GenAI may suggest or write IaC templates (e.g., Terraform, CloudFormation). While these may be created by an infrastructure engineer less familiar with software engineering principles, these generated scripts must be treated these as critical assets, subject to the same secure design and validation workflows.

- **SBOM / MLBOM / AIBOM Practices** - Generated code must be tracked and attributed in the software bill of materials. Prompt provenance and model source should be considered part of an emerging Machine Learning Bill of Materials (MLBOM) and Artificial Intelligence Bill of materials (AIBOM).

Note: This mirrors traceability expectations in model-based engineering workflows, where executable artifacts generated from tools like CAMEO (SysML) or MATLAB/Simulink are documented. GenAI artifacts must meet the same bar for lineage, compliance, and mission assurance.

>##### 📌 Takeaway
>
>GenAI tools for code generation and completion must be treated as components of the DevSecOps ecosystem—not as shortcuts or sidecar helpers. Integrating these tools requires automated validation, traceability, and aligned governance. Their use must reinforce—not bypass—the secure, auditable, and mission-assured delivery principles already established in modern software pipelines.

### Human-Machine Interaction Patterns

How teams interact with GenAI tools shapes traceability, validation, and alignment with mission assurance goals. Interaction patterns determine whether generated outputs can be audited, governed, and integrated into secure DevSecOps workflows.

This play references patterns introduced in [Fundamentals for Designing an AI-Augmented Tool Chain](fundamentals-play.md):

- **Standalone Interfaces** (e.g., ChatGPT browser use): Not recommended—no traceability or policy enforcement.
- **IDE Plugins** (e.g., Copilot, Continue.dev): Inline suggestions with limited visibility. Use only with strong local controls and peer review.
- **API-Based Integrations**: Enable prompt logging, CI/CD control, and SBOM/MLBOM support. Ideal for structured GenAI use. These are well-suited for structured GenAI use, especially in environments requiring auditability, observability, and policy enforcement.
- **Agentic Platforms**: High capability and autonomy, but require guardrails, execution limits, and policy-as-code enforcement.

Select interaction patterns that align with your **trust boundaries, observability needs, and pipeline maturity**. See [Fundamentals for Designing an AI-Augmented Tool Chain](fundamentals-play.md)  for detailed architectural recommendations.

----

## **3. The Basics**

Effective use of GenAI for code completion and generation starts with practical discipline. This section introduces two foundational techniques that shape GenAI output: providing relevant technical context and crafting precise, structured prompts. These skills help teams produce consistent, trustworthy results while reinforcing traceability and intent.

### **Context Awareness**

GenAI models operate within constrained "context windows"—meaning they can only “see” a limited amount of input (code, comments, architecture notes) at a time. Without the right context, outputs may be incomplete, incorrect, or misaligned with system goals.

To improve relevance and reduce hallucination in GenAI-generated code:

- **Provide scoped architectural context**  
  *Specify the layer, framework, and purpose of the code*

  > **Example:** Instead of saying “generate a login component,” say “generate an Angular frontend login component that calls a Flask-based API endpoint for JWT token authentication.”  
  > **Tip:** Paste relevant architectural stubs (e.g., existing interface definitions or service contracts) to guide completions.

- **Inject domain-specific terms**  
  *Reinforce military or mission-relevant semantics the model might not know by default*

  > **Example:** “Generate a parser for a tactical data link message using the Link-16 protocol” will produce better results than “generate a network message parser.”  
  > **Tip:** Maintain a glossary or prompt library of key mission-specific terms and phrases for prompt reuse.

- **Include security constraints**  
  *Explicitly call out rules for data handling, encryption, and access control*

  > **Example:** “Write a Python function that logs access attempts and enforces RBAC using DoD CAC authentication.”  
  > **Tip:** Pair prompts with brief in-line comments or notes on required compliance standards (e.g., FIPS 140-3, NIST 800-53).

- **Avoid prompts that assume global awareness or full-system understanding**  
  *Keep prompts focused and bounded to specific tasks with provided context*

  > **Pitfall:** “Generate the backend for our logistics app” is too vague.  
  > **Better:** “Create a Flask route for submitting transport requests using the existing `RequestForm` class and storing data in PostgreSQL.”

> ##### 📌 Takeaway
>
Treat GenAI more like an unseasoned contributor than a trusted teammate—it can be remarkably helpful, but only when guided with precision. Think of prompting as mentoring: you need to set clear expectations, define boundaries, and provide architectural and domain context. Just as junior staff need coaching, these tools benefit from examples, constraints, and continuous review.
>
>Effective use means articulating the "why" and "how" behind tasks, not just the "what."
>

### **Prompting**

### What is a Prompt?

A **prompt** is the input provided to a Generative AI model that directs it to produce a specific output.

In software engineering contexts, a prompt typically includes:

- **Instructions or tasks** (e.g., “Write a Python function to hash passwords using SHA-256.”)
- **Code context or examples** (e.g., function headers or config templates)
- **Constraints** (e.g., required language, security policies)
- **Expected outcome** (e.g., unit-testable code, secure configuration)

##### 📌 Takeaway:
> Think of prompting as giving direction to a capable but inexperienced teammate. The more clearly you describe the task—including relevant context, boundaries, and constraints—the more useful and trustworthy the output will be. Precision isn't optional—it's the difference between noise and value.
>
> You wouldn’t ask a new hire to “just write some code.” Don’t do that with GenAI either.

### **Leading Practices for Writing AI Prompts**

Writing high-quality prompts is critical to ensuring that AI-generated code is useful, secure, and aligned with system intent. Prompts should be treated as **governed artifacts**, subject to peer review, reuse, and revision over time.

Even though GenAI outputs can vary between runs, storing and governing prompts still enables:
-	Traceability of design decisions
-	Reviewability for alignment with policy or coding standards
-	Feedback loops to improve future prompt effectiveness

> ##### 📌 Takeaway:
>
Think of prompts less like natural conversation and more like test cases for generative systems: they should be scoped, structured, and explainable.
>

Emerging tools such as Helicone, PromptLayer, and LangSmith enable observability, version control, and feedback tracking for prompt engineering. These capabilities are especially valuable in mission environments where reproducibility, traceability, and auditability are required.

Here are key leading practices for operational use:

| Principle                    | Practice Example                                                                                |
| ---------------------------- | ----------------------------------------------------------------------------------------------- |
| **Be Specific**              | Instead of “generate a login function,” say “write a Python login function using JWT and RBAC.” |
| **Set Constraints**          | Define what libraries, standards, or DoD policies must be followed.                             |
| **State Expected Output**    | Describe the format (e.g., JSON, Bash script, Python function with docstring).                  |
| **Avoid Ambiguity**          | Avoid vague verbs (“optimize,” “improve”) without clear criteria.                               |
| **Reference Context**        | Include domain, service boundaries, file structure, or architectural decisions if relevant.     |
| **Use Declarative Language** | Ask for outcomes (“validate all user inputs”) rather than low-level steps.                      |

> ✅ Tip: Prompts that are too open-ended lead to hallucination or overly verbose outputs. Precision and clarity are key to reliability and reuse.

----

### **Prompt Patterns Table**

Below is a set of example prompts for common use cases relevant to DoD software engineering:

| **Use Case**             | **Effective Prompt**                                                                                            |
| ------------------------ | --------------------------------------------------------------------------------------------------------------- |
| **Algorithm Generation** | “Write a Python function that implements merge sort for a list of integers and returns a new sorted list.”      |
| **Secure Coding**        | “Generate a Java login method using OAuth 2.0 and input validation to prevent SQL injection.”                   |
| **Test Generation**      | “Create a set of JUnit tests for a function that calculates flight duration given departure and arrival times.” |
| **CI/CD Integration**    | “Write a GitHub Actions workflow that runs unit tests and performs a SAST scan on each pull request.”           |
| **IaC Template**         | “Create a Terraform script that provisions a public/private subnet pair in an AWS VPC.”                         |
| **Logging & Monitoring** | “Add structured JSON logging to a Python API handler for DoD telemetry collection.”                             |

> 🚫 **Avoid**: “Make a script for user auth.”

> ✅ **Prefer**: “Generate a Python script for user authentication using JWT, with comments and input validation.”

----

## Upcoming DoD Prompting Guide

Check back soon for the upcoming prompting guide tailored to software value streams for DoD software. This guide will include a description of what prompting is, how to construct a prompt, and examples based on the many roles—from product owners to software engineers, testers, and operations personnel.

----

## Industry Prompting Guides

The following resources offer practical and evolving guidance for writing effective AI prompts. While tailored for different use cases, many principles (e.g., specificity, context-setting, declarative language) are applicable across DoD environments.

| **Source**             | **Guide Title**                              | **Notes**                                                                 |
|------------------------|----------------------------------------------|---------------------------------------------------------------------------|
| Microsoft              | [Prompt engineering 101](https://learn.microsoft.com/en-us/training/modules/introduction-to-copilot/) | Practical patterns, risk considerations, and use case structure for GitHub Copilot |
| Google DeepMind        | [What is Prompt Engineering? (Google Cloud)](https://cloud.google.com/discover/what-is-prompt-engineering) | Emphasizes examples, constraints, and prompt testing in code generation 
 | LangChain / LangSmith  | [Prompt Engineering Quick Start (LangSmith UI)](https://docs.smith.langchain.com/prompt_engineering/quickstarts/quickstart_ui) |Versioning, testing, and observability in structured pipelines            |

✅ These guides reinforce that prompting is not just an art—it's an engineering discipline, especially in regulated or mission-critical environments.

## **4. Important Definitions**

To align understanding across diverse teams and stakeholders, this section defines foundational terms used throughout this play.

- **Code Completion**  
  Token-level or line-level suggestions provided inline as a developer types. Completions typically leverage the immediate code context (e.g., variable names, function signatures) and are most effective in IDE environments. Tools like GitHub Copilot and Continue.dev fall into this category.

- **Code Generation**  
  The use of prompts to produce complete code blocks, files, or configurations. Generation often spans multiple lines and may include functions, classes, test scaffolds, IaC templates, or documentation. It requires clear intent and validation, especially when used for production workflows.

- **CHop Programming** (*Chat-Oriented Programming*)  
  A structured, intentional approach where prompts are engineered to reliably produce high-quality, mission-aligned code. CHop emphasizes prompt versioning, reuse, and governance. It aligns with secure-by-design practices by treating prompts as first-class artifacts.

- **VIBE Programming**  
  A casual, exploratory mode of working with GenAI, where developers iterate quickly by "vibing" with the model—adjusting prompts on the fly without formal structure or traceability. 

> ⚠️ **Warning:** While this pattern may accelerate prototyping, it poses serious risks in high-assurance environments, including unvetted logic, hallucinated code, policy violations, and the inability to trace how decisions were made. VIBE should be explicitly restricted or bounded by policy, with strict gating before any output is promoted to shared environments or production workflows.

----

### Shifting Roles: From Creators to Reviewers and Beyond

As GenAI capabilities become integrated into daily software workflows, the role of the developer is shifting—from a focus on **hand-coding logic** to one of **curating, reviewing, and validating machine-generated code**.This isn’t just a tooling change—it’s a cognitive and cultural shift

This is not just a tooling change—it’s a cognitive and cultural transformation.

----

### What’s Changing?

Traditional software development emphasizes problem decomposition, algorithm design, and precise implementation. Now with GenAI:

- Developers often **describe intent in structured natural language**, not just code.
- GenAI fills in boilerplate, scaffolding, and sometimes entire methods or configurations. 
- The human becomes a reviewer, risk manager, and design validator, evaluating:

  * Logical correctness
  * Security and performance implications
  * Fit to architecture, coding standards, and domain norms

**Emerging dynamic:** In addition to reviewing GenAI output, developers are also leveraging GenAI to critique their own code—identifying vulnerabilities, anti-patterns, or performance issues. This bi-directional review loop is becoming part of modern DevSecOps workflows.

----

### ⚠️ Risks Without Role Reframing

Without acknowledging this shift, teams risk:
- Mis-calibrated trust: over-trusting GenAI output when it “looks right,” or under-trusting it when it could be helpful.Over-trusting model output, especially when it “looks right”

- **Skill atrophy**, as deeper logic design and design thinking are is outsourced too early
- **Undertraining reviewers**, especially newer engineers unfamiliar with system-level tradeoffs
- Ambiguity in accountability when machine-generated code enters production pipelines

----

### What Teams Can Do

- **Treat GenAI output like a pull request from a junior developer**: always review, never assume.
- **Introduce peer-pairing and AI-pairing** for both generation and validation.
- **Encourage structured prompt reuse** (see: CHop Programming).
- **Define ownership expectations**—who is accountable for what’s committed?
- **Train teams in critical review**, not just prompting—and begin exploring GenAI tools that review human-written code as part of CI/CD.
- **Prioritize training** for both early-career and experienced engineers—covering prompt design, GenAI use, system-level validation, and architectural reasoning—to prevent skill atrophy and ensure mission-aligned decision-making.

---

> The future of software engineering isn’t just faster coding—it’s smarter reviewing.
> Let’s make sure our teams are equipped for the role they now play.

----

## **5. Use Cases and Boundaries**

Not all code is equal when it comes to applying GenAI. This section outlines where AI-generated code offers clear value—and where the risk outweighs the reward.

----

### **Appropriate Use Cases**

These use cases are well-suited for GenAI assistance because they typically involve low to moderate risk, are bounded in scope, and have well-understood intent—making human oversight easier and more effective. When paired with review, validation, and pipeline-integrated governance, they deliver efficiency gains without compromising mission assurance.

- **Infrastructure as Code (IaC)**  
  Generating Terraform, CloudFormation, or Ansible templates to scaffold cloud infrastructure or policy-as-code declarations. These follow predictable patterns and benefit from repeatable structure.

- **Unit and Integration Test Scaffolding**  
  Creating boilerplate tests based on function signatures or example inputs/outputs. While initial scaffolding is safe, human augmentation is required for completeness and correctness.

- **Boilerplate and Repetitive Code Generation**  
  Reducing time spent writing repetitive accessors, data transfer objects (DTOs), API wrappers, and data mapping layers. These tasks are pattern-based and present limited functional risk.

- **Documentation Support**  
  Generating inline comments, method docstrings, or markdown files to support code clarity and maintainability. Prompts should reinforce clarity, not just verbosity.

- **Code Refactoring Suggestions**  
⚠️ Use with caution. While GenAI can propose modularization or naming improvements, automated refactoring—especially on production or system-critical code—requires careful human review and regression testing

> ✅ These scenarios benefit from AI acceleration while preserving human accountability and mission alignment.

----

## High-Risk Use Cases

These categories pose elevated risk for **hallucination, incomplete logic**, or **critical vulnerabilities**. GenAI tools should generally not be used for **generation or modification** of source code in these areas—though they may be appropriate for **review, analysis**, or **documentation** in a controlled setting:

- **Authentication and Authorization Logic (AuthN/AuthZ)**  
  Mistakes in access control or identity management can compromise the entire system. These functions merit expert design, human review, and rigorous validation.

- **Encryption or Cryptographic Implementations**  
  GenAI must not generate or modify crypto routines. Always use vetted libraries and defer implementation to trained professionals.

- **Safety- or Mission-Critical Software**  
  Software tied to life, safety, kinetic effects, or weapons systems must follow formal methods and certified workflows. GenAI may assist in reviewing logs or documentation, but not generating executable logic.

> _**NOTE:** This is a draft and should not be disseminated yet. Work group members are asked to provide feedback and may share with select individuals who can provide additional expertise._

- **Export-Controlled or Classified Domains**  
  Prompts involving ITAR, EAR, CUI, or classified topics must not be entered into commercial GenAI tools. Doing so may violate DoD policy and emerging CIO guidance related to data sharing and model boundary protections.

❌ **While GenAI should not be used to author code in these domains, it may play a role in _reviewing or critiquing human-authored logic_—under the same validation and audit controls used in high-assurance environments.**

## **6. Trust, Verification, and DevSecOps Pipeline Integration**

The trustworthiness of AI-generated code is not a static property—it must be **earned**, **validated**, and **maintained** through repeatable processes. Within secure software delivery environments like those in the DoD, trust is achieved not by assuming GenAI is correct, but by designing workflows that **verify and trace every output**.

This section outlines how teams can integrate GenAI-assisted code generation into existing DevSecOps pipelines, applying rigorous verification practices that preserve mission assurance and accountability.

----

### **Why Trust Must Be Engineered**

AI-generated code is often syntactically correct but semantically wrong. It may pass initial tests but:

- Contain subtle security vulnerabilities
- Misuse libraries or APIs
- Violate architectural conventions
- Drift from mission-specific requirements

But let’s be clear: these risks are not unique to GenAI. Human-generated code also requires validation at time of creation—not deferred to a late-stage review. That’s the essence of DevSecOps: embedding trust, security, and accountability throughout the pipeline, regardless of who (or what) wrote the code.

----

### **Verification Practices**

To establish trust in GenAI-generated code, apply multiple layers of validation:

- **Automated Tests**  
  Ensure generated functions are covered by unit and integration tests. When test scaffolds are generated alongside code, review both before inclusion.

- **Static and Dynamic Analysis (SAST/DAST)**  
  Scan all generated code as if it were human-authored. Treat it with zero-trust assumptions until verified.

- **Peer Review**  
  AI-assisted code must undergo human review. Focus on logic correctness, boundary conditions, and consistency with mission goals.

- **Prompt and Output Logging**  
  Capture the prompt, model used, and output version. Associate each with a commit or CI/CD build ID for traceability.

- **IP & License Validation**
  Treat GenAI-generated code as potentially tainted until proven otherwise. Apply SBOM scanning tools and license validators (e.g., FOSSA, ScanCode) to detect unapproved open-source license patterns or reuse of protected content.

----

### **Pipeline Integration**

Trust must be enforced through the pipeline, not just during local development.

- **IDE + Pre-Commit Hooks**  
  Warn or block commits with unreviewed or untagged AI-generated code.

- **CI/CD Controls**  
  Validate prompts and responses against policy (e.g., permitted models, classification labels). Run tests and scans in automated jobs.

- **Prompt Artifact Management**  
  Store reusable prompts in Git, tag by function (e.g., `generate-test-prompt-v3`), and associate with model version.

- **Generated Code Labeling**  
  Include inline comments to mark generated code (e.g., `// Generated by GitHub Copilot`). Support downstream analysis and SBOM inclusion.

- **SBOM/MLBOM Integration**  
  Track model usage and prompt lineage as part of SBOM or emerging MLBOM practices. Link to secure model registries where possible.

----

### Tool Behavior and Model Variability

Not all GenAI tools behave the same way—even when using similar model families. Developers and reviewers must account for meaningful differences in:

- **IDE plugin behavior** (e.g., GitHub Copilot vs. Continue.dev)
- **Backend models** (e.g., GPT-4 vs. Claude vs. open-source LLMs)
- **Deployment context** (e.g., SaaS vs. on-prem vs. air-gapped instances)

These variations can affect everything from code style and verbosity to security filters and prompt interpretation.

> A prompt that generates clean, testable output in one context may produce noisy or unsafe results in another.

### Practical Recommendations

- **Maintain a vetted tool registry** with approved GenAI tools, known limitations, and recommended usage patterns or guardrails for each (e.g., prompt libraries, constrained output types, acceptable use cases).
- **Log prompt/model metadata** with every commit or pipeline run to ensure traceability, reproducibility, and model lifecycle control.
- **Evaluate tools in representative mission environments** before widespread adoption—especially for offline use or deployment on classified or air-gapped systems.
- **Flag and test for version drift** in IDE extensions, plugins, or model interpreters to prevent silent changes in behavior or output.
- **Provide role-specific tradecraft guidance** for approved tools, including prompt examples, review checklists, and lessons learned from secure use in operational contexts.


> For systemic risk guidance, see the upcoming [Risk Reference Companion](../resources/risk-reference.md) where “Tool Drift”, “Model Fragmentation”, and "Prompt Misuse" are addressed in more depth.

----

### **Example Human-in-the-Loop Workflow**

A simple, secure-by-design workflow might look like:

> `AI Suggests` → `Automated Test Runs` → `Human Reviews & Approves` → `Secure Deploy`

This pattern reinforces accountability and allows teams to scale GenAI usage without weakening their software assurance posture.

## **7. Measures and Feedback**

Before adopting GenAI tools for code generation, teams must clarify:  
> *What are we trying to improve—and why?*

This section introduces system-aligned metrics that support trustable, explainable, and secure GenAI integration, guided by the **AI-SWEC (AI Software Evaluation Criteria Framework)**.  Check back for the upcoming guidance and deeper dive into the AI-SWEC.

----

### **AI-SWEC: Grounding Your “Why”**

The AI-SWEC framework helps teams evaluate the fit and effectiveness of GenAI tools using four dimensions:

| Dimension         | Sample Questions |
|------------------|------------------|
| **Value Delivered**     | Did GenAI meaningfully reduce lead time or cognitive burden? Did it support reuse? |
| **Effort Required**     | How much prompting, revision, or integration work was needed? |
| **Risk Introduced**     | Did the tool create any insecure code, incorrect logic, or hallucinated output? |
| **Confidence Gained**   | Can the output be reviewed, tested, and trusted? Was traceability preserved? |

----

### **DevSecOps-Aligned Metrics**

To assess adoption and system health, consider:

#### Pipeline & Flow

- Time-to-merge for GenAI-generated code
- Percentage of PRs with GenAI attribution
- Prompt reuse rate (from prompt library or versioned prompts)

#### Security & Quality

- Post-generation defect rate (e.g., bugs detected in review or testing)
- Security issue density in GenAI-authored code (e.g., via SAST/DAST)
- Review delta: manual changes after GenAI suggestions

#### Compliance & Traceability

- SBOM/MLBOM inclusion of GenAI-generated artifacts
- Model source attribution logged (e.g., which model, version)
- Commit tags or inline comments indicating generated code

----

### **Team Feedback & Review Confidence**

Encourage structured team reflection:

- Did using GenAI improve the *experience* of coding, or just shift the work?
- What percentage of GenAI-generated code was accepted without revision?
- Do reviewers trust the output? Why or why not?

---

### **Pre-Adoption Guidance**

Before experimenting with GenAI tools, define your “why”:

- What phase of the SDLC are we targeting?
- Are we aiming to improve delivery speed, quality, or knowledge sharing?
- How will we know if it worked?

Use the AI-SWEC to create a shared hypothesis and evaluation plan.

> 📌 *Measure what matters. Don’t mistake code generation volume for mission progress.*


MITRE’s **AI Software Evaluation Criteria (AI-SWEC)** framework is a public-good resource developed to support this type of intentional, mission-aligned experimentation. Created by MITRE as part of its work as a Federally Funded Research and Development Center (FFRDC), AI-SWEC is freely available to U.S. government teams, allied partners, and the broader software engineering community.

AI-SWEC is not a productivity tracker or tool comparison matrix—it is a lightweight, outcome-focused decision support framework. It helps teams evaluate the adoption of GenAI tools based on:

- **Value Delivered**
- **Effort Required**
- **Risk Introduced**
- **Confidence Gained**

This framework is already in use by government and industry software teams to frame pilots, capture pre/post comparisons, and support trustable integration of GenAI tooling.

For facilitation support or templates, contact **ArchAITecture@mitre.org**.

----

### **Summary: What to Track Across the Lifecycle**

To evaluate GenAI-assisted code generation effectively, ensure your measures address:

- **Time-to-value vs. time-to-rework** – Are we gaining speed or just creating cleanup?
- **Prompt reuse and effectiveness** – Are teams learning and sharing effective interactions?
- **Code quality metrics** – Are GenAI outputs helping or harming code reliability and security?
- **Feedback into training and governance** – Are lessons learned shaping future tool use and policy?

These metrics complement AI-SWEC and reinforce mission-focused, evidence-based adoption.


## **8. Governance and Policy**

Governance is essential, but beyond the scope of this play. Full guidance on usage policies, enforcement mechanisms, audit trails, and Zero Trust alignment for GenAI-assisted development will be covered in a dedicated play: **Governance for Responsible GenAI Adoption** (coming soon).

> In the meantime, refer to **Home / Basics** and **Fundamentals for Designing an AI-Augmented Tool Chain** for baseline traceability and security design principles.

----

## **9. Training and Workforce Enablement**

Effectively adopting GenAI tools means developing fluency across roles—not just for software engineers.

A full guidance play titled **Building an AI-Augmented Workforce** will expand on how to grow capability across the AI-Curious to AI-Native spectrum, pairing, mentoring, and preventing skill atrophy.

> For now, see **Fundamentals for Designing an AI-Augmented Tool Chain** for prompt engineering and human-in-the-loop patterns.

----

## **11. What’s Emerging**

Agentic platforms, secure software platforms with embedded GenAI, and the shift to low-code and no-code generation are reshaping how code is authored, tested, and deployed.

A separate **Futures Watch** play will explore the implications of these patterns for architectural control, lifecycle governance, and workforce transformation.

> This play focuses on today’s practices. Refer to **Code Generation & Completion** for current implementation guidance.

----

## **12. Risks and Red Flags**

This play touches on common risks—hallucinated code, overreliance on opaque models, and IP concerns—but a more detailed exploration will be available in the **Risk Reference Companion** play.

> For architectural guidance on identifying and mitigating early-stage risks, refer to **Fundamentals for Designing an AI-Augmented Tool Chain** and the traceability patterns in **Code Generation & Completion**.

----

 **End of Play**