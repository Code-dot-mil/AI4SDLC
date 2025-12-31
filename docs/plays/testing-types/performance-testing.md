---
title: "Performance and Load Testing with GenAI"
parent: "testing-play.md"
lifecycle: "beta"
last_updated: "2025-12-30"
---

# Performance and Load Testing with GenAI

[← Back to Testing Play](../testing-play.md)

---

## What is Performance and Load Testing?

Performance testing validates system responsiveness, throughput, and scalability under realistic and extreme load conditions. It ensures systems meet performance requirements and identifies bottlenecks before they impact operations. GenAI can generate diverse load scenarios and detect performance regressions, but operational realism and infrastructure context require domain expertise.

---

## When Performance Testing Occurs

Performance testing happens at key milestones and continuously in mature environments:

**Common triggers:**


- **After major features** - Validates new functionality doesn't degrade performance
- **Before production deployment** - Load testing at expected operational scale
- **Infrastructure changes** - Validates performance after scaling, configuration, or platform changes
- **Performance regression detection** - Continuous monitoring of response times across builds
- **Capacity planning** - Stress testing to determine system limits for scaling decisions
- **After performance incidents** - Reproducing and validating fixes for performance issues

GenAI helps generate load scenarios and analyze trends, but understanding operational load patterns requires domain expertise.

---

## Where GenAI Helps

- **Load scenario generation** - Creating diverse user behavior profiles and traffic patterns
- **Test data scaling** - Generating large realistic datasets for stress testing
- **Performance regression detection** - Identifying trends and degradation across test runs
- **Baseline analysis** - Comparing performance results across builds and environments

---

## Example Requirement

**REQ-PERF-401:** *"The mission planning API shall support 500 concurrent users with 95th percentile response time under 2 seconds and zero errors at steady state."*

---

## Example Prompt

> *"Generate load test scenarios for requirement REQ-PERF-401 using Locust. Create scenarios for: gradual ramp-up to 500 users over 5 minutes, sustained load at 500 users for 30 minutes, spike to 750 users for 2 minutes, then ramp-down. Include realistic user behavior with mixed read/write operations."*

---

## Example Output

```python
from locust import HttpUser, task, between

class MissionPlannerUser(HttpUser):
    wait_time = between(1, 3)  # Realistic user think time

    @task(6)  # 60% of requests - read-heavy operations
    def get_mission_plan(self):
        mission_id = self.get_random_mission_id()
        self.client.get(
            f"/api/missions/{mission_id}",
            name="Get Mission Plan"
        )

    @task(2)  # 20% of requests - updates
    def update_mission_waypoint(self):
        mission_id = self.get_random_mission_id()
        self.client.put(
            f"/api/missions/{mission_id}/waypoints/wp-001",
            json={
                "lat": 38.8977 + random.uniform(-0.1, 0.1),
                "lon": -77.0365 + random.uniform(-0.1, 0.1)
            },
            name="Update Waypoint"
        )

    @task(2)  # 20% of requests - searches
    def search_missions(self):
        self.client.get(
            "/api/missions?status=active&region=CENTCOM",
            name="Search Missions"
        )

    def get_random_mission_id(self):
        return random.choice(self.mission_ids)

    def on_start(self):
        # Load test data: realistic mission IDs
        self.mission_ids = [f"MISSION-{i:04d}" for i in range(1, 1001)]

# Load profile (configured in Locust UI or command line):
# - Gradual ramp-up: 0 to 500 users over 5 minutes
# - Sustained load: 500 users for 30 minutes
# - Spike test: Ramp to 750 users over 1 minute, hold 2 minutes
# - Ramp-down: Return to 0 over 2 minutes
```

<!-- TODO: Replace generic example with DoW-specific performance scenario (tactical operations under load, real-time C2 response times) -->

---

## What GenAI-Generated Performance Tests Often Miss

GenAI can generate realistic-looking load tests that miss critical operational factors:

- **Actual operational load patterns** - Real user behavior varies from synthetic profiles
- **Infrastructure constraints** - Network bandwidth, database connections, memory limits
- **Realistic data distribution** - Production data skew, hot spots, cache hit rates
- **Geographic distribution** - Latency from different operational locations
- **Compound stress factors** - What happens when multiple systems are under load simultaneously

**Human reviewers must validate that performance tests reflect actual operational load and infrastructure constraints.**

---

## Governance Checklist

Before accepting GenAI-generated performance tests:

- [ ] Load scenarios validated against operational user behavior and traffic patterns
- [ ] Test environment mirrors production infrastructure (compute, network, database)
- [ ] Performance requirements traceable to operational SLAs and mission needs
- [ ] Test data reflects production data volume and distribution characteristics
- [ ] Results tracked over time to detect performance trends and regressions
- [ ] Findings documented for capacity planning and infrastructure scaling decisions

---

## Performance Regression Detection with GenAI

GenAI can analyze performance test results across builds to identify degradation:

**Example prompt for regression analysis:**


> *"Here are response time results from the last 5 performance test runs for the mission planning API. Identify any performance regressions or concerning trends."*

```
Run 1 (baseline):  p50=150ms, p95=450ms, p99=800ms
Run 2 (+1 week):   p50=155ms, p95=460ms, p99=810ms
Run 3 (+2 weeks):  p50=165ms, p95=520ms, p99=950ms
Run 4 (+3 weeks):  p50=180ms, p95=600ms, p99=1100ms
Run 5 (+4 weeks):  p50=210ms, p95=750ms, p99=1400ms
```

**GenAI analysis output:**


```
PERFORMANCE REGRESSION DETECTED

Trend: Steady degradation across all percentiles
- p50 increased 40% (150ms → 210ms)
- p95 increased 67% (450ms → 750ms)
- p99 increased 75% (800ms → 1400ms)

Severity: MEDIUM (approaching requirement threshold of 2000ms at p95)

Likely causes to investigate:
1. Data growth without proper indexing (check database query performance)
2. Memory leak causing garbage collection pressure (review heap usage trends)
3. Inefficient queries introduced in recent changes (check N+1 query patterns)
4. Increased external API latency (review dependency response times)

Recommendation: Investigate immediately before degradation impacts operations.
Correlate with deployment history and review application performance monitoring data.
```

**Human expert must:**

- Correlate with deployment and infrastructure changes
- Validate hypothesis with profiling and APM tools
- Prioritize investigation based on mission impact
- Determine root cause and implement fixes

---

## Integration with Performance Testing Tools

GenAI-generated performance tests work with standard tooling:

**Load Testing:**

- Locust, JMeter, Gatling, K6 (load generation and orchestration)
- Artillery, LoadRunner (enterprise load testing)

**APM Platforms:**

- Datadog, New Relic, Dynatrace, AppDynamics (production monitoring)

**Profiling:**

- py-spy, async-profiler, perf, gprof (code-level performance analysis)

**Infrastructure Monitoring:**

- Prometheus, Grafana (resource utilization and dashboards)

**Database Analysis:**

- Query analyzers, execution plan tools, slow query logs

GenAI generates load scenarios and analyzes trends; performance tools provide measurement, profiling, and operational insights.

---

[← Back to Testing Play](../testing-play.md)
