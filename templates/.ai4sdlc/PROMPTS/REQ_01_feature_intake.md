# REQ_01 Feature Intake

## Purpose
Turn a feature idea into scope, acceptance criteria, risks, and verification steps.

## Instructions
- Use an appropriate agent contract from `.ai4sdlc/AGENTS/` (recommended).
- Follow `.ai4sdlc/POLICY/guardrails.md`.
- If a field is unknown, write **UNVERIFIED** and explain what is missing.

## Prompt template
**Context**
- Feature request: [paste]
- Users/stakeholders: [list]
- Constraints: [security/compliance/performance/platform]
- Non-goals: [list]

**Task**
Draft a feature intake document with:
- Problem statement
- Scope (In/Out)
- User stories
- Acceptance criteria (testable)
- Risks & edge cases (security included)
- Verification plan
- Assumptions (UNVERIFIED where applicable)


## Output contract
Return Markdown with the sections listed above.
