# DOC_01 Update Docs

## Purpose
Create or update documentation with clear assumptions and verification steps.

## Instructions
- Use an appropriate agent contract from `.ai4sdlc/AGENTS/` (recommended).
- Follow `.ai4sdlc/POLICY/guardrails.md`.
- If a field is unknown, write **UNVERIFIED** and explain what is missing.

## Prompt template
**Context**
- What changed: [paste]
- Audience: [engineers/operators]
- Constraints: [security/compliance]

**Task**
Write documentation that includes:
- Summary
- How to use
- Configuration/inputs
- Troubleshooting
- Verification steps
- Assumptions (UNVERIFIED)


## Output contract
Return Markdown with the required sections.
