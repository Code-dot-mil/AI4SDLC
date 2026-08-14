## v0.3.4 (GitLab Hardened Full Drop-In)

- Hardened `.gitlab/duo/chat-rules.md` with explicit refusal pattern for identity/instance metadata requests.
- Updated prompt templates to reduce identifier leakage risk and enforce evidence-based outputs:
  - `SEC_02_ai_feature_abuse_suite.md` (adds evidence/redaction rules, bounded test-case framing)
  - `REVIEW_01_code_risk_review.md` (adds evidence/redaction rules and structured findings)

# Changelog

## v0.1
- Initial PromptOps starter pack: agents, prompts, guardrails, autonomy levels, forms, review templates.

## v0.2
- Added GitLab overlay: AGENTS.md, .gitlab/duo chat rules, MR review instructions YAML, CODEOWNERS, GitLab issue/MR templates.

## v0.3
- Hardened Duo rules: explicit redaction/identifier restrictions for chat + MR review output.
- Added optional GitLab CI include: `.gitlab/ci/ai4sdlc-safety.yml` to fail on obvious sensitive identifier patterns.
- Date: 2026-01-28

## v0.3.2
- Added Apache 2.0 LICENSE + NOTICE for frictionless reuse.
- Added DATA_HANDLING.md, QUICKSTART.md, SECURITY.md, SUPPORT.md, RELEASE_CHECKLIST.md.
- Regenerated SHA256SUMS.
- Date: 2026-01-28
