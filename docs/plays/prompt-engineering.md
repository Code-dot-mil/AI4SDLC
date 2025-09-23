---
# automatic badge generation
lifecycle: rc
last_updated: "2025-08-15"
---

# Play: Prompt Engineering Play

## I. Purpose & Scope

**Objective**: To establish foundational prompt engineering practices for DoD software teams, enabling secure, consistent, and auditable use of large language models (LLMs) in software development workflows.

**Primary Audience**: Software developers, security analysts, and program managers who are integrating or evaluating LLM tools within their development processes.

**Secondary Audience**: DevSecOps engineers, test leads, operations teams, and acquisition professionals using AI-assisted tools for documentation, compliance, or technical analysis.

**What This Playbook Covers**:
- Core principles and anatomy of effective prompts
- Security and compliance considerations for DoD environments  
- Advanced prompting techniques and when to apply them
- Role-specific examples across the software development lifecycle
- Risk mitigation strategies and common pitfalls

**What This Playbook Does NOT Cover**:
- AI model deployment, fine-tuning, or infrastructure management
- Organization-wide AI governance frameworks or policy development
- Procurement guidance and vendor evaluation for AI platforms
- Advanced topics like model training, embeddings, or vector databases

**Alignment**: This guidance supports the Department's strategic objectives including Zero Trust Architecture, DoD AI Ethical Principles, and secure software development practices outlined in DoDI 5000.82.

**Why This Matters Now**: DoD teams are rapidly adopting LLMs for code review, documentation generation, compliance assessment, and technical analysis. Without structured prompting practices, teams risk inconsistent outputs, security vulnerabilities, and compliance gaps. Early adoption of disciplined prompt engineering prevents these issues and establishes secure-by-design AI integration.

**Expected Outcomes**: After applying this guidance, teams will be able to:
- Write structured, repeatable prompts that produce consistent, mission-aligned outputs
- Apply appropriate security controls and risk mitigation strategies  
- Integrate prompt engineering into existing SDLC processes and tooling
- Avoid common prompting pitfalls that lead to hallucinations or compliance issues

This playbook provides the foundational knowledge needed to use LLMs effectively and securely within DoD software development constraints. For advanced topics like model deployment or enterprise AI governance, refer to specialized guidance documents.

> 📌 **Note**: This playbook covers prompt engineering across the entire software development lifecycle—not just code generation. Effective prompting is equally important for documentation, security assessment, infrastructure configuration, and compliance activities.

----

## II. Introduction to Prompt Engineering

### What Is Prompt Engineering?

In the context of Generative AI (GenAI), a prompt is text input in natural language provided to a large language model (LLM) to guide its behavior and generate a relevant, useful output. It is both the instruction and the context. **This is the critical mechanism** through which we shape model responses to support technical, analytical, and operational tasks.

Prompt engineering is the practice of crafting those inputs with precision and intentionality. A well-structured prompt provides the LLM with clear instructions, sufficient context, defined constraints, and an expected format—allowing it to produce more accurate, secure, and mission-aligned outputs.

> “Without clear and precise instructions, these models can produce irrelevant, biased, or overly general outputs.”
> — IBM Prompt Engineering Guide [^1]

In modern software value streams, prompt engineering is no longer an experimental skill—it is an operational discipline. From writing IaC policies to summarizing acquisition artifacts, prompts are increasingly how we interact with AI systems to support the SDLC.

There are three common structural approaches to prompting[^2]:

- **Direct Instructions** – Specific, task-oriented commands that tell the model what to do (e.g., “Summarize this technical report in under 150 words.”). These are concise and do not assume domain context.
- **Task-Specific Instructions** – Tailored prompts aligned to domain processes or roles (e.g., “Convert this JCIDS Capability Document into MoSCoW-prioritized user stories.”). These assume deeper context and are ideal for repeatable mission or enterprise tasks.
- **Open-Ended Instructions** – Used for ideation or exploratory analysis (e.g., “List risks associated with deploying LLMs in IL5 environments.”). These prompts leave room for the model to explore possible directions.

> 📌 **Clarification**: Task-specific prompts differ from direct instructions in that they assume and embed domain context. A direct instruction tells the model *what to do* in a general sense; a task-specific instruction frames the request in terms of an existing process, role, or structured artifact (e.g., acquisition workflow, STIG review, mission thread).

As with any engineering discipline, effectiveness improves with structure, governance, and iteration. This playbook provides the foundational techniques and patterns to apply prompt engineering with confidence across DoD software value streams.

