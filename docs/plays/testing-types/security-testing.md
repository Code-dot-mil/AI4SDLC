---
title: "Security Testing with GenAI"
parent: "testing-play.md"
lifecycle: "beta"
last_updated: "2025-12-30"
---

# Security Testing with GenAI

[← Back to Testing Play](../testing-play.md)

---

> **Important:** This guidance provides examples of how GenAI can augment security testing workflows. It does not replace existing DoD or Department of War security testing guidance, policies, or requirements. All security testing must comply with applicable DoD directives (e.g., DoDI 8500.01, DoDI 8531.01), RMF processes, and organization-specific security requirements.

---

## What is Security Testing?

Security testing validates that systems resist attacks, protect sensitive data, and follow secure coding practices. It identifies vulnerabilities before adversaries can exploit them. GenAI assists with static analysis, dynamic testing scenarios, and fuzzing—but **all security findings require expert validation**. GenAI supplements, never replaces, professional security assessment.

---

## When Security Testing Occurs

Security testing happens continuously throughout the SDLC and at critical RMF gates:

**Common triggers:**



- **Every code commit** - Automated SAST scans identify vulnerabilities in new code
- **Pull request review** - Security-focused code review with GenAI-assisted analysis
- **Pre-deployment gates** - DAST and penetration testing before production release
- **RMF Assessment & Authorization (A&A)** - Comprehensive security testing for ATO
- **After security patches** - Regression tests ensure vulnerabilities stay fixed
- **Continuous monitoring (Con Mon)** - Ongoing vulnerability scanning per DoDI 8531.01
- **Scheduled security audits** - Periodic comprehensive security assessments
- **Threat model updates** - When new threats emerge or system architecture changes

GenAI helps generate security test scenarios and identify vulnerability patterns, but security experts must validate findings and assess exploitability.

---

## Where GenAI Helps

- **SAST vulnerability identification** - Finding common vulnerability patterns in source code
- **DAST scenario generation** - Creating runtime security test cases
- **Fuzzing seed creation** - Generating diverse malicious inputs to find edge-case vulnerabilities
- **Security regression tests** - Ensuring fixed vulnerabilities don't reappear
- **Mapping to standards** - Linking findings to OWASP Top 10, CWE, NIST controls

---

## Example Requirement (SAST)

**REQ-SEC-401:** *"The authentication module shall use parameterized queries for all database operations and shall not construct SQL queries through string concatenation."*

---

## Example Prompt (SAST)

> *"Review this authentication function for security vulnerabilities. Check for: SQL injection, command injection, improper input validation, weak cryptography, hard-coded secrets, and insufficient logging. Reference OWASP Top 10 and CWE-89 (SQL Injection)."*

**Code being reviewed:**


```python
def authenticate_user(username, password):
    query = f"SELECT * FROM users WHERE username='{username}' AND password='{password}'"
    result = db.execute(query)
    return result
```

---

## Example Output (SAST)

**GenAI-generated security findings:**


```
CRITICAL: SQL Injection Vulnerability (CWE-89, OWASP A03:2021)
Location: authenticate_user(), line 2
Issue: Direct string interpolation in SQL query allows injection attacks
Evidence: query = f"SELECT * FROM users WHERE username='{username}'..."
Impact: Attacker can bypass authentication, extract data, or modify database
Recommendation: Use parameterized queries with prepared statements

Example exploit: username="' OR '1'='1" bypasses authentication

CRITICAL: Plaintext Password Handling
Location: authenticate_user(), line 2
Issue: Password compared in plaintext without hashing
Evidence: password='{password}' suggests plaintext comparison
Impact: Passwords exposed in logs, database breach reveals all passwords
Recommendation: Use bcrypt/argon2 for password hashing with unique salt

HIGH: Insufficient Logging
Location: authenticate_user()
Issue: No audit log for authentication attempts (success or failure)
Impact: Cannot detect brute force attacks or account compromises
Recommendation: Log authentication events with timestamp, username, source IP, outcome
```

**Human security expert must:**

- Validate exploitability in actual deployment environment
- Assess business risk and mission impact
- Prioritize fixes based on threat model
- Verify recommendations align with security architecture

<!-- TODO: Replace generic example with DoW-specific security scenario (classified system access control, SCIF authentication, mission data protection) -->

---

## Example Requirement (DAST)

**REQ-SEC-305:** *"The API shall reject requests with missing or invalid authorization headers and return HTTP 401 with error code AUTH_REQUIRED."*

---

## Example Prompt (DAST)

> *"Generate DAST security test scenarios for requirement REQ-SEC-305. Include tests for: missing auth header, malformed auth header, expired token, tampered token signature, valid token with insufficient privileges. Use Python with requests library."*

---

## Example Output (DAST)

```python
def test_missing_auth_header_returns_401():
    response = requests.get("https://api.example.mil/protected", headers={})
    assert response.status_code == 401
    assert response.json()["error"] == "AUTH_REQUIRED"

def test_malformed_auth_header_returns_401():
    response = requests.get(
        "https://api.example.mil/protected",
        headers={"Authorization": "NotAValidFormat"}
    )
    assert response.status_code == 401

def test_expired_token_returns_401():
    expired_token = generate_expired_jwt()
    response = requests.get(
        "https://api.example.mil/protected",
        headers={"Authorization": f"Bearer {expired_token}"}
    )
    assert response.status_code == 401
    assert response.json()["error"] == "TOKEN_EXPIRED"

def test_tampered_token_signature_returns_401():
    valid_token = generate_valid_jwt()
    tampered_token = valid_token[:-10] + "TAMPERED00"  # Corrupt signature
    response = requests.get(
        "https://api.example.mil/protected",
        headers={"Authorization": f"Bearer {tampered_token}"}
    )
    assert response.status_code == 401

def test_insufficient_privileges_returns_403():
    low_privilege_token = generate_jwt(role="read-only")
    response = requests.delete(
        "https://api.example.mil/protected/resource",
        headers={"Authorization": f"Bearer {low_privilege_token}"}
    )
    assert response.status_code == 403
    assert response.json()["error"] == "INSUFFICIENT_PRIVILEGES"
```

