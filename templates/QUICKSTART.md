# Quickstart (10–15 minutes)

This quickstart proves the starter pack works without requiring complex infrastructure.

## 1) Copy the pack into a test repo
1. Copy `.ai4sdlc/` to repo root.
2. If using GitLab, also copy `platform_overlays/gitlab/*` into repo root (overlay includes `.gitlab/**`, `AGENTS.md`, `CODEOWNERS`).

Commit to a branch.

## 2) Customize CODEOWNERS (GitLab recommended)
Open `CODEOWNERS` and replace any placeholder owners with real GitLab users/groups.

## 3) Create an MR using the included template
In GitLab, create a merge request and select:
- `.gitlab/merge_request_templates/AI_Assisted_Change.md`

Fill it out (especially “AI usage” and “verification performed”).

## 4) Run a “truthfulness” test
Create a doc under `docs/ai/security/` using a template prompt:
- Use `.ai4sdlc/PROMPTS/SEC_01_threat_model_lite.md`

Intentionally include:
- a placeholder like `<OWNER>`
- an assumption stated as a fact, without **UNVERIFIED:**

If GitLab Duo is enabled and configured to use the repo instructions, request a Duo review. It should flag:
- placeholder ownership
- missing **UNVERIFIED:** labeling for unverifiable assumptions

## 5) Optional: enable deterministic redaction scanning
If you want CI to hard-stop common leakage patterns:
- Add to `.gitlab-ci.yml`:

```yaml
include:
  - local: ".gitlab/ci/ai4sdlc-safety.yml"
```

Then add a fake email like `someone@example.com` to a file under `docs/ai/**` and confirm CI fails. Redact to `<EMAIL_REDACTED>` and confirm CI passes.

## 6) Done
You now have:
- a portable PromptOps baseline (`.ai4sdlc/`)
- GitLab-native templates + (optional) Duo steering
- and optional deterministic guardrails (CI scan)
