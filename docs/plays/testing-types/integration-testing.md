---
title: "Integration Testing with GenAI"
parent: "testing-play.md"
lifecycle: "beta"
last_updated: "2025-12-30"
---

# Integration Testing with GenAI

[← Back to Testing Play](../testing-play.md)

---

## What is Integration Testing?

Integration testing validates behavior across component or service boundaries. Unlike unit tests that isolate individual functions, integration tests verify that multiple components work correctly together—validating interface contracts, data flow, error handling, and behavioral correctness when systems interact.

---

## When Integration Testing Occurs

Integration testing happens when components are combined and throughout the SDLC:

**Common triggers:**


- **After unit testing** - Once individual components pass, integration tests validate they work together
- **During feature development** - As new APIs or services are built, integration tests validate contracts
- **Before deployment** - Pre-production gates ensure services integrate correctly in target environment
- **After dependency updates** - Validates compatibility when external services or libraries change
- **Continuous integration** - Automated integration tests run on every merge to catch boundary issues early

GenAI helps generate contract tests and error scenarios, but engineers must validate tests reflect actual system architecture.

---

## Where GenAI Helps

- **API contract validation** - Testing request/response schemas, status codes, and data formats
- **Error handling scenarios** - Generating tests for timeouts, malformed responses, service unavailability
- **Mock/stub generation** - Creating test doubles for external dependencies
- **Data consistency checks** - Verifying data flows correctly across component boundaries

---

## Example Requirement

**REQ-INT-201:** *"When the Authorization Service receives a valid token, the Resource API shall grant access and log the request with user ID and timestamp."*

---

## Example Prompt

> *"Generate integration tests for requirement REQ-INT-201. The test should validate the contract between Authorization Service and Resource API. Include positive cases (valid token), negative cases (invalid/expired token), and verify logging behavior. Use mocked HTTP calls for the Authorization Service."*

---

## Example Output

```python
def test_valid_token_grants_access(requests_mock):
    # Mock authorization service response
    requests_mock.get(
        "https://auth.service/validate",
        json={"valid": True, "user_id": "user-123"}
    )

    # Call resource API
    response = resource_api.get_protected_resource(token="valid-token-abc")

    # Verify access granted
    assert response.status_code == 200
    assert response.json()["access"] == "granted"

    # Verify logging
    logs = get_audit_logs()
    assert any(
        log["user_id"] == "user-123" and "timestamp" in log
        for log in logs
    )

def test_invalid_token_denies_access(requests_mock):
    requests_mock.get(
        "https://auth.service/validate",
        json={"valid": False, "error": "TOKEN_INVALID"}
    )

    response = resource_api.get_protected_resource(token="bad-token")

    assert response.status_code == 403
    assert response.json()["error"] == "ACCESS_DENIED"

def test_auth_service_timeout_returns_503(requests_mock):
    requests_mock.get(
        "https://auth.service/validate",
        exc=requests.exceptions.Timeout
    )

    response = resource_api.get_protected_resource(token="valid-token")

    # Should fail gracefully when dependency is unavailable
    assert response.status_code == 503
    assert response.json()["error"] == "SERVICE_UNAVAILABLE"
```

<!-- TODO: Replace generic example with DoW-specific scenario (tactical data link integration, C2 system interoperability) -->

---

## What GenAI-Generated Integration Tests Often Miss

GenAI can generate syntactically correct integration tests that miss critical scenarios:

- **Realistic network conditions** - Latency, packet loss, partial failures, retries
- **Authentication/authorization complexity** - Mutual TLS, certificate rotation, token refresh flows
- **Data consistency** - Eventual consistency, transaction boundaries, rollback behavior
- **Deployment topology** - Service mesh routing, load balancers, DNS resolution, failover
- **Operational failure modes** - What actually breaks in your environment under real conditions

**Human reviewers must validate that integration tests reflect actual deployment architecture and realistic failure modes.**

---

## Governance Checklist

Before accepting GenAI-generated integration tests:

- [ ] Tests validate the actual interface contract documented in system architecture
- [ ] Error scenarios reflect realistic failure modes in deployment environment
- [ ] External dependencies are properly mocked or use dedicated test instances
- [ ] Tests verify observable side effects (logs, metrics, database state, message queues)
- [ ] Tests are traceable to integration requirements or interface specifications
- [ ] Security controls (authentication, authorization, encryption) are validated

---

## Integration with Contract Testing Tools

GenAI-generated integration tests work well alongside contract testing frameworks:

- **Pact** - Consumer-driven contract testing across teams
- **Spring Cloud Contract** - Contract-first API development and validation
- **OpenAPI/Swagger** - Schema validation and contract enforcement
- **Postman/Newman** - API testing and collection-based validation

GenAI can help generate test scenarios; contract testing tools enforce consistency and enable independent deployment.

---

[← Back to Testing Play](../testing-play.md)