---

## What GenAI-Generated Security Tests Often Miss

GenAI can identify common vulnerability patterns but misses critical security context:

- **Threat model alignment** - Whether vulnerabilities actually matter for your threat landscape
- **Exploitability assessment** - If vulnerabilities can be exploited in your environment
- **Defense-in-depth** - Compensating controls that mitigate risk
- **Organizational security context** - Classification levels, compartmentalization, network isolation
- **False positive identification** - Distinguishing real threats from unlikely scenarios

**All GenAI security findings must be triaged and validated by security professionals before action.**

---

## Governance Checklist

Before accepting GenAI-generated security tests:

- [ ] All security test code reviewed and approved by security team
- [ ] Tests run only in approved test environments (never production)
- [ ] Findings triaged and prioritized by security professionals
- [ ] High/critical findings tracked in official vulnerability management system (e.g., eMASS)
- [ ] Test results logged for RMF artifacts (SAR, SSP, POA&M)
- [ ] Tests aligned with organizational threat model and risk assessment
- [ ] Findings mapped to NIST SP 800-53 controls where applicable
- [ ] Vulnerability remediation timelines comply with DoDI 8531.01 requirements

---

## Integration with Security Testing Tools

GenAI-generated security tests work alongside professional security tools:

**SAST Tools:**

- SonarQube, Fortify, Checkmarx, Veracode (automated code scanning)
- Semgrep, CodeQL (custom rule-based analysis)

**DAST Tools:**

- OWASP ZAP, Burp Suite (dynamic runtime testing)
- Acunetix, Nessus (vulnerability scanning)

**Secret Scanning:**

- GitGuardian, TruffleHog, detect-secrets (credential detection)

**Dependency Scanning:**

- Dependabot, Snyk, WhiteSource (vulnerable library detection)

**Fuzzing:**

- AFL, LibFuzzer, Peach (automated input fuzzing)

GenAI helps generate test ideas and scenarios. Security tools provide systematic coverage, validation, and compliance reporting.

---

## Security Testing Workflow

> **Note:** This workflow section is unique to security testing because it involves multiple types of testing (SAST, DAST, penetration testing) that happen at different stages of the SDLC, each with distinct tools, techniques, and timing requirements.

> **Important:** The workflows below are example patterns for integrating GenAI into security testing processes. Organizations must adapt these examples to align with their existing security testing procedures, tooling, and compliance requirements.

**1. Continuous SAST (every commit):**
- GenAI analyzes new code for vulnerability patterns
- Automated SAST tools scan for known vulnerabilities
- Security team triages findings

**2. DAST (pre-deployment gates):**
- GenAI generates runtime security test scenarios
- DAST tools execute tests against running application
- Security team validates exploitability

**3. Security Regression (after fixes):**
- GenAI generates regression tests for fixed vulnerabilities
- Tests added to CI/CD pipeline to prevent reintroduction
- Tracked in security tracking system

**4. Penetration Testing (scheduled):**
- Professional security assessors conduct manual testing
- GenAI assists with test case generation
- Results feed back into automated test suite

---

## Vulnerability Management (DoDI 8531.01)

GenAI-generated security findings must be managed through the official DoD vulnerability management process defined in DoDI 8531.01. Security teams should:

- Track all findings in the official vulnerability management system
- Follow established processes for assessment, remediation, and reporting
- Document findings and remediation status for RMF compliance

Refer to DoDI 8531.01 for the complete vulnerability management framework and requirements.

---

## DoD and NIST Security Testing Framework

This guidance aligns with DoD cybersecurity and vulnerability management policies:

**Key DoD Instructions:**

- **DoDI 8531.01** - DoD Vulnerability Management: Establishes uniform vulnerability management programs and requires efficient vulnerability assessment techniques throughout the system lifecycle
- **DoDI 8510.01** - Risk Management Framework (RMF): Governs security testing and assessment for ATO processes
- **DoDI 8500.01** - Cybersecurity: Establishes DoD cybersecurity program and requirements

**NIST Standards:**

- **NIST SP 800-53** - Security and Privacy Controls: Defines security testing controls (CA-8, RA-5, SI-2)
- **NIST SP 800-115** - Technical Guide to Information Security Testing and Assessment

GenAI-augmented security testing supports these frameworks by accelerating test generation and vulnerability identification, while maintaining human oversight required for DoD compliance.

---

## References

- U.S. Department of Defense, "DoD Vulnerability Management," DoDI 8531.01, Sept. 2020. [Online]. Available: https://www.esd.whs.mil/Portals/54/Documents/DD/issuances/dodi/853101p.pdf
- U.S. Department of Defense, "Risk Management Framework (RMF) for DoD Information Technology (IT)," DoDI 8510.01, July 2023. [Online]. Available: https://www.esd.whs.mil/Portals/54/Documents/DD/issuances/dodi/851001p.pdf
- U.S. Department of Defense, "Cybersecurity," DoDI 8500.01, Mar. 2024. [Online]. Available: https://www.esd.whs.mil/Portals/54/Documents/DD/issuances/dodi/850001p.PDF
- National Institute of Standards and Technology, "Security and Privacy Controls for Information Systems and Organizations," NIST SP 800-53 Rev. 5, Sept. 2020. [Online]. Available: https://doi.org/10.6028/NIST.SP.800-53r5

---

[← Back to Testing Play](../testing-play.md)
