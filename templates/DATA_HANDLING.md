# Data handling rules (read before using any AI tool)

This starter pack is designed to be **safe by default**. Your fastest path to a bad day is pasting sensitive data into an AI prompt, MR comment, issue, or artifact.

## Never include (paste, attach, or quote)
- Credentials of any kind (passwords, API keys, tokens, private keys, session cookies)
- Certificates, SSH keys, Kubeconfigs, cloud access keys, vault output
- Raw logs that may contain tokens, headers, or internal identifiers
- Internal hostnames, instance URLs, clone URLs, internal domains/TLDs
- Usernames, email addresses, user IDs, project IDs, namespace IDs
- CUI/PII/PHI, export-controlled, classified, or mission-sensitive details
- Network diagrams, IP addressing, IAM policy details, RBAC bindings, firewall rules (unless explicitly approved)

## Always redact to placeholders
Use placeholders consistently:
- `<TOKEN_REDACTED>`, `<PASSWORD_REDACTED>`, `<PRIVATE_KEY_REDACTED>`
- `<EMAIL_REDACTED>`, `<USER_REDACTED>`, `<PROJECT_ID_REDACTED>`
- `<INTERNAL_HOSTNAME>`, `<INTERNAL_URL>`, `<INTERNAL_DOMAIN>`
- `<IP_REDACTED>`, `<SUBNET_REDACTED>`

## Safe defaults
- Prefer **repo-only context**: only what is already in the repo and approved for sharing.
- Treat AI output as **draft**: require human review for correctness, security impact, and policy alignment.
- If you are uncertain about data classification: **stop** and ask your security/compliance owner.

## If you accidentally leaked something
1. Stop. Do not add more details trying to “explain”.
2. Remove/redact the content from the prompt thread, MR, issue, or file if possible.
3. Rotate any potentially exposed secrets/tokens.
4. Notify your security owner and follow your org’s incident handling process.
