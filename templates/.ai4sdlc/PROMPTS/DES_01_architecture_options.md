# DES_01 Architecture Options

## Purpose
Generate architecture options and tradeoffs for a change, with constraints and verification hooks.

## Instructions
- Use an appropriate agent contract from `.ai4sdlc/AGENTS/` (recommended).
- Follow `.ai4sdlc/POLICY/guardrails.md`.
- If a field is unknown, write **UNVERIFIED** and explain what is missing.

## Prompt template
**Context**
- System/component: [describe]
- Key constraints: [latency, reliability, compliance, restricted egress, etc.]
- Current architecture (if known): [describe or UNVERIFIED]
- Non-goals: [list]

**Task**
Provide 2–3 architecture options. For each option include:
- Summary
- Tradeoffs
- Security considerations
- Operational considerations
- Verification steps (how to validate it works)
- Risks & mitigations


## Output contract
Return Markdown with an 'Option 1/2/3' structure and required bullets.
