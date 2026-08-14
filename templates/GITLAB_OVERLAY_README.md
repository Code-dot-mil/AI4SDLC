# GitLab Overlay (Duo + Templates) — AI4SDLC-aligned

This overlay makes the PromptOps Starter Pack feel **native** in GitLab by adding:
- `AGENTS.md` (project-wide agent instructions)
- `.gitlab/duo/chat-rules.md` (Duo Chat behavior rules)
- `.gitlab/duo/mr-review-instructions.yaml` (Duo Code Review Flow standards)
- `CODEOWNERS` protections for AI governance files
- Issue/MR templates for repeatable AI-assisted SDLC workflows

## How to apply
Copy the contents of this folder into the **root** of your GitLab repository:

- `AGENTS.md` → repo root
- `.gitlab/**` → repo root `.gitlab/`
- `CODEOWNERS` → repo root

Then commit and push to your default branch.

## Tune for your organization
1. Edit `CODEOWNERS` to match your actual GitLab groups/users.
2. Edit `AGENTS.md` boundaries (repo-only vs approved external sources).
3. Optionally narrow `.gitlab/duo/mr-review-instructions.yaml` `fileFilters` to your repo layout.

## Notes
- Keep instructions minimal and actionable; add complexity only when you see recurring failures.
- These files are designed to be safe defaults for broad DoD use.
- Date: 2026-01-28

## Optional: Add a lightweight CI “redaction” safety scan
If you want CI to hard-stop obvious leaks (emails, private IPs, internal TLD hostnames) in AI artifacts:

1) Copy `.gitlab/ci/ai4sdlc-safety.yml` into your repo (already included in this overlay).
2) In your `.gitlab-ci.yml`, add:

```yaml
include:
  - local: ".gitlab/ci/ai4sdlc-safety.yml"
```

This is deliberately simple and may create false positives in some environments. Tune the regex and paths as needed.
