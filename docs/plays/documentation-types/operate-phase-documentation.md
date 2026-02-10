---
title: "Operate Phase Documentation"
parent: "documentation-play.md"
lifecycle: beta
last_updated: "2026-01-26"
---

# Operate Phase Documentation

[← Back to Documentation Play](../documentation-play.md)

---

## What Happens in This Phase

The Operate phase maintains software in production through daily operations, scaling, and routine maintenance. Documentation covers operational procedures, scaling policies, backup strategies, and change logs. AI generates operational documentation from infrastructure code and operational scripts, with humans validating strategies align with mission requirements.

---

## Where AI Helps

- **Generating operational procedures** from automation scripts and playbooks
- **Documenting scaling policies** from autoscaling configurations
- **Creating backup and recovery documentation** from backup scripts and schedules
- **Maintaining change logs** from configuration management systems
- **Producing performance baseline documentation** from historical metrics

---

## Example: Operations Guide

This example demonstrates **GenAI Completion**—AI generates operational documentation directly from infrastructure configurations, producing deterministic output that requires validation against mission requirements.

### Context Setup

Before prompting, the ops team provided:

- Kubernetes HorizontalPodAutoscaler configurations
- Backup CronJob definitions with retention policies
- Ops playbooks in /runbooks directory
- Historical performance metrics for baseline calculation

### Prompt

> *"Generate operational documentation for the intelligence aggregation service based on the Kubernetes HPA configs, the backup CronJobs, and the ops playbooks in /runbooks. Include scaling triggers, backup schedules, routine maintenance tasks, and performance baselines."*

### Output

```markdown
# Intelligence Aggregation Service Operations Guide

## Scaling Policy
| Metric | Scale Up | Scale Down | Min | Max |
|--------|----------|------------|-----|-----|
| CPU | > 70% for 3m | < 30% for 10m | 3 | 12 |
| Memory | > 80% for 5m | < 40% for 15m | 3 | 12 |
| Queue depth | > 1000 msgs | < 100 msgs | 3 | 12 |

## Backup Schedule
- **Database:** Daily at 0200 UTC, retained 30 days
- **Configuration:** On every change, retained 90 days
- **Audit logs:** Weekly archive to cold storage

## Routine Maintenance
- **Weekly:** Review and rotate logs, verify backup integrity
- **Monthly:** Security patch assessment, capacity review
- **Quarterly:** Disaster recovery drill, performance baseline update

## Performance Baselines
- Request latency p50: 45ms, p99: 180ms
- Throughput: 2,500 requests/second sustained
- Error rate: < 0.01% under normal load
```

---

## What AI-Generated Operate Documentation Often Misses

- **Mission tempo alignment** - Scaling and maintenance timing relative to operations
- **Degraded mode operations** - How to operate with reduced capability
- **Cross-system dependencies** - Impact of upstream/downstream system changes
- **Capacity planning context** - Why current baselines exist and growth projections
- **Incident patterns** - Recurring issues and their root causes

**Human reviewers must validate operational procedures against mission requirements.**

---

## Governance Checklist

Before accepting AI-assisted Operate phase documentation:

- [ ] Scaling policies tested under load
- [ ] Backup recovery procedures verified
- [ ] Maintenance windows aligned with operations
- [ ] Performance baselines current and accurate
- [ ] Change log reflects all configuration changes

---

## Brownfield Additions

For modernization efforts, Operate phase requires additional focus:

- **Transition validation** - Document evidence that modern system operates as well as legacy
- **Legacy retirement planning** - Procedures and timeline for decommissioning legacy operations
- **Operational parity documentation** - Side-by-side comparison of legacy vs. modern operational characteristics

---

[← Back to Documentation Play](../documentation-play.md)
