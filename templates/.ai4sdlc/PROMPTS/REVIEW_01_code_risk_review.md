# REVIEW_01 Code Risk Review

## Purpose
Review a change for security/reliability risks and propose concrete fixes.

## Instructions
- Use an appropriate agent contract from `.ai4sdlc/AGENTS/` (recommended).
- Follow `.ai4sdlc/POLICY/guardrails.md`.
- If a field is unknown, write **UNVERIFIED** and explain what is missing.

### Evidence & redaction rules (CRITICAL)
- **Evidence rule:** Only claim facts supported by the MR diff/snippets and repo context provided. If something cannot be verified, label it **UNVERIFIED:** and state what evidence is missing.
- **No inference rule:** Do not invent or restate internal identifiers (hostnames, URLs, emails, usernames, IDs, project paths). Use placeholders like `<INTERNAL_HOSTNAME>`, `<EMAIL_REDACTED>`, `<PROJECT_ID_REDACTED>`.
- **No secrets rule:** Do not request or output secrets/credentials. If examples are needed, ask for redacted samples.

## Prompt template
**Context**
- Change summary: [paste]
- Diff/snippets: [paste]
- Constraints: [runtime, compliance, restricted egress]
- Known concerns: [list]
- Threat model notes (optional): [assets, trust boundaries, sensitive data]

**Task**
Provide:

1. **Risk summary**
   - Top 3 risks (what + why + impact)
   - Overall risk rating: Low | Medium | High | Critical

2. **Findings** (repeat for each finding)
   For each finding include:
   - Title
   - Category: AuthZ/AuthN | Input validation | Injection | Data exposure | Secrets | Crypto | Logging | Supply chain | Availability | Integrity | Privilege/RBAC | Concurrency | Error handling | Config/IaC | Other
   - Severity: Low | Medium | High | Critical
   - Evidence: exact snippet/line reference from provided diff/snippets
   - Risk: what could go wrong (realistic failure/abuse path)
   - Recommendation: concrete fix (code-level or config-level)
   - Verification: test(s) to add/run to prove the fix

3. **Tests to add/run**
   - Unit tests
   - Integration tests
   - Security checks (as applicable)

4. **Security-sensitive change triggers**
   Call out explicitly if the change touches any of:
   - Authentication/authorization, access control, roles/permissions
   - Secrets handling, crypto, token/session management
   - Network egress, SSRF, file uploads, deserialization
   - CI/CD, pipeline permissions, IaC/RBAC
   - Logging/telemetry that may include sensitive data

5. **Assumptions**
   - List assumptions, each prefixed with **UNVERIFIED:** unless proven by the supplied context.

## Output contract
Return Markdown following the Code Review / Risk Reviewer Agent deliverable template.
