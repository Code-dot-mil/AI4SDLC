# CODE_01 Generate Change (Proposal)

## Purpose
Generate a proposed code change plan (not blind copy/paste) with tests and verification.

## Instructions
- Use an appropriate agent contract from `.ai4sdlc/AGENTS/` (recommended).
- Follow `.ai4sdlc/POLICY/guardrails.md`.
- If a field is unknown, write **UNVERIFIED** and explain what is missing.

## Prompt template
**Context**
- Goal: [what should change]
- Repo paths: [where]
- Constraints: [language, frameworks, toolchain]
- Non-goals: [list]

**Task**
Propose a change plan:
- Files to change (with reasons)
- Key code patterns to follow
- Security considerations
- Tests to add/update
- Verification steps

If you cannot see the repo, mark file paths as UNVERIFIED and list questions.


## Output contract
Return Markdown with a 'Plan' + 'Tests' + 'Verification' structure.
