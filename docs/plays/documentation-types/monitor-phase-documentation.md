---
title: "Monitor Phase Documentation"
parent: "documentation-play.md"
lifecycle: beta
last_updated: "2026-02-08"
---

# Monitor Phase Documentation

[← Back to Documentation Play](../documentation-play.md)

---

## What Happens in This Phase

The Monitor phase provides continuous visibility into system health, performance, and security. Unlike other phases where documentation consists of static artifacts, **monitoring "documentation" is fundamentally different**—it includes live dashboards, real-time metrics, and automated alerts that function as observable infrastructure.

This distinction matters: the dashboards and alert configurations themselves ARE documentation. They're version-controlled code (Grafana JSON, Prometheus YAML, Terraform modules) that should be automatically deployed and continuously validated against actual system behavior.

---

## Two Types of Monitor Phase Artifacts

### Dynamic Monitoring Artifacts (Observable Infrastructure)

These are the configurations that define what gets monitored and how. They're code, not prose.

| Artifact Type | Examples | Format |
|---------------|----------|--------|
| Dashboard definitions | Service health, security events, SLO tracking | Grafana JSON, Datadog YAML |
| Alert rules | Threshold alerts, anomaly detection | Prometheus YAML, PagerDuty configs |
| SLO/SLI definitions | Availability targets, latency budgets | OpenSLO YAML, service configs |
| Log aggregation configs | What gets collected, retention policies | Fluentd configs, CloudWatch rules |

**AI's role**: Generate and maintain these configurations directly—dashboard-as-code, alerts-from-SLOs, observability-as-code.

### Supporting Documentation (Traditional Docs)

These explain the "why" behind monitoring decisions and guide human response.

| Artifact Type | Purpose |
|---------------|---------|
| Monitoring strategy | Why these metrics matter, coverage philosophy |
| Alert response procedures | What to do when alerts fire |
| Threshold rationale | Why specific values were chosen |
| Runbook procedures | Step-by-step incident response |
| Compliance mapping | Which metrics satisfy which requirements |

**AI's role**: Generate and maintain these docs, keeping them synchronized with the dynamic artifacts.

---

## Where AI Helps

### For Dynamic Artifacts (Monitoring-as-Code)

- **Generating dashboard configurations** from SLO definitions and service metadata
- **Creating alert rules** from SLOs and operational requirements
- **Maintaining consistency** across dashboard definitions as services evolve
- **Validating configurations** against actual system behavior

### For Supporting Documentation

- **Generating runbook procedures** when alert thresholds are defined
- **Documenting threshold rationale** from operational history and SLO targets
- **Creating compliance mappings** from metric definitions to requirements
- **Keeping docs synchronized** when monitoring configs change

---

## Example: Prometheus Alert Rules (Dynamic Artifact)

This example demonstrates **GenAI Completion**—AI generates alert configurations directly from SLO definitions, producing deterministic monitoring-as-code that requires validation against operational experience.

### Context Setup

Before prompting, the SRE provided:

- Service SLO definitions (99.9% availability, p99 < 500ms, error rate < 0.1%)
- Prometheus metric naming conventions for the service
- Runbook URL patterns for alert annotations
- Severity level standards from on-call policy

### Prompt

> *"Generate Prometheus alerting rules for the logistics API based on these SLOs: 99.9% availability, p99 latency under 500ms, error rate below 0.1%. Include appropriate severity levels and annotations for runbook links."*

### Output

```yaml
# logistics-api-alerts.yaml
groups:
  - name: logistics-api-slos
    rules:
      - alert: LogisticsAPIAvailabilityBreach
        expr: |
          (1 - (sum(rate(http_requests_total{service="logistics-api",code=~"5.."}[5m]))
          / sum(rate(http_requests_total{service="logistics-api"}[5m])))) < 0.999
        for: 5m
        labels:
          severity: critical
          service: logistics-api
        annotations:
          summary: "Logistics API availability below 99.9% SLO"
          runbook_url: "https://runbooks.example.com/logistics-api/availability"

      - alert: LogisticsAPILatencyBreach
        expr: |
          histogram_quantile(0.99, rate(http_request_duration_seconds_bucket{service="logistics-api"}[5m])) > 0.5
        for: 5m
        labels:
          severity: warning
          service: logistics-api
        annotations:
          summary: "Logistics API p99 latency exceeds 500ms SLO"
          runbook_url: "https://runbooks.example.com/logistics-api/latency"
```

