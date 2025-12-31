---
title: "Unit Testing with GenAI"
parent: "testing-play.md"
lifecycle: "beta"
last_updated: "2025-12-30"
---

# Unit Testing with GenAI

[← Back to Testing Play](../testing-play.md)

---

## What is Unit Testing?

Unit testing validates individual functions or modules in isolation with deterministic inputs and outputs. GenAI is most effective here because the scope is narrow, the expected behavior is well-defined, and function signatures provide clear structure for test generation.

---

## Where GenAI Helps

- **Positive test cases** - Validating expected behavior with valid inputs
- **Boundary conditions** - Testing edge cases (empty, null, max/min values)
- **Negative test cases** - Verifying error handling for invalid inputs
- **Test scaffolding** - Creating test structure that follows project conventions

---

## Example Requirement

**REQ-TR-102:** *"The system shall process approved requests within 3 seconds and log a warning if processing exceeds 5 seconds."*

---

## Example Prompt

> *"Generate three unit tests and one negative test for the code in the currently open file for requirement REQ-TR-102: 'The system shall process approved requests within 3 seconds and log a warning if processing exceeds 5 seconds.' Use the file to understand test structure and function signatures, but base all assertions and expected outcomes on the requirement, not on what the code currently does."*

---

## Example Output

```python
def test_process_within_three_seconds():
    result = process_request(valid_request)
    assert result.status == "success"
    assert result.duration <= 3

def test_process_between_three_and_five_seconds():
    result = process_request(moderate_load_request)
    assert 3 < result.duration <= 5

def test_process_exceeds_five_seconds_logs_warning():
    result = process_request(heavy_load_request)
    assert result.duration > 5
    assert "slow_processing_warning" in get_logs()

def test_reject_unapproved_request():
    result = process_request(unapproved_request)
    assert result.status == "error"
    assert result.code == "REQUEST_NOT_APPROVED"
```

<!-- TODO: Replace generic example with DoW-specific scenario (mission planning, secure messaging, authorization validation) -->

---

## What GenAI-Generated Unit Tests Often Miss

GenAI can generate syntactically correct tests that still miss important scenarios. Review GenAI-generated unit tests for:

- **Concurrency and race conditions** - GenAI doesn't know if your function will be called concurrently
- **Resource cleanup** - File handles, connections, locks that need explicit cleanup
- **State dependencies** - Implicit assumptions about global state or execution order
- **Exception propagation** - Whether exceptions should be caught, logged, or re-raised
- **Domain-specific edge cases** - Business rules that aren't obvious from the code structure

**Human reviewers must validate that tests actually verify the requirement, not just exercise the code.**

---

## Governance Checklist

Before accepting GenAI-generated unit tests:

- [ ] Every test is traceable to a specific requirement ID
- [ ] Test names clearly describe what is being validated
- [ ] Assertions check expected behavior, not implementation details
- [ ] Negative test cases verify proper error handling
- [ ] Tests run in isolation (no external dependencies)
- [ ] Coverage gaps identified in review comments

---

## Integration with Testing Tools

GenAI-generated unit tests work with standard testing frameworks:

- **Python:** pytest, unittest, nose2
- **JavaScript/TypeScript:** Jest, Mocha, Jasmine
- **Java:** JUnit, TestNG
- **C#/.NET:** xUnit, NUnit, MSTest
- **Go:** testing package, Testify

GenAI can adapt test output to match your project's testing framework conventions. Specify the framework in your prompt for better results.

---

## Tie-in with Test-Driven Development (TDD)

GenAI can help generate tests before implementation by scaffolding initial test structures and suggesting boundary cases early. This accelerates the "write the test first" habit, especially for teams new to TDD.

For detailed guidance on using GenAI with the full TDD Red → Green → Refactor workflow, see [Section 5.11: Test-Driven Development](../testing-play.md#511-genai-augmented-test-driven-development-tdd).

---

[← Back to Testing Play](../testing-play.md)
