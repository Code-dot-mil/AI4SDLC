# SEC_01 Threat Model Lite

## Purpose
Create a threat-model-lite suitable for early design and review.

## Instructions
- Use an appropriate agent contract from `.ai4sdlc/AGENTS/` (recommended).
- Follow `.ai4sdlc/POLICY/guardrails.md`.
- If a field is unknown, write **UNVERIFIED** and explain what is missing.

## Prompt template
**Context**
- Feature/system: [describe]
- Data processed: [types]
- Entry points: [APIs, UI, integrations]
- Trust boundaries: [zones/identities]
- Constraints: [deployment, compliance]

**Task**
Produce a threat model lite:
- Assets & objectives
- Trust boundaries & data flows
- Top threats/misuse cases (likelihood/impact)
- Mitigations & controls
- Verification steps
- Assumptions (UNVERIFIED)


## Output contract
Return Markdown with the above sections.
