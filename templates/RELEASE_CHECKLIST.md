# Release checklist (for distributors)

Use this before sending the pack broadly.

## Content safety
- [ ] No internal hostnames, instance URLs, clone URLs
- [ ] No usernames/emails/IDs
- [ ] No tokens/keys/certs/log dumps
- [ ] Templates clearly marked TEMPLATE where applicable
- [ ] Guardrails explicitly require **UNVERIFIED:** labeling for unverifiable claims

## Usability
- [ ] README.md is present at repo root and explains the pack end-to-end
- [ ] QUICKSTART.md works in a blank repo
- [ ] GitLab templates appear under `.gitlab/*_templates/`
- [ ] CODEOWNERS includes obvious placeholders and instructions to replace

## GitLab overlay (if applicable)
- [ ] `.gitlab/duo/chat-rules.md` contains explicit redaction rules
- [ ] `.gitlab/duo/mr-review-instructions.yaml` aligns to baseline guardrails
- [ ] Optional CI scan include exists at `.gitlab/ci/ai4sdlc-safety.yml`

## Licensing
- [ ] LICENSE exists (Apache-2.0)
- [ ] NOTICE exists (if desired)

## Integrity
- [ ] SHA256SUMS regenerated after any change
