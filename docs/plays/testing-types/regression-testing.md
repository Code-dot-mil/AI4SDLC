---
title: "Regression Testing with GenAI"
parent: "testing-play.md"
lifecycle: "beta"
last_updated: "2025-12-30"
---

# Regression Testing with GenAI

[← Back to Testing Play](../testing-play.md)

---

## What is Regression Testing?

Regression testing ensures that code changes, new features, or bug fixes don't break existing functionality. In continuous delivery environments, regression testing is critical for maintaining mission assurance as systems evolve rapidly. GenAI can help identify which tests to run based on code changes, analyze failure patterns across CI/CD runs, and suggest new regression tests when defects are fixed.

---

## When Regression Testing Occurs

In modern DevSecOps environments, regression testing happens continuously throughout the SDLC:

**Common triggers:**


- **Every code commit/PR** - Automated subset of regression tests runs in CI pipeline (fast feedback)
- **Merge to main branch** - Broader regression suite validates integration with other changes
- **Nightly builds** - Full regression suite runs to catch issues missed by fast checks
- **Pre-deployment gates** - Comprehensive regression validation before promoting to production
- **After bug fixes** - New regression tests added to prevent defect reoccurrence
- **Dependency updates** - Validates compatibility when libraries, frameworks, or OS patches are applied

GenAI helps determine **which tests to run** for each trigger, optimizing pipeline speed while maintaining coverage.

---

## Where GenAI Helps

- **Test selection** - Identifying which subset of tests to run based on code changes (risk-based test selection)
- **Failure pattern analysis** - Detecting trends in test failures across multiple CI/CD runs
- **Regression test generation** - Creating new tests when bugs are fixed to ensure they stay fixed
- **Test suite maintenance** - Identifying obsolete or redundant tests that can be removed

---

## Example Requirement

**REQ-REG-505:** *"After fixing defect DEF-1234 (authentication bypass on expired tokens), create regression tests to ensure the vulnerability does not reoccur in future releases."*

---

## Example Prompt for Test Selection

> *"Analyze the following code changes in this pull request. Identify which existing integration and system tests should be run to validate that no regressions were introduced. Focus on tests that exercise the authentication module, token validation, and session management."*
>
> *[Provide git diff or list of changed files]*

**GenAI Output (summarized):**


```
Recommended test execution based on code changes:

HIGH PRIORITY (must run):
- test_token_validation_expired_token()
- test_session_timeout_behavior()
- test_authentication_flow_with_refresh()
- test_authorization_after_token_expiry()

MEDIUM PRIORITY (should run):
- test_login_logout_complete_flow()
- test_concurrent_session_handling()
- test_api_requests_with_invalid_tokens()

LOW PRIORITY (optional):
- Full integration test suite (if time permits)

JUSTIFICATION:
- Authentication module modified (auth/validator.py)
- Token expiry logic changed (auth/tokens.py)
- Session management imports updated (session/manager.py)
```

---

## Example Prompt for Regression Test Generation

> *"Generate regression tests for defect DEF-1234: 'Authentication bypass on expired tokens.' The fix ensures that expired tokens are rejected with HTTP 401 and error code TOKEN_EXPIRED. Create tests that validate this behavior and would catch this bug if it reappears."*

**GenAI Output:**


```python
def test_expired_token_rejected():
    """Regression test for DEF-1234: Expired tokens must be rejected"""
    expired_token = generate_token_expired_1_hour_ago()
    response = api_client.get("/protected/resource",
                              headers={"Authorization": f"Bearer {expired_token}"})

    assert response.status_code == 401
    assert response.json()["error"] == "TOKEN_EXPIRED"

def test_expired_token_not_refreshable():
    """Regression test for DEF-1234: Expired tokens cannot be refreshed"""
    expired_token = generate_token_expired_1_hour_ago()
    response = api_client.post("/auth/refresh",
                               headers={"Authorization": f"Bearer {expired_token}"})

    assert response.status_code == 401
    assert response.json()["error"] == "TOKEN_EXPIRED"

def test_barely_expired_token_still_rejected():
    """Regression test for DEF-1234: Edge case - token expired 1 second ago"""
    expired_token = generate_token_expired_1_second_ago()
    response = api_client.get("/protected/resource",
                              headers={"Authorization": f"Bearer {expired_token}"})

    assert response.status_code == 401
    assert response.json()["error"] == "TOKEN_EXPIRED"

def test_valid_token_still_works():
    """Regression test for DEF-1234: Fix didn't break valid token handling"""
    valid_token = generate_valid_token()
    response = api_client.get("/protected/resource",
                              headers={"Authorization": f"Bearer {valid_token}"})

    assert response.status_code == 200
```

