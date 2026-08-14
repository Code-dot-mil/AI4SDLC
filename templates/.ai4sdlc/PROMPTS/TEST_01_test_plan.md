# TEST_01 Test Plan

## Purpose
Generate a pragmatic test plan from acceptance criteria and risks.

## Instructions
- Use an appropriate agent contract from `.ai4sdlc/AGENTS/` (recommended).
- Follow `.ai4sdlc/POLICY/guardrails.md`.
- If a field is unknown, write **UNVERIFIED** and explain what is missing.

## Prompt template
**Context**
- Feature/change summary: [paste]
- Acceptance criteria: [paste]
- Risk areas: [list]

**Task**
Create a test plan:
- Strategy (unit/integration/e2e)
- Test cases with expected outcomes
- Negative/edge cases
- Automation recommendations
- Verification steps


## Output contract
Return Markdown following the Test Planner Agent deliverable template.
