# Support and maintenance

This pack is intended to be **copied and customized** per organization.

## How to adopt safely
- Start with the baseline `.ai4sdlc/` guardrails and 1–2 agents.
- Add GitLab overlay files if you use GitLab.
- Protect governance files with CODEOWNERS + approvals.
- Add deterministic checks only after you see recurring failure modes.

## Updating
Treat updates like code:
- Change via Merge Request
- Require CODEOWNERS approval for governance files
- Document why changes are needed and what risk they address