<!-- TODO: Replace generic example with DoW-specific scenario (C2 system defect regression, mission planning bug fix validation) -->

---

## What GenAI-Generated Regression Tests Often Miss

GenAI can generate syntactically correct regression tests that still miss important scenarios:

- **Historical context** - Why the bug occurred originally and what conditions led to it
- **Related defects** - Similar bugs that might exist in other modules
- **Environmental factors** - Configuration, deployment, or infrastructure issues that contributed
- **Operational impact** - Mission-critical workflows affected by the original defect
- **Root cause validation** - Whether the fix actually addresses the underlying issue

**Human reviewers must validate that regression tests target the actual vulnerability and would reliably catch reintroduction.**

---

## Governance Checklist

Before accepting GenAI-generated regression tests:

- [ ] Tests are explicitly linked to the defect ID (e.g., DEF-1234)
- [ ] Tests validate the fix, not just the symptom
- [ ] Tests cover edge cases related to the original defect
- [ ] Tests include a positive case confirming fix didn't break valid functionality
- [ ] Test names clearly indicate this is a regression test for a specific defect
- [ ] Tests are added to the standard regression test suite in CI/CD

---

## Test Selection Strategy with GenAI

GenAI can help implement risk-based test selection to optimize CI/CD pipeline runtime:

### **For every commit/PR:**
Ask GenAI to analyze code changes and recommend:
- Which unit tests to run (focus on changed modules)
- Which integration tests to run (focus on changed interfaces)
- Which system tests might be affected (focus on changed workflows)

### **Example prompt for CI/CD optimization:**

> *"Our full test suite takes 45 minutes to run. Based on these code changes [provide diff], recommend a subset of tests that covers the risk while completing in under 15 minutes. Categorize by risk level (high/medium/low) and estimated runtime."*

### **Human validation required:**
- Confirm GenAI's risk assessment aligns with operational priorities
- Ensure mission-critical paths are always tested regardless of code changes
- Override GenAI recommendations based on upcoming deployments or known risks

---

## Integration with CI/CD Tools

GenAI-generated regression test recommendations work alongside:

- **Test impact analysis tools** - Launchable, Ponicode, Codecov (AI-powered test selection)
- **CI/CD platforms** - GitHub Actions, GitLab CI, Jenkins (parameterized test execution)
- **Test management** - TestRail, Zephyr, qTest (regression test tracking)
- **Code coverage tools** - JaCoCo, Istanbul, Coverage.py (validating test selection)

GenAI helps generate recommendations; test impact analysis tools provide historical data and optimization.

---

## Regression Test Suite Maintenance

Over time, regression test suites can grow unwieldy. GenAI can help identify:

**Obsolete tests:**

> *"Review this regression test suite. Identify tests that may be obsolete because: (1) they test features that no longer exist, (2) they duplicate coverage of other tests, or (3) they test internal implementation details that have changed."*

**Redundant tests:**

> *"These three regression tests appear to test similar functionality. Analyze them and recommend if any can be consolidated or removed without losing coverage."*

**Important:** Human experts must approve any test removal. Never delete regression tests without understanding why they exist.

---

## Common Patterns

### Pattern 1: Bug Fix → Regression Test

Every time a defect is fixed, create a regression test:

1. Document the defect (ID, description, root cause)
2. Prompt GenAI to generate regression test validating the fix
3. Human reviews and adds test to regression suite
4. Link test to defect in test management system

### Pattern 2: Risk-Based Test Selection

For each PR or commit:

1. GenAI analyzes code changes and recommends test subset
2. CI/CD pipeline runs recommended tests (fast feedback)
3. Nightly builds run full regression suite (comprehensive validation)
4. Track test selection accuracy and adjust over time

### Pattern 3: Regression Test Triage

When regression tests fail:

1. GenAI analyzes failure logs and suggests likely root cause
2. GenAI identifies related tests that might also be affected
3. Human investigates and confirms root cause
4. Fix is validated with expanded regression test coverage

---

[← Back to Testing Play](../testing-play.md)