[^1]: IBM, “The Prompt Engineering Guide,” *IBM Think Blog*. [Online]. Available: [https://www.ibm.com/think/topics/prompt-engineering-guide](https://www.ibm.com/think/topics/prompt-engineering-guide). \[Accessed: 20-Jun-2025].

[^2]: IBM, “Prompt Engineering Techniques,” *IBM Think Blog*. [Online]. Available: [https://www.ibm.com/think/topics/prompt-engineering-techniques](https://www.ibm.com/think/topics/prompt-engineering-techniques). \[Accessed: 15-Jun-2025].

### Why It Matters in the SDLC

Prompt engineering is crucial for the software development lifecycle (SDLC).  It directly impacts the quality, reliability, and efficiency of Generative AI functionality.  Well defined prompts enable AI models to understand user intent and produce the desired outcomes. This becomes particularly important as AI-augmentation is integrated into more stages of the SDLC, from requirements through deployment. 

Benefits to the AI-Augmented SDLC:

- **Reduces ambiguity** - Effective prompt engineering eliminates confusion between the human intent and the AI interpretation. This minimizes rework.
- **Accelerates documentation** - Concise prompt engineering streamlines the generation of documentation saving time and improving consistency. They can be used to quickly summarize technical documents as well.
- **Codifies expertise** - Prompt engineering captures domain knowledge and leading practices in a reusable format. This can help less experience team members to learn from an leverage collective knowledge from experienced team members. It also builds a reusable library of prompts for future projects.
- **Improves human/machine teaming** - The process of prompting and the assets created act as a shared language between the humans and the language models (the machine.) The iterative nature of prompt refinement and continuous feedback can make the workflow between humans and machines more intuitive.  

### Security & Policy Considerations

Prompt engineering, like all use of AI-Augmentation,  must be approached as a **security-sensitive activity**. In the DoD software environment, careless prompting could lead to unauthorized disclosures, model misuse, or generation of insecure artifacts. This section outlines key considerations to protect mission integrity, enforce compliance, and align with DoD cybersecurity strategy.

#### **Responsible Use of Commercial Models** 

A primary concern is preventing data leakage through commercial AI models that may train on government inputs. DoD organizations must establish contractual guarantees with vendors that government data remains isolated from training processes, commercial analysis.  In some instances, it may require implementation of  air-gapped solutions for classified environments.

It is an important step to carefully reviewing **End User License Agreements (EULAs)** and establish contractual controls that restrict commercial AI models from retaining government data and from training models based on inputs.  You should also understand any retention and secondary use.

#### **Leverage Approved Platforms**

Use government-authorized LLM deployments that meet IL-level hosting and accreditation requirements. Prompts should be tested and deployed on platforms that align with DoD security and compliance standards, especially when used in mission, weapon, or enterprise environments. 

> ==Do we have a list of approved platforms and LLMs? for DoD --> Check with Navy and HPC==

#### **Data Protection and Access Controls** 

To prevent leakage of sensitive or classified information,  prompts must never include Controlled Unclassified Information (CUI), Personally Identifiable Information (PII), or mission-sensitive details unless processed within appropriately cleared and accredited environments.  It is also imperative to respect classification boundaries, that is, do not rely on model outputs to infer or verify classification status.  Always confirm user access controls are enforced.  

Individuals also have a responsibility to honor  access controls and classification of data and documents

#### **Auditability and Traceability** 

Audit logs play an important role when leveraging AU-augmentation. It is important to log both inputs and outputs and include data such as userID and model version.  Remember that a prompt is an electronic asset.  Prompts used across the SDLC should support traceability and be linkable to tickets, commits, documents, or delivery artifacts.  

In some instances, you may be using and integrated development environment (IDE) which has AI-augmentation configured, installed or available withing the IDE. The IDE is the workbench tool where software engineers and developers create/generate software code, automated tests, and documentation. When operating within a local IDE, consider the following:

- Structuring code comments that include prompt references
- Configure tools to automatically include prompt metadata in commit messages / notes 
- Establish centralized prompt repositories

Additional IDE related guidance available for applying security principles when using in-IDE AI augmentation here: [IDE Security](ide-security.md).

#### **Adversarial prompting and red-teaming** 

Adversarial prompting refers to deliberately crafted inputs designed to bypass safeguards, extract sensitive information, or manipulate outputs in unintended ways or even induce unsafe behavior from AI systems.

In the DoD context, the technique called Red-teaming applies systematic testing methodologies to identify these vulnerabilities and simulate misuse as well as test the robustness of prompt guards that may be in place.  Security teams security teams attempt to exploit AI systems, and in the case of the SDLC, exploit Ai-augmented tooling through carefully constructed prompts that circumvent built-in protections. Regular red-teaming helps validate that prompt engineering practices and AI tools withstand manipulation and remain compliant with security policies.

In addition to defending against attacks, proactive security measures in prompt design are equally important:

- **Incorporate prompt red-teaming.** Test prompts for potential leakage, injection, or manipulation by adversaries.
- **Embed security-focused prefixes.** According to Kim et al. [^3], adding a security-focused prompt prefix reduced generated vulnerabilities by up to 56% in benchmark tasks.
- **Avoid anti-patterns.** Prompts that encourage shortcutting authentication, bypassing validation, or using deprecated cryptographic libraries must be flagged and rejected.

Detailed guidance on designing prompts is covered under Section [III. How to Write an Effective Prompt](#iii-how-to-write-an-effective-prompt) below.

Prompt engineering is foundational to secure and privacy-preserving AI integration in the SDLC. By embedding security and privacy considerations directly into prompt design and workflow, organizations can reduce ambiguity, accelerate documentation, codify expertise, and improve human-machine teaming—all while maintaining strict compliance with DoD standards and safeguarding sensitive information

These key frameworks—**Zero Trust**, **DoD AI Ethics**, **NIST RMF**, and **DoDI 5000.82**—must also be considered during prompt engineering. See [Mapping Prompt Engineering to DoD Cyber and AI Governance](prompt-engineering-and-security-policy.md)

Advanced techniques can include incorporating encryption into prompt design and designing prompting workflows that are secure by default.Prompt engineering is a secure-by-design behavior. Embedding secure-by-design behavior into the fabric of software value streams verifies that the benefits of LLMs are realized without introducing new operational or cybersecurity risks.

[^3] H. Kim, L. Wang, T. Kim, and S. Lee, “Benchmarking Prompt Engineering Techniques for Secure Code Generation with GPT Models,” in *Proceedings of the 2nd International Conference on Foundations of Responsible Generative AI (FORGE 2025)*, 2025. [Online]. Available: https://conf.researchr.org/details/forge-2025/forge-2025-papers/11/Benchmarking-Prompt-Engineering-Techniques-for-Secure-Code-Generation-with-GPT-Models. \[Accessed: 20-Jun-2025]

> 📌 *Prompts shape model behavior. Better prompts lead to better outcomes—securely, repeatably, and transparently.*


## III. How to Write an Effective Prompt

Now that we understand the importance of prompting and the risks involved, let’s get specific. This section outlines how to author high-quality prompts that drive mission alignment, increase explainability, and ensure reuse.

Prompt engineering is the practice of crafting precise, structured inputs that guide large language models (LLMs) to generate reliable, relevant, and secure outputs. In DoD environments, prompts must be treated as governed artifacts—subject to review, refinement, and reuse. A well-written prompt improves traceability, reduces risk, and ensures that generated artifacts support mission assurance.

### Prompt Engineering Principles for the Mission-Focused SDLC

Principles represent conceptual guidelines and create a framework for thinking about a topic like prompt engineering.  Principles enable practitioners to adapt their approach based on context and objectives while maintaining consistency with underlying quality factors. Stated differently, principles are the rules of engagements.  Let's start with a core set of prompt engineering principles:

- **Be Specific and Complete** – Provide precise instructions and sufficient context to reduce ambiguity and improve outcome quality.
- **Define Role and Intent** – Establish the AI’s role, expertise, and purpose to align responses with mission needs.
- **Clear Objectives and Boundaries** – Define the desired outcome and set explicit constraints for format, tone, and scope. When the goal is exploration or ideation, use open-ended prompts to broaden insight. When precision and compliance are critical, use tightly scoped directives.
- **Tailor to the Audience** – Adjust technical depth, language, and tone based on whether the output is for developers, analysts, operators, or decision-makers.
- **Use Structured Context and Output** – Organize inputs and expected outputs using lists, delimiters, or structured formats like JSON/YAML to guide response shape.
- **Refine Through Iteration and Versioning** – Continuously improve prompt effectiveness with testing and maintain version control for traceability and reproducibility.
- **Treat Prompts as Artifacts** – Manage prompts and output as part of the SDLC: link to issues, commits, and documentation to support Zero Trust and compliance.

### Anatomy of a High-Quality Prompt

A well-engineered prompt is more than a question—it’s a structured input that acts like a design artifact. In the DoD context, where prompts may influence everything from Infrastructure-as-Code (IaC) to CONOPS summaries or cyber policy generation, **clarity, control, and context** are essential. The components below can be combined to build consistently effective prompts for mission-focused outcomes.

> 📌 **Tip**: Think of prompts like test cases—they must be understandable, reproducible, and reviewable. They are design artifacts, not one-off conversations.


### 1. Role Definition

**What to do:** Assign the AI a role to establish its point of view and domain expertise.

**Why it matters:** This frames the model’s tone, terminology, and assumptions. In mission systems, role identity helps align AI reasoning with the expectations of security engineers, test leads, PMs, or policy authors.

**Prompt Pattern:**  
`You are a [specific role] with expertise in [domain]. Your primary responsibility is [function].`

**Example:**  
`You are a cybersecurity analyst trained on NIST 800-53 and DoD STIGs.`

---

### 2. Define the Goal

**What to do:** State the specific task using action verbs. Describe the intended outcome and, when helpful, include context or dependencies.

**Why it matters:** Action-oriented prompts reduce ambiguity and focus the model on a mission-aligned outcome.

**Prompt Pattern:**  
`Your task is to [action] that supports [objective].`

**Example:**  
`Evaluate this infrastructure-as-code file for security misconfigurations within a cloud-native container platform.`

---

### 3. Specify the Audience

**What to do:** Tailor the language, technical depth, and tone based on the expected consumer of the output.

**Why it matters:** Models adapt output style and complexity better when the audience is clear—this avoids wasted cycles or confusing jargon.

**Prompt Pattern:**  
`This output is intended for [audience].`

**Example:**  
`Prepare this response for a cross-functional engineering team familiar with DevSecOps practices.`

----

### 4. Provide Context and Input

**What to do:** Describe the scenario, system, or constraints and define the input structure.

**Why it matters:** Background enables better relevance and accuracy. Precise input formats prevent model confusion and increase reliability.

**Prompt Pattern:**  
`Given [situation/environment], process the following input [format]...`

**Example:**  
`This operates within an IL5 Kubernetes environment. The following input is a YAML deployment manifest.`

----

### 5. Set Constraints, Style, and Compliance

**What to do:** Define policies, formatting rules, or prohibited content.

**Why it matters:** Explicit constraints help reduce hallucination, keep outputs mission-safe, and enforce internal quality standards.

**Prompt Pattern:**  
`Do not [restrictions]. Limit to [format/style/length].`

**Example:**  
`Do not reference public GitHub repos. Limit response to 300 words. Use only DoD-approved terminology.`

----

### 6. Format the Result

**What to do:** Request specific formats such as JSON, checklist, or markdown.

**Why it matters:** Structured output enables immediate use, especially when integrated into tooling or compliance workflows. Output formatting should reflect the audience’s needs—for example, structured formats for engineers, or narrative summaries for policy leads.

**Prompt Pattern:**  
`Return the results as [format] with [fields].`

**Example:**  
`Return the results as a markdown checklist with inline annotations.`

----

### 7. (Optional) Reinforce with Examples and Delimiters

**What to do:** Add illustrative examples or delimiter scaffolds.

**Why it matters:** Examples improve clarity and help models generalize the pattern. Delimiters improve parseability and prompt reuse.

**Prompt Pattern:**  
```
### INPUT ###
[structured example input]

### OUTPUT ###
[desired structured output]
```

**Example:**
```
### INPUT ###
[Insert Terraform configuration]

### OUTPUT ###
- Detected STIG violations
- Remediation steps
```

### Assembly Pattern
**You don’t need every element every time**—but following this pattern improves reliability, security, and reproducibility.

```
[Role Definition]  
You are a [specific role] with expertise in [domain]. Your primary responsibility is [function].

[Define the Goal]  
Your task is to [action] that supports [objective].

[Specify the Audience]  
This output is intended for [audience].

[Provide Context and Input]  
Given [situation/environment], process the following input [format]...

[Set Constraints, Style, and Compliance]  
Do not [restrictions]. Limit to [format/style/length].

[Format the Result]  
Return the results as [format] with [fields].

[Optional: Examples and Delimiters]  
### INPUT ###  
[structured example input]  
### OUTPUT ###  
[desired structured output]
```

This pattern enables repeatable, compliant prompting across use cases—whether you're working on acquisition strategies, system modeling, or defensive cybersecurity tools.

> For readers applying prompt engineering to generate or complete code, refer to the dedicated Play: [Leading Practices for Code Completion and Generation](code-gen-play.md). It contains additional role-specific guidance, architectural guardrails, and usage patterns for secure software delivery in DoD environments.

## IV. Advanced Prompting Techniques

Advanced prompting techniques enhance the 7-component anatomy for specific cognitive tasks and complex scenarios. These patterns can be combined and modified to address sophisticated requirements in DoD environments.

### ⚙️ **Core Cognitive Patterns**

#### **1. Chain-of-Thought (CoT) Prompting**
A reasoning-based pattern that encourages the AI to "think out loud" through step-by-step logic. This increases transparency in complex problem-solving and mitigates hallucinations by making the reasoning process visible.

* **Use case**: Risk analysis, defect triage, threat modeling
* **Example**: `Walk through the logic of how a buffer overflow could occur in this C code.`

#### **2. Tree-of-Thought (ToT) Prompting**
An expansion of CoT that branches into multiple reasoning paths, enabling the AI to evaluate tradeoffs or alternatives. Particularly useful for decision analysis where multiple criteria must be balanced.

* **Use case**: Architectural decision analysis, requirements negotiation
* **Example**: `Generate 3 architectural options for IL5 Kubernetes deployment, then evaluate based on cost, security, and maintainability.`

#### **3. Self-Critique / Self-Refinement Prompting**
This technique asks the AI to review and improve its own outputs, enabling higher-quality generation, especially in technical or compliance-sensitive contexts.

* **Use case**: ATO packages, quality assurance
* **Example**: `Review the following Bash script for STIG compliance and suggest improvements based on DoD IA controls.`

### 🏗️ **Structured Context Patterns**

#### **4. CHOP Pattern (Context, History, Objective, Prompt)**
A structured prompting technique that explicitly defines the task context, relevant history, the user's objective, and the core prompt. This structure improves clarity and reduces ambiguity in outputs, especially for complex or multi-stage requests.

* **Use case**: Mission thread generation, architectural analysis
* **Example**:
  ```
  CONTEXT: A Flask-based DoD logistics API
  HISTORY: Uses JWT authentication, FIPS 140-3 encryption required
  OBJECTIVE: Add a new `/request/submit` endpoint that logs all access attempts
  PROMPT: Generate a secure Flask route that uses role-based access control and logs each submission in JSON.
  ```

#### **5. Context Windowing / Embedded Context Prompting**
Embedding supporting documents or code within the prompt, usually framed by delimiters. This keeps context tightly scoped and prevents the model from straying from the intended source material.

* **Use case**: Policy Q&A, source code interpretation
* **Example**:
  ```
  ### DoD Policy Extract ###
  [Policy content here]
  ###
  Based on this, what documentation is needed for RMF Step 4?
  ```

#### **6. RAG-Enhanced Prompting (Retrieval-Augmented Generation)**
A pattern that combines LLM reasoning with external knowledge retrieved from a vector database or document index. Ideal for high-assurance, mission-critical use where answers must be grounded in validated sources.

* **Use case**: Classified references, MBSE model interaction
* **Example**: `Using the mission thread stored in this vector index, summarize constraints affecting EW operations.`

### 🔗 **Workflow and Format Patterns**

#### **7. Prompt Chaining / Pipeline Prompting**
A technique where multiple linked prompts handle sequential tasks (e.g., requirements → user stories → tests). Chaining improves traceability, modularity, and reuse across SDLC phases.

* **Use case**: Requirements-to-code-to-test transformation
* **Example**:
  1. Extract features from transcript
  2. Generate user stories
  3. Generate unit tests for each story

#### **8. Zero-shot / Few-shot Prompting**
These techniques involve prompting the model with no (zero-shot) or minimal (few-shot) examples. Few-shot methods are helpful when the task format needs to be inferred through pattern recognition.

* **Use case**: Test case generation, bug triage, regex generation
* **Example (Few-shot)**:
  ```
  Input: '123-45-6789' → Output: SSN
  Input: 'john.doe@af.mil' → Output: Email
  Input: '10.10.10.1' → Output: ?
  ```

#### **9. Schema/Format-Constrained Prompting**
A pattern that explicitly requests outputs in a structured format, such as tables, JSON, or YAML. Useful for downstream parsing or integrations with dashboards and pipelines.

* **Use case**: Integrations, dashboards, output parsing
* **Example**: `Output a table in Markdown with columns: Risk ID, Severity, Mitigation.`

### 🔀 **Cross-Cutting Enhancement Techniques**

#### **10. Role-Based Prompt Pattern (RBPP): A Cross-Cutting Modifier **

RBPP is not a standalone prompt pattern. It’s a powerful modifier that enhances other techniques. By assigning the model a specific **mission-relevant role**, it shapes tone, domain language, compliance expectations, and the scope of acceptable reasoning.

* **Use case**: Adding domain expertise to any prompting pattern
* **Enhancement example**: 
  - **Basic CoT**: `Walk through how this vulnerability occurs`
  - **Role-Enhanced CoT**: `Act as a DoD cybersecurity analyst and walk through how this vulnerability could be exploited in an IL5 environment`

> **Role-based enhancement works with all patterns**. It can be combined with CoT for expert reasoning, CHOP for domain-specific context, or RAG for authoritative role-based retrieval. Think of it as a multiplier that adds domain credibility and context to any cognitive pattern.

### **Pattern Selection Guidance**

| **When you need...** | **Use this pattern** | **Consider enhancing with** |
|---------------------|---------------------|---------------------------|
| Step-by-step reasoning | Chain-of-Thought (CoT) | Role-based enhancement |
| Multiple solution evaluation | Tree-of-Thought (ToT) | Schema formatting |
| Complex background context | CHOP or Context Windowing | Role-based enhancement |
| Authoritative source grounding | RAG-Enhanced | Format constraints |
| Multi-stage workflows | Prompt Chaining | Self-critique validation |
| Consistent output format | Schema/Format-Constrained | Role-based enhancement |
| Quality improvement | Self-Critique | Any base pattern |

## V. Common Pitfalls and Anti-Patterns

Understanding what **not** to do is as important as following best practices. These anti-patterns frequently undermine prompt effectiveness in DoD environments, leading to inconsistent outputs, security risks, or compliance failures.

### **1. Vague Role Definition**

- **Problem:** Using generic expertise without domain specificity  
- **Anti-pattern:** "Act as an expert and review this code"  
- **Why it fails:** The AI has no context for what type of expertise or standards to apply  
- **Better approach:** `"Act as a DoD cybersecurity analyst certified in NIST 800-53 and familiar with DISA STIGs"`

### **2. Missing Audience Context**
- **Problem:** Producing outputs without considering the consumer  
- **Anti-pattern:** Technical implementation details for executive briefings  
- **Why it fails:** Misaligned complexity and terminology waste stakeholder time  
- **Better approach:** `"Format this for senior leadership (GS-15/SES) requiring executive summary with risk implications"`

### **3. Unrestricted Output Scope**

- **Problem:** Open-ended prompts that encourage hallucination  
- **Anti-pattern:** "Tell me about security vulnerabilities"  
- **Why it fails:** Leads to generic, potentially inaccurate information without actionable specifics  
- **Better approach:** `"Identify OWASP Top 10 vulnerabilities in this Node.js application with specific remediation steps"`

### **4. Ignoring Output Format Requirements**

- **Problem:** Requesting analysis without specifying structure  
- **Anti-pattern:** "Analyze this system for compliance issues"  
- **Why it fails:** Produces narrative text that's difficult to integrate into reports or track  
- **Better approach:** `"Return compliance findings as a table with Control ID, Status, Risk Level, and Remediation Steps"`

### **5. Mixing Classification Levels**

- **Problem:** Including sensitive context in prompts for commercial models  
- **Anti-pattern:** Referencing specific mission names, locations, or system details  
- **Why it fails:** Potential unauthorized disclosure and policy violations  
- **Better approach:** `Use sanitized examples or process within appropriate IL-level environments`

### **6. No Constraints or Guardrails**

- **Problem:** Failing to set boundaries on AI behavior  
- **Anti-pattern:** Allowing suggestions of unapproved tools or non-compliant practices  
- **Why it fails:** Outputs may recommend solutions outside DoD policy or procurement constraints  
- **Better approach:** `"Use only DISA-approved tools and reference FedRAMP-authorized solutions"`

### **7. Single-Shot Complex Tasks**

- **Problem:** Asking for multiple complex deliverables in one prompt  
- **Anti-pattern:** "Generate requirements, design the architecture, and create test plans"  
- **Why it fails:** Reduces quality of each deliverable and makes validation difficult  
- **Better approach:** `Use prompt chaining for sequential tasks with validation points between steps`

### **8. Assuming Model Knowledge Currency**

- **Problem:** Expecting AI to know the latest policies or technical updates  
- **Anti-pattern:** "Apply the latest CMMC requirements" without providing current guidance  
- **Why it fails:** Models may reference outdated information or hallucinate recent changes  
- **Better approach:** `Provide current policy extracts or explicit version references`

## **Risk Mitigation Checklist**

Before deploying prompts in production workflows, verify:
- [ ] Role and domain expertise clearly defined
- [ ] Audience and output format specified  
- [ ] Appropriate constraints and compliance boundaries set
- [ ] No sensitive information included in prompt text
- [ ] Expected output scope is bounded and testable
- [ ] Authoritative sources referenced when needed
- [ ] Prompt is versioned and traceable to requirements

----

## VI. Applying the Framework by Role

This introduces role-specific guidance for designing effective prompts across the software development lifecycle (SDLC). Each role brings unique mission objectives, technical touchpoints, and trust boundaries. Each *example* follows this structure:

- **Role** – The SDLC stakeholder (e.g., Developer, Security Analyst) whose responsibilities shape the prompt’s tone, detail, and purpose.  
- **Use Case** – A real task or mission scenario where prompting improves speed, quality, or compliance.  

- **Leading Practice** – A proven prompting technique that enables structured, secure, and reusable outputs.

- **Risk Mitigation** – Guardrails to reduce hallucination, data leakage, noncompliance, or deviation from standards.

This structure moves prompting from ad hoc experimentation toward structured, repeatable design..

### Examples "At-a-Glance"

This is a quick summary of the five role-based examples that will be detailed subsequently.

| **Role**                          | **Use Case**                                                 | **Key Emphasis Areas**                                   |
|----------------------------------|--------------------------------------------------------------|----------------------------------------------------------|
| A - Capability Sponsor / Requirements Owner | Drafting capability statements aligned with JP 3-0 and IL5 hosting constraints | Doctrine, Goal Clarity, Compliance Language, Audience |
| B - Developer                    | IaC security review with Terraform                           | Role, Task, Format, Compliance                           |
| C - Security Analyst             | Log anomaly detection for IL5 environments                   | Constraints, Format, Examples                            |
| D - Project Manager              | Executive-ready AI-generated SITREP or progress update       | Audience, Goal, Output Control                           |
| E - Operations Engineer          | Diagnosing runtime issues in containerized workloads          | Context, Role, Task, Structured Input/Output             |

>📌 Note: Each example will include all 7 prompt components of the prompt framework.  All components are not always necessary.  These are intended to provide holistic examples.

### A. Capability Sponsor / Requirements Owner Sample Prompt

- **Role** – Capability Sponsor / Requirements Owner  
- **Use Case** – Operational Capability Statement Generation – Drafting requirements aligned to doctrinal sources like JP 3-0, incorporating mission-specific constraints such as IL5 compliance  
- **Leading Practice** – Use doctrinal vocabulary and outcome-focused tasking to frame capabilities in mission-aligned language  
- **Risk Mitigation** – Ground outputs in authoritative sources to reduce doctrinal drift and clarify dependencies or environmental assumptions  

#### 1. Role Definition

<pre>
You are a capability sponsor responsible for articulating mission-aligned requirements and translating operational needs into formal capability statements. You are fluent in joint doctrine, including JP 3-0 and CJCSI 3170.01I, and understand the technical and hosting constraints of DoD IL5 cloud environments.
</pre>

#### 2. Define the Goal

<pre>
Draft a capability requirement aligned with joint operational doctrine that describes the mission objective and includes relevant constraints such as IL5 hosting. Incorporate considerations for availability, resilience, and integration with existing mission systems.
</pre>

#### 3. Specify the Audience

<pre>
This requirement will be reviewed by both acquisition strategy leads and system architects responsible for downstream technical decomposition and risk evaluation.
</pre>

#### 4. Provide Context and Input

<pre>
The sponsoring command has identified a capability gap in rapid, theater-wide coordination of logistics assets across Joint and Coalition partners. The goal is to define a cloud-native decision support capability that operates in disconnected, intermittent, and limited (DIL) connectivity conditions and complies with IL5 hosting and data handling constraints.

### INPUT ###
[Insert operational context or mission scenario]
### OUTPUT ###
</pre>

#### 5. Set Constraints and Compliance

<pre>
- Reference only authoritative sources such as JP 3-0 and CJCSI 5123.01
- Use plain language with embedded doctrinal terms where appropriate
- Do not suggest acquisition solutions or specific vendors
- Ensure compliance language aligns with DoD IL5 security requirements
</pre>

#### 6. Format the Result

<pre>
Return the requirement as a structured narrative with:
- Capability Title
- Operational Need Statement
- Capability Description
- Mission Threads or Use Scenarios
- Known Constraints and Assumptions
</pre>

#### 7. Reinforce with Examples and Delimiters

<pre>
### EXAMPLE OUTPUT ###
**Capability Title:** Joint Logistics Situational Awareness (JLSA)  
**Operational Need:** Enable synchronized, real-time tracking and planning of Joint logistics operations across domains in contested environments  
**Capability Description:** A cloud-native platform that aggregates multi-domain logistics data and enables decision-makers to plan, reallocate, and prioritize sustainment under IL5 constraints  
**Constraints:** Must operate in DIL environments, support IL5-compliant hosting, and integrate with GCCS-J and Global Combat Support System (GCSS) APIs
</pre>
----

### B. Developer Sample Prompt: Infrastructure-as-Code Security Review

- **Role** – Developer  
- **Use Case** – Code Security Review – Evaluating Terraform configuration for compliance with DoD cloud security standards  
- **Leading Practice** – Multi-stage prompting with specific domain expertise  
- **Risk Mitigation** – Prevent hallucination of security standards and ensure alignment with compliance requirements  

#### 1. Role Definition

<pre>
You are a senior DevSecOps engineer with expertise in NIST 800-53 controls, DoD STIGs, and cloud security best practices. Your primary responsibility is identifying security misconfigurations in infrastructure-as-code that could create compliance violations or operational vulnerabilities.
</pre>

#### 2. Define the Goal

<pre>
Analyze the provided Terraform configuration for security misconfigurations, policy violations, and STIG compliance issues. Identify specific violations and provide actionable remediation steps.
</pre>

#### 3. Specify the Audience

<pre>
This analysis is intended for a development team with intermediate Terraform experience working on IL4/IL5 systems requiring FedRAMP compliance.
</pre>

#### 4. Provide Context and Input

<pre>
This Terraform configuration deploys a microservices architecture to AWS GovCloud for a mission-critical application processing CUI data. The environment must comply with NIST 800-53 moderate baseline and relevant DoD STIGs.

### INPUT ###
[Insert Terraform configuration file]
### OUTPUT ###
</pre>

#### 5. Set Constraints and Compliance

<pre>
- Reference only authoritative sources (NIST 800-53, DoD STIGs, FedRAMP baselines)
- Do not suggest commercial tools without explicit approval
- Limit response to 500 words
- Use DoD-approved terminology for security controls
- Flag any findings that could impact Authority to Operate (ATO)
</pre>

#### 6. Format the Result

<pre>
Return results as a structured markdown report with:
- Executive summary of critical findings
- Detailed findings table with STIG/NIST control references
- Remediation code snippets
- Risk severity ratings (Critical/High/Medium/Low)
</pre>

#### 7. Reinforce with Examples and Delimiters

<pre>
### FINDING EXAMPLE ###
**Control:** NIST 800-53 SC-7 (Boundary Protection)  
**Issue:** Security group allows unrestricted inbound access (0.0.0.0/0) on port 22  
**Risk:** Critical  
**Remediation:** Restrict SSH access to specific IP ranges or VPN endpoints
</pre>
----

### C. Security Analyst Sample Prompt: STIG/FISMA Compliance Checklist Generation

* **Role** – Security Analyst
* **Use Case** – STIG/FISMA Checklist Generation for ATO Support
* **Leading Practice** – Use structured prompts to generate repeatable, auditable checklists based on authoritative controls
* **Risk Mitigation** – Reduce hallucination by anchoring to approved STIG families and DoD guidance; limit output scope and enforce checklist formats

#### 1. Role Definition

<pre>
You are a cybersecurity analyst supporting DoD system ATO preparation. You specialize in interpreting STIG guidance, FISMA requirements, and DISA controls to assess compliance posture and generate audit-ready artifacts.
</pre>

#### 2. Define the Goal

<pre>
Generate a detailed compliance checklist based on the latest DISA STIGs for a RHEL 8 server, aligned with FISMA moderate controls. Identify unmet requirements and include mitigation notes for open items.
</pre>

#### 3. Specify the Audience

<pre>
This output is intended for an ISSO preparing documentation for an RMF package and presenting findings to the security controls assessor (SCA).
</pre>

#### 4. Provide Context and Input

<pre>
The system is hosted in an IL5 cloud environment and supports processing of CUI data. The checklist must reflect Red Hat Enterprise Linux 8 STIG v3r2 requirements.

### INPUT ###
[Insert scanned configuration or assessment output]
### OUTPUT ###
</pre>

#### 5. Set Constraints and Compliance

<pre>
- Reference only official DISA STIG guidance (https://public.cyber.mil/stigs/)
- Do not suggest unapproved tools or remediation outside the DISA ecosystem
- Output should use checklist format with Control ID, Description, Status, and Comments
- Limit total length to 1,000 words
</pre>

#### 6. Format the Result

<pre>
Return results as a markdown-formatted checklist including:
- STIG ID (e.g., RHEL-08-010010)
- Control description
- Compliance status (Compliant, Not Compliant, Not Applicable)
- Comments and mitigation strategy (if not compliant)
</pre>

#### 7. Reinforce with Examples and Delimiters

<pre>
### CHECKLIST ENTRY EXAMPLE ###
**STIG ID:** RHEL-08-010010  
**Description:** The operating system must enable FIPS mode.  
**Status:** Not Compliant  
**Comments:** System FIPS mode is disabled. Remediation scheduled in Q3 maintenance window.
</pre>
----

### D. Program Manager Sample Prompt: CMMC Status Report Generation

* **Role** – Program Manager
* **Use Case** – Compliance Reporting – Drafting weekly updates on CMMC progress and blocker resolutions
* **Leading Practice** – Time-box prompts and define desired output formats (bullet summary, table, narrative)
* **Risk Mitigation** – Reduce verbosity, hallucinated milestones, or misrepresented compliance posture through scoped, auditable responses

#### 1. Role Definition

<pre>
You are a Program Manager responsible for overseeing technical delivery, risk management, and compliance reporting across multiple workstreams in a DoD Agile acquisition program. You are familiar with CMMC Level 2 requirements, RMF workflows, and stakeholder communications for IL5 systems.
</pre>

#### 2. Define the Goal

<pre>
Generate a weekly status update highlighting progress toward CMMC Level 2 compliance, including resolved blockers, active risks, and any new artifacts generated this sprint. Summarize this in a format suitable for executive review.
</pre>

#### 3. Specify the Audience

<pre>
This output is intended for senior leadership (GS-15/SES) overseeing cybersecurity compliance, as well as contracting officers and government leads tracking milestone alignment and authorization readiness.
</pre>

#### 4. Provide Context and Input

<pre>
The system is undergoing phased CMMC readiness and has completed 6 of 17 domains. Recent sprint activities included control implementation reviews, POA&M updates, and team onboarding to a new security automation platform.

### INPUT ###
[List of sprint accomplishments, unresolved risks, and artifacts generated]
### OUTPUT ###
</pre>

#### 5. Set Constraints and Compliance

<pre>
- Limit the output to 400 words
- Organize information by domain (Access Control, Configuration Management, etc.)
- Avoid speculative claims or unstated assumptions about control maturity
- Use CMMC-defined terminology and reference relevant control identifiers
- Include only verified artifacts or actions completed during the reporting period
</pre>

#### 6. Format the Result

<pre>
Return results as an executive summary containing:
- Overall compliance status (numeric and narrative)
- Sectioned highlights by CMMC domain
- Tabular risk summary (risk, impact, mitigation status)
- Bullet list of artifacts submitted this week
</pre>

#### 7. Reinforce with Examples and Delimiters

<pre>
### CMMC WEEKLY STATUS EXAMPLE ###
**Compliance Summary:**  
- Completed: 6 of 17 domains  
- In Progress: 4 domains  
- Risks: 2 open, 1 mitigated

**Access Control:**  
- Completed AC.1.001 and AC.1.002  
- Risk: Delayed MFA integration for contractor systems  
- Artifact: Access Policy v1.3 (uploaded to eMASS)

**Risk Summary Table:**  
| Risk ID | Domain | Description                        | Impact | Mitigation Status |
|---------|--------|------------------------------------|--------|-------------------|
| R-019   | AC     | MFA delay on external systems      | High   | In Progress       |
| R-020   | CM     | Incomplete change tracking policy  | Medium | Mitigated         |
</pre>

----

### E. Operations Engineer Sample Prompt: Diagnosing Runtime Issues in Containerized Workloads

* **Role** – Operations Engineer
* **Use Case** – Diagnosing runtime issues in containerized workloads across IL environments
* **Leading Practice** – Use role-defined context with structured logs and environment metadata to focus diagnostic reasoning
* **Risk Mitigation** – Limit scope to in-scope logs, constrain external references, and enforce consistent output structure to avoid drift or unsupported assumptions

#### 1. Role Definition

<pre>
You are a site reliability engineer supporting containerized workloads running in IL5 Kubernetes clusters. Your primary responsibility is diagnosing service disruptions, latency spikes, and resource contention across application pods.
</pre>

#### 2. Define the Goal

<pre>
Analyze runtime logs and environment metadata to identify root cause of service degradation impacting container startup and response times. Recommend next troubleshooting steps and possible mitigation actions.
</pre>

#### 3. Specify the Audience

<pre>
This output is intended for a cross-functional incident response team including developers, operations engineers, and security monitors during post-incident review.
</pre>

#### 4. Provide Context and Input

<pre>
The environment includes a Kubernetes cluster hosted in AWS GovCloud with IL5 controls enabled. The affected service is a mission-support API used during operational readiness assessments.

### INPUT ###
- Container logs (last 5 minutes)
- Pod metadata (CPU/mem limits, restart count)
- Deployment YAML
- Service mesh (Envoy) metrics

### OUTPUT ###
</pre>

#### 5. Set Constraints and Compliance

<pre>
- Do not reference commercial SaaS observability tools unless pre-cleared
- Limit response to 750 words
- Flag any issues that might trigger a continuity of operations (COOP) response
- Provide plain language summary for non-engineers
</pre>

#### 6. Format the Result

<pre>
Return a structured report including:
- Root cause hypothesis
- Timeline of relevant log events
- System components impacted
- Suggested next steps (e.g., isolate pod, restart container, modify autoscaler thresholds)
</pre>

#### 7. Reinforce with Examples and Delimiters

<pre>
### DIAGNOSTIC SUMMARY ###
**Root Cause:** Resource limits exceeded due to unthrottled initialization routines  
**Log Evidence:** Init container terminated with OOMKill at 10:42:17 UTC  
**Impacted Components:** api-service pod (namespace: ops-readiness)  
**Recommended Action:** Adjust memory limits, instrument retry backoff for init tasks
</pre>

-----
## VII. Practices, Tools, Patterns, and Resources

### Prompt Review and Reuse

Prompts are not disposable. They should be versioned, shared, and curated over time to improve effectiveness and reduce redundant experimentation.

Consider using tools like:

- **LangSmith** or **PromptLayer** for prompt observability and versioning
- **Git repositories** to track prompt evolution
- **Prompt libraries** for team-wide reuse and learning

### Measuring Prompt Engineering Effectiveness

Effective prompt engineering should be measurable and continuously improved. Track these key indicators to validate your team's prompt engineering effectiveness:

#### **Prompt Quality Metrics**

- **Prompt clarity score**: How often prompts require clarification or additional context from users
- **Instruction completeness**: Percentage of prompts that include all 7 framework components (role, goal, audience, context, constraints, format, examples)
- **Prompt reusability rate**: How often prompts can be adapted across similar use cases without major revision
- **Component effectiveness**: Which prompt elements (role definition, constraints, examples) most improve output quality

#### **Prompt Iteration Metrics**

- **Revision cycles per prompt**: Average number of refinements needed before a prompt produces consistent, quality outputs
- **First-attempt success rate**: Percentage of new prompts that produce usable output on initial deployment
- **Prompt refinement velocity**: Time from initial prompt draft to production-ready version
- **Template adoption rate**: How quickly teams adopt and customize proven prompt patterns

#### **Prompt Governance Metrics**

- **Security compliance rate**: Percentage of prompts that follow DoD security guidelines (no CUI, proper constraints, approved platforms)
- **Version control adoption**: How consistently teams version and document prompt changes
- **Anti-pattern detection**: Frequency of prompts exhibiting known anti-patterns (vague roles, missing constraints, etc.)
- **Prompt review coverage**: Percentage of prompts that undergo peer review before production use

#### **Prompt Library Maturity**

- **Cross-team reuse**: How often prompts developed by one team are successfully adopted by others
- **Pattern standardization**: Consistency in applying the 7-component framework across different roles and use cases
- **Knowledge capture**: How effectively prompts codify domain expertise for reuse
- **Template coverage**: Percentage of common SDLC tasks with standardized, tested prompt templates

#### **Implementation Guidance**

- **Start Simple**: Begin by tracking prompt revision cycles and reuse rates before expanding to comprehensive measurement.

- **Baseline Establishment**: Document current prompting practices (if any) and time spent on prompt creation vs. refinement.

- **Regular Review**: Conduct monthly prompt retrospectives focusing on which patterns work best for different roles and use cases.

- **Version Control Integration**: Track prompt evolution alongside code changes to understand what prompt modifications correlate with better outcomes.

<!--  > 📊 **For detailed measurement frameworks, prompt testing strategies, and advanced prompt analytics, see**: [Measuring AI Effectiveness in Software Development](ai-metrics-measurement.md) -->


## VIII. Resources and References

The following resources offer practical and evolving guidance for writing effective AI prompts. While tailored for different use cases, many principles (e.g., specificity, context-setting, declarative language) are applicable across DoD environments.

| **Source**             | **Guide Title**                                                                                     | **Notes**                                                                 |
|------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| Microsoft              | [Prompt engineering 101](https://learn.microsoft.com/en-us/training/modules/introduction-to-copilot/) | Practical patterns, risk considerations, and use case structure for GitHub Copilot |
| Google DeepMind        | [What is Prompt Engineering? (Google Cloud)](https://cloud.google.com/discover/what-is-prompt-engineering) | Emphasizes examples, constraints, and prompt testing in code generation  |
| LangChain / LangSmith  | [Prompt Engineering Quick Start (LangSmith UI)](https://docs.smith.langchain.com/prompt_engineering/quickstarts/quickstart_ui) | Versioning, testing, and observability in structured pipelines            |
| AcqBot (GovPrompt)     | [Prompt Engineering Crash Course for Government](https://acqbot.com/blog/prompt-engineering-crash-course-for-government) | Role-based prompt walkthroughs tailored to government use cases; emphasizes structure and risk mitigation |
| Anthropic              | [Anthropic Prompt Library](https://docs.anthropic.com/en/resources/prompt-library/library)          | Library of categorized prompts for Claude models with secure design examples |
| OpenAI                 | [OpenAI Prompt Examples](https://platform.openai.com/docs/examples)                                 | Code-focused examples showing real-world prompting for common use cases   |
| West Point             | [GenAI Prompting Library](https://library.westpoint.edu/GenAI/prompting)                            | Educational resource with prompt structure examples aligned to military education |

These guides reinforce that prompting is not just an art.  It's an engineering discipline, especially in regulated or mission-critical environments.

----
