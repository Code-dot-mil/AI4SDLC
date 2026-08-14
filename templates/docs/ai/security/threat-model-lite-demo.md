# Threat Model Lite — AI4SDLC PromptOps Demo

**Artifact type:** Threat Model Lite  
**Scope:** Demo-only (platform-agnostic)  
**Data sensitivity:** Public / Demo (no secrets, no internal identifiers)  
**Owner (human):** <OWNER_TEAM_OR_ROLE>  
**Last updated:** <YYYY-MM-DD>

## Context (human-provided)
- Feature/system: Demo web app with Web UI + REST API + Postgres
- Data processed: Demo user profile data (non-sensitive)
- Entry points: Web UI, REST API
- Trust boundaries: Browser↔UI, UI↔API, API↔DB
- Constraints: Platform-agnostic demo, restricted/unknown environment, no secrets

## AI task
Use `.ai4sdlc/PROMPTS/SEC_01_threat_model_lite.md` and follow `.ai4sdlc/POLICY/guardrails.md`.
Generate the full Threat Model Lite below this section.
If any fact is not supported by the context above or repo evidence, label it **UNVERIFIED:** and say what is missing.

---
<!-- AI GENERATED CONTENT BELOW -->
