---
title: "Deploy Phase Documentation"
parent: "documentation-play.md"
lifecycle: beta
last_updated: "2026-01-26"
---

# Deploy Phase Documentation

[← Back to Documentation Play](../documentation-play.md)

---

## What Happens in This Phase

The Deploy phase activates software in production and makes it available to users. Documentation includes operational runbooks, incident response procedures, and end-user guidance. AI generates runbooks from operations code while humans ensure user documentation matches actual user mental models and operational needs.

---

## Where AI Helps

- **Generating operational runbooks** from monitoring configs and scripts
- **Creating incident response procedures** from alert definitions
- **Documenting configuration management** from deployed state
- **Producing monitoring dashboard documentation** from observability setup
- **Drafting end-user documentation** from feature specifications

---

## Example: Operational Runbook

This example demonstrates **GenAI Generation**—AI synthesizes alert definitions and remediation scripts into operational procedures that require SRE validation but follow predictable structure.

### Context Setup

Before prompting, the SRE provided:

- Prometheus alert definitions (/monitoring/alerts.yml)
- Remediation scripts with inline documentation (/ops/scripts/)
- Escalation policy from on-call rotation
- Service architecture diagram for dependency context

### Prompt

> *"Generate an operational runbook for the mission planning service based on the Prometheus alerts in /monitoring/alerts.yml and the remediation scripts in /ops/scripts/. Include alert descriptions, severity levels, investigation steps, and remediation procedures."*

### Output

```markdown
# Mission Planning Service Runbook

## Alert: HighLatencyP99
**Severity:** Warning
**Threshold:** p99 latency > 2000ms for 5 minutes

### Investigation
1. Check current request rate: `curl localhost:9090/metrics | grep request_count`
2. Review recent deployments: `kubectl rollout history deployment/mission-planner`
3. Check database connection pool: `psql -c "SELECT count(*) FROM pg_stat_activity"`

### Remediation
- If traffic spike: Scale replicas `kubectl scale deployment/mission-planner --replicas=8`
- If DB bottleneck: Clear connection pool and investigate slow queries
- If recent deploy: Consider rollback per deployment runbook

### Escalation
- If unresolved after 15 minutes, page on-call SRE
- If mission-impacting, notify operations center

---

## Alert: DatabaseConnectionExhausted
**Severity:** Critical
...
```

---

## What AI-Generated Deploy Documentation Often Misses

- **Operational context** - Why certain alerts matter more during specific missions
- **Tribal knowledge** - Workarounds and gotchas known only to experienced operators
- **User workflow integration** - How the system fits into broader operational processes
- **Communication procedures** - Who to notify and how during incidents
- **Classification-appropriate language** - User docs for different clearance levels

**Human reviewers must validate runbooks reflect actual operational experience.**

---

## Governance Checklist

Before accepting AI-assisted Deploy phase documentation:

- [ ] Runbook procedures tested against actual scenarios
- [ ] Incident response contacts current and verified
- [ ] User documentation reviewed for accuracy and clarity
- [ ] Monitoring dashboards match documented metrics
- [ ] Configuration management records complete

---

## Brownfield Additions

For modernization efforts, Deploy phase requires additional focus:

- **User transition guides** - Document differences between legacy and modern interfaces
- **System cutover procedures** - Step-by-step activation of modern system with user communication
- **Parallel operations documentation** - Procedures for operating both systems during transition

---

[← Back to Documentation Play](../documentation-play.md)
