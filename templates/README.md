# AI4SDLC PromptOps Starter Pack — v0.3 (GitLab Hardened)

**Goal:** give teams a **drop‑in, tool‑agnostic** way to integrate AI into the SDLC **safely and repeatably**—without forcing a specific model/vendor—by standardizing:
- **guardrails** (what AI must never do, how to handle uncertainty, when to escalate),
- **agent “role cards”** (consistent inputs/outputs per SDLC function),
- **prompt templates** (bounded, reusable SDLC workflows),
- **lightweight intake/review forms** (repeatable human accountability),
- and (optionally) **GitLab-native** wiring for GitLab Duo + MRs + CODEOWNERS + simple CI checks.

This pack is designed for broad adoption (including restricted/regulated environments) and emphasizes:
- **No secrets / no internal identifiers** in AI inputs/outputs unless explicitly approved.
- **Truthfulness:** label uncertainty as **UNVERIFIED**.
- **Human-in-the-loop:** AI drafts; humans review and approve.
- **Minimal overhead:** start small, add enforcement only where it pays off.

---

## Who this is for

- Teams piloting AI-assisted SDLC (requirements → design → security → code → test → docs).
- Security/DevSecOps teams who need **repeatable guardrails** that scale across many repos.
- Organizations that need a **portable baseline** that works whether you use GitLab Duo, Ask Sage, ChatGPT, Claude, local models, or “no AI tool yet”.

---

## What this pack is (and is not)

### This pack **is**
- A **text-first PromptOps baseline**: policies, agent instructions, and prompts you can copy/paste into your AI tool or keep in-repo for consistent use.
- A **governance pattern**: CODEOWNERS + MR templates + review checklists to keep humans accountable.
- A **GitLab overlay**: optional files that make the baseline “native” in GitLab and steer GitLab Duo behavior.

### This pack **is not**
- A magic “AI in CI/CD” engine.
- A replacement for your organization’s data classification policy, RMF/NIST controls, or secure coding standards.
- A guarantee that an AI tool will never leak information. The pack reduces risk, but you still need training + sane repo hygiene + approvals + (optionally) deterministic checks.

---

## Contents (exact folder layout)

```
.ai4sdlc/
  AGENTS/
    Code_Review_Risk_Reviewer_Agent.md
    DevSecOps_Control_Mapper_Agent.md
    Red_Team_Prompting_Agent.md
    Requirements_Acceptance_Criteria_Agent.md
    Security_Analyst_Agent.md
    Test_Planner_Agent.md
  FORMS/
    ai_task_request.yaml
    ai_review_checklist.yaml
  POLICY/
    autonomy_levels.md
    guardrails.md
    guardrails.yaml
  PROMPTS/
    REQ_01_feature_intake.md
    DES_01_architecture_options.md
    SEC_01_threat_model_lite.md
    SEC_02_ai_feature_abuse_suite.md
    CODE_01_generate_change.md
    TEST_01_test_plan.md
    REVIEW_01_code_risk_review.md
    DOC_01_update_docs.md
  REVIEW_TEMPLATES/
    gitlab_merge_request_template_AI_Assisted_Change.md
    github_pull_request_template.md
    github_issue_template_ai_task.yml

platform_overlays/
  gitlab/
    README.md
    AGENTS.md
    CODEOWNERS
    .gitlab/
      duo/
        chat-rules.md
        mr-review-instructions.yaml
      issue_templates/
        AI_Task_Request.md
      merge_request_templates/
        AI_Assisted_Change.md
      ci/
        ai4sdlc-safety.yml

README.md
CHANGELOG.md
```

---

# Part A — Platform-agnostic core (`.ai4sdlc/`)

The `.ai4sdlc/` directory is the **portable baseline**. You can use it with *any* AI tool.

## 1) POLICY — Guardrails & Autonomy Levels

### `.ai4sdlc/POLICY/guardrails.md` and `guardrails.yaml`
These are the **non-negotiables**:
- **Data handling:** no secrets, no credentials, no tokens; avoid internal identifiers unless explicitly approved.
- **Truthfulness:** if you can’t point to evidence, label the claim **UNVERIFIED**.
- **Outputs:** structured results with assumptions/risks/verification steps.
- **Escalation triggers:** stop and ask a human on high-risk topics (auth, crypto, logging/auditing, bypass controls, etc.).

