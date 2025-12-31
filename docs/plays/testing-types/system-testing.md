---
title: "System Testing with GenAI"
parent: "testing-play.md"
lifecycle: "beta"
last_updated: "2025-12-30"
---

# System Testing with GenAI

[← Back to Testing Play](../testing-play.md)

---

## What is System Testing?

System testing validates complete mission workflows end-to-end across the entire integrated system. It verifies that the system meets operational requirements from the user's perspective, testing the full stack—UI, services, databases, external integrations—as a unified whole under realistic conditions.

---

## When System Testing Occurs

System testing happens when the complete system is assembled and before operational deployment:

**Common triggers:**


- **End of sprint/iteration** - Validates that newly integrated features work in the complete system
- **Before UAT (User Acceptance Testing)** - Ensures system is ready for stakeholder validation
- **Pre-deployment gates** - Final validation before promoting to staging or production
- **After major integration** - Validates system behavior after significant architectural changes
- **Release candidates** - Comprehensive system validation before declaring a release ready

GenAI helps generate test scenarios and scripts, but mission context and operational realism require human expertise.

---

## Where GenAI Helps

- **End-to-end scenario generation** - Creating complete user workflows from requirements
- **Test script scaffolding** - Generating step-by-step test procedures
- **User journey mapping** - Identifying paths through the system based on personas
- **Acceptance criteria validation** - Translating requirements into executable test cases

---

## Example Requirement

**REQ-SYS-701:** *"An operator shall be able to create a new mission plan, assign waypoints, validate against airspace restrictions, and submit for approval—all within 5 minutes for a standard 10-waypoint mission."*

---

## Example Prompt

> *"Generate a system test for requirement REQ-SYS-701. The test should validate the complete workflow from mission creation through approval submission. Include steps for UI interaction, waypoint assignment, airspace validation, and timing verification. Identify expected outcomes at each step."*

---

## Example Output

```gherkin
Feature: Mission Planning End-to-End Workflow
  As a mission operator
  I want to create and submit a mission plan
  So that it can be reviewed and approved for execution

  Scenario: Create standard 10-waypoint mission within time limit
    Given I am logged in as "mission_operator_1"
    And airspace restriction data is current
    And the mission planning system is in ready state

    When I create a new mission plan "ALPHA-2024-001"
    Then the mission editor should open within 2 seconds

    When I add the following waypoints:
      | Sequence | Latitude  | Longitude  | Altitude | Purpose       |
      | WP-001   | 38.8977   | -77.0365   | 1000ft   | Start         |
      | WP-002   | 38.9072   | -77.0369   | 1500ft   | Waypoint      |
      # ... (8 more waypoints)
      | WP-010   | 38.8951   | -77.0364   | 1000ft   | End           |

    Then all waypoints should appear on the map
    And the total flight path should be calculated

    When I validate the mission against airspace restrictions
    Then the system should complete validation within 10 seconds
    And no airspace violations should be detected

    When I submit the mission for approval
    Then the mission status should change to "PENDING_APPROVAL"
    And the submission timestamp should be recorded
    And the elapsed time from creation should be less than 5 minutes

    When I verify the audit log
    Then the following events should be recorded:
      - Mission created by mission_operator_1
      - 10 waypoints added
      - Airspace validation passed
      - Mission submitted for approval
```

<!-- TODO: Replace generic example with DoW-specific mission scenario (C2 workflow, intelligence tasking, logistics coordination) -->

---

## What GenAI-Generated System Tests Often Miss

GenAI can generate comprehensive test scripts that still miss critical operational factors:

- **Mission-critical workflow priorities** - Which scenarios actually matter for operations
- **Operational environment realism** - Network conditions, system load, concurrent users
- **Cross-system dependencies** - External systems, legacy integrations, data feeds
- **Actual user behavior patterns** - How operators really use the system under stress
- **Environmental variables** - Deployment configuration, security policies, infrastructure constraints

**Human reviewers must validate that system tests reflect real operational workflows and deployment realities.**

---

## Governance Checklist

Before accepting GenAI-generated system tests:

- [ ] Test validates a complete operational workflow from end user perspective
- [ ] Test includes realistic data and conditions from target deployment environment
- [ ] Test verifies all cross-system integrations and external dependencies
- [ ] Test includes timing and performance expectations aligned with requirements
- [ ] Test is traceable to system-level requirements or user stories
- [ ] Test results produce evidence suitable for RMF/ATO documentation

---

## Integration with System Testing Tools

GenAI-generated system tests work with standard testing frameworks:

- **BDD frameworks** - Cucumber, SpecFlow, Behave (Gherkin scenarios)
- **E2E testing tools** - Selenium, Playwright, Cypress (UI automation)
- **API testing** - Postman, REST Assured, Karate (service orchestration)
- **Test management** - TestRail, Zephyr, qTest (scenario organization and traceability)

GenAI generates test scenarios and scripts; testing tools execute them against the live system.

---

[← Back to Testing Play](../testing-play.md)