---

## Example: Alert Runbook (Supporting Documentation)

This example demonstrates **GenAI Generation**—AI synthesizes alert context and service architecture into operational procedures that require SRE validation but follow predictable structure.

### Context Setup

Before prompting, the SRE provided:

- Alert definition with threshold and severity
- Service architecture diagram showing dependencies
- Historical incident data for common failure patterns
- Escalation policy from on-call rotation
- Existing remediation scripts and kubectl commands

### Prompt

> *"Generate a runbook for the LogisticsAPIAvailabilityBreach alert. Include triage steps, common causes, escalation paths, and recovery procedures based on our service architecture."*

### Output

```markdown
# Runbook: LogisticsAPIAvailabilityBreach

## Alert Context
- **Severity**: Critical
- **SLO**: 99.9% availability
- **Threshold**: Fires when availability drops below 99.9% for 5 minutes

## Immediate Triage (First 5 minutes)
1. Check service health dashboard: [link]
2. Verify alert is not a false positive (check upstream dependencies)
3. Identify error pattern in logs: `kubectl logs -l app=logistics-api --tail=100`

## Common Causes
| Symptom | Likely Cause | Quick Fix |
|---------|--------------|-----------|
| 503 errors spike | Pod crashes | Check pod restarts, scale up |
| Connection timeouts | Database overload | Check DB connections, consider read replica |
| Intermittent 500s | Dependency failure | Check downstream service health |

## Escalation
- **15 min unresolved**: Page on-call SRE
- **30 min unresolved**: Engage service owner
- **Production impact confirmed**: Incident commander protocol
```

---

## Observable Infrastructure Principles

Monitor phase artifacts should be treated as infrastructure:

- **Version controlled**: Dashboard JSON and alert YAML live in git alongside application code
- **Automatically deployed**: CI/CD pipelines deploy monitoring configs, not manual UI changes
- **Continuously validated**: Tests verify alerts fire correctly, dashboards load, metrics exist
- **Self-documenting**: Well-structured configs with clear naming reduce need for separate docs

This approach means monitoring documentation stays current because it IS the monitoring system—not a separate description that drifts out of sync.

---

## What AI-Generated Monitor Artifacts Often Miss

### For Dynamic Artifacts

- **Operational context** - Alert thresholds that work in theory but cause alert fatigue in practice
- **Cross-service dependencies** - Metrics that matter for end-to-end flows, not individual services
- **Environment differences** - Thresholds that vary between dev, staging, and production

### For Supporting Docs

- **Tribal knowledge** - Why certain alerts exist based on past incidents
- **Team-specific escalation** - Who actually responds vs. documented paths
- **Compliance nuances** - Which metrics satisfy specific audit requirements

**Human reviewers must validate that monitoring artifacts reflect operational reality, not theoretical ideals.**

---

## Governance Checklist

Before accepting AI-assisted Monitor phase artifacts:

### Dynamic Artifacts

- [ ] Alert thresholds validated against operational experience (not just SLO math)
- [ ] Dashboard configs tested in staging before production deployment
- [ ] Monitoring-as-code integrated into CI/CD pipeline
- [ ] Rollback procedures exist for monitoring config changes

### Supporting Documentation

- [ ] Runbooks tested during incident simulations
- [ ] Escalation paths current with team structure
- [ ] Compliance metrics mapped to specific ATO requirements
- [ ] Threshold rationale documented for future maintainers

---

## Brownfield Additions

For modernization efforts, Monitor phase requires additional focus:

- **Legacy vs. modern performance comparison** - Side-by-side dashboards demonstrating modernization success
- **Retirement validation metrics** - Observable evidence that legacy system can be safely decommissioned
- **Unified monitoring** - Single observability layer across legacy and modern systems during transition
- **Migration of monitoring-as-code** - Converting legacy monitoring (manual UI configs) to version-controlled definitions

---

[← Back to Documentation Play](../documentation-play.md)