**How to implement:**
- Put these guardrails in front of every AI interaction:
  - copy/paste them into your AI tool’s “custom instructions”, “system prompt”, or “rules” feature
  - or keep them as the authoritative source in the repo and reference them in your agent files.

**Tip:** Treat `guardrails.yaml` as a machine-readable version you can later use for automated checks, policy engines, or structured validation.

### `.ai4sdlc/POLICY/autonomy_levels.md`
This is your **AI authority contract**:
- L0: advice only
- L1–L2: draft artifacts / propose changes (recommended baseline)
- L3–L5: progressively more autonomy (generally not recommended without formal governance)

**How to implement:**
- Pick the baseline (usually L1–L2).
- In your repo, add a short policy statement: “This project uses AI autonomy Level X.”

---

## 2) AGENTS — Role cards (repeatable AI behaviors)

Agent files are **portable role definitions** that you can apply to any model/tool. Each agent:
- states purpose and scope,
- reiterates the guardrails,
- defines required inputs,
- defines the expected output format,
- includes “stop and ask” triggers.

### Included agents
- **Security_Analyst_Agent.md** — security analysis, risk identification, mitigations, verification steps.
- **DevSecOps_Control_Mapper_Agent.md** — map “principles” → concrete controls; suggest where automation vs manual review belongs.
- **Requirements_Acceptance_Criteria_Agent.md** — clean requirements, acceptance criteria, edge cases, non-functional needs.
- **Code_Review_Risk_Reviewer_Agent.md** — risk-focused review: auth, crypto, logging, secrets, input validation, least privilege.
- **Test_Planner_Agent.md** — test plan with coverage intent, negative tests, boundary cases, evidence expectations.
- **Red_Team_Prompting_Agent.md** — adversarial prompts/abuse scenarios to pressure-test AI features.

### How to use an agent (any tool)
1. Open the agent file.
2. Copy its content into your AI tool as the “role/instructions”.
3. Provide the required inputs (repo context, feature summary, code diff, constraints).
4. Require the agent to output in the specified structure.
5. Commit resulting artifacts to the repo (docs/ai/…) and route through human review.

---

## 3) PROMPTS — SDLC prompt templates (bounded workflows)

These are **ready-to-run** templates. The idea is to make AI output repeatable and auditable by:
- constraining scope,
- forcing explicit assumptions,
- requiring verification steps,
- and producing outputs that fit into normal SDLC artifacts.

### Included prompt templates
- **REQ_01_feature_intake.md** — structured feature intake (scope, users, constraints, risks, acceptance criteria).
- **DES_01_architecture_options.md** — architecture options with tradeoffs and decision drivers.
- **SEC_01_threat_model_lite.md** — small, practical threat model (assets, trust boundaries, threats, controls, verification).
- **SEC_02_ai_feature_abuse_suite.md** — adversarial abuse cases for AI features (prompt injection, data leakage, policy bypass).
- **CODE_01_generate_change.md** — produce a bounded change plan + patch suggestions + validation steps.
- **TEST_01_test_plan.md** — test plan aligned to risk and change scope.
- **REVIEW_01_code_risk_review.md** — structured code risk review with explicit “UNVERIFIED” handling.
- **DOC_01_update_docs.md** — documentation update prompt with truthfulness rules.

### How to use prompts
1. Pick the prompt for your SDLC phase.
2. Fill in the bracketed fields.
3. Run it with your AI tool.
4. Save output into repo (recommended paths below).
5. Route through human review and approvals.

---

## 4) FORMS — Lightweight intake & review (YAML)

These forms are meant to be “paperwork that doesn’t suck”:
- **ai_task_request.yaml** — captures intent, inputs, data sources, prohibited content, outputs, and required reviewers.
- **ai_review_checklist.yaml** — captures what a human reviewer must check (truthfulness, safety, tests/evidence, risk areas).

**How to use:**
- Copy into an issue template or internal wiki workflow.
- Or keep them in-repo and link them from MRs/issues as the “definition of done” for AI-assisted work.

---

## 5) REVIEW_TEMPLATES — Optional platform templates

These are “starter” templates you can copy into your platform’s native templates:
- GitLab MR template
- GitHub PR template
- GitHub issue template

If you use GitLab, the overlay provides the native `.gitlab/...` versions already.

---

# Part B — GitLab overlay (Duo + Templates + Hardened rules)

If you use GitLab, copy the overlay to make this feel native and reduce manual friction.

## What the overlay adds
- **`AGENTS.md` (repo root)** — GitLab Duo-friendly “project-wide” instructions.
- **`.gitlab/duo/chat-rules.md`** — Duo Chat constraints (including **redaction rules**).
- **`.gitlab/duo/mr-review-instructions.yaml`** — Duo Code Review Flow standards (guardrails applied during review).
- **`CODEOWNERS`** — protects the governance files so they can’t be silently weakened.
- **Issue/MR templates** — consistent intake + accountability.
- **Optional CI safety scan** — deterministic grep-based redaction guard.

## GitLab install steps (copy/paste friendly)

### Step 1 — Copy the core pack
In your repo root:
- Copy `.ai4sdlc/` into the repo
- Commit to your default branch (e.g., `main`)

### Step 2 — Apply the GitLab overlay
Copy everything from `platform_overlays/gitlab/` into your repo root:
- `AGENTS.md` → repo root
- `CODEOWNERS` → repo root
- `.gitlab/**` → repo root `.gitlab/`

Commit these changes.

### Step 3 — Fix CODEOWNERS placeholders
Open `CODEOWNERS` and replace placeholder owners (example groups like `@security-team`) with **real** groups/users in your GitLab instance.

**If you don’t do this, the “governance protection” demo will be weak.**

### Step 4 — Enforce approvals (recommended)
In GitLab project settings:
- Protect your default branch (e.g., `main`)
- Require at least 1 approval for merge
- If available, enable **Require approval from code owners**

### Step 5 — Ensure templates show up
GitLab will auto-detect:
- `.gitlab/issue_templates/AI_Task_Request.md`
- `.gitlab/merge_request_templates/AI_Assisted_Change.md`

Use them when creating new issues/MRs.

---

## Optional: enable the CI “redaction safety scan”
This is the deterministic complement to “AI promises it didn’t leak anything”.

### What it does
`.gitlab/ci/ai4sdlc-safety.yml` adds a job that scans these paths:
- `AGENTS.md`
- `.ai4sdlc/`
- `.gitlab/duo/`
- `.gitlab/issue_templates/`
- `.gitlab/merge_request_templates/`
- `docs/ai/`

It fails the pipeline if it finds obvious patterns like:
- email addresses
- private IPs
- internal TLD hostnames (`.lan`, `.local`, `.internal`, `.corp`)
- `user_id:` / `project_id:` style leaks

### How to enable
Add to your repo’s `.gitlab-ci.yml`:

```yaml
include:
  - local: ".gitlab/ci/ai4sdlc-safety.yml"
```

### Important tuning notes
- This job is intentionally simple and will create false positives in some environments.
- If your org uses `.local` for legitimate docs, remove that pattern or narrow the scan to `docs/ai/**` only.
- You can also add patterns for your environment (internal domains, known project prefixes, etc.).

---

# Recommended repo conventions (to make this usable at scale)

## Where to store AI-assisted artifacts
Use a predictable structure:
- `docs/ai/requirements/`
- `docs/ai/design/`
- `docs/ai/security/`
- `docs/ai/testing/`
- `docs/ai/reviews/`

This makes review easier and keeps AI work product visible and auditable.

## Mark templates vs “real” artifacts
If a file is intended to be copied and customized, mark it clearly:
- Put `TEMPLATE` in the title
- Use placeholders like `<OWNER>` and add “replace before use”
- Avoid making factual claims unless you can cite repo evidence

---

# How to use this day-to-day (repeatable workflows)

## Workflow 1 — AI feature intake (low friction)
1. Create an Issue using **AI_Task_Request** template.
2. Fill in scope, constraints, prohibited content.
3. Use `REQ_01_feature_intake.md` with the Requirements agent.
4. Commit output to `docs/ai/requirements/<feature>.md`.
5. Open MR using **AI_Assisted_Change** template.

## Workflow 2 — Threat Model Lite (security-friendly)
1. Use `SEC_01_threat_model_lite.md` with **Security_Analyst_Agent**.
2. Output to `docs/ai/security/threat-model-lite-<feature>.md`.
3. Ensure every uncertain assumption is prefixed **UNVERIFIED:**.
4. Route through CODEOWNERS + security approval.

## Workflow 3 — Abuse suite for AI features (prompt injection / leakage)
1. Use `SEC_02_ai_feature_abuse_suite.md` with **Red_Team_Prompting_Agent**.
2. Output to `docs/ai/security/abuse-suite-<feature>.md`.
3. Use it to drive test cases and guardrails.

## Workflow 4 — AI-assisted code change with bounded outputs
1. Use `CODE_01_generate_change.md` to produce:
   - small patch plan
   - tests
   - rollback notes
2. Implement changes (human applies).
3. Use `REVIEW_01_code_risk_review.md` for a risk-based review narrative.

---

# Demonstrating value (what to show leadership)

This pack is useful when it makes AI integration:
- **repeatable** (same prompts, same output structure),
- **governed** (approvals + CODEOWNERS),
- **truthful** (UNVERIFIED labeling),
- and **safe by default** (no secrets/internal identifiers).

## A strong “10 minute demo” sequence (GitLab)
1. Show `AGENTS.md`, `.gitlab/duo/*`, and CODEOWNERS.
2. Create an MR using the AI-assisted MR template.
3. Ask Duo to review a doc that contains:
   - a placeholder owner, and
   - an assumption written as fact without **UNVERIFIED**.
4. Show Duo flags it.
5. (Optional) Add a fake email string and show the CI safety scan fails.
6. Fix by redacting to `<EMAIL_REDACTED>`; rerun; pipeline goes green.

---

# Troubleshooting

## “Duo is hallucinating repo details / leaking identifiers”
- Ensure `platform_overlays/gitlab/.gitlab/duo/chat-rules.md` contains the **Redaction and identifiers (CRITICAL)** section.
- Keep “repo-only” as the default stance.
- Add the optional CI safety scan to catch leaks deterministically.
- Treat internal identifiers as sensitive by default (hostnames, instance URLs, IDs, emails).

## “Templates don’t appear in GitLab”
- Issue templates must be under: `.gitlab/issue_templates/`
- MR templates must be under: `.gitlab/merge_request_templates/`
- They must exist on the default branch to be selectable.

## “CODEOWNERS isn’t enforcing”
- Confirm `CODEOWNERS` is at repo root.
- Confirm “Require approval from code owners” is enabled (if available).
- Confirm owners in CODEOWNERS are real groups/users.

---

# Extending the pack (how to customize safely)

## Add a new agent
1. Create `.ai4sdlc/AGENTS/<New_Agent>.md`
2. Include:
   - purpose
   - required inputs
   - output format
   - stop/escalation triggers
   - explicit reference to `.ai4sdlc/POLICY/guardrails.md`
3. Protect it with CODEOWNERS if it’s governance-relevant.

## Add a new prompt
1. Create `.ai4sdlc/PROMPTS/<PHASE>_<NN>_<name>.md`
2. Keep it bounded: explicit inputs, explicit outputs, explicit verification.

## Change governance rules
Do it like code:
- MR only
- required CODEOWNERS approval
- document “why” in the MR

---

# Versioning

See `CHANGELOG.md` for what changed per version.  
This v0.3 hardened build specifically strengthens redaction rules and provides the optional CI safety scan to prevent the most common real-world failure mode: **accidental disclosure**.

---

## License / reuse
This pack is intended to be copied into many repositories and adapted locally. If you distribute it broadly, keep it text-first and avoid embedding environment-specific identifiers.
