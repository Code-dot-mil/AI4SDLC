# Security policy

This starter pack is a set of templates and configuration patterns. It does not ship executable services, but it can still create security risk if misused (especially information disclosure).

## Reporting a security issue
If you find:
- a pattern that encourages leaking sensitive identifiers,
- a prompt/template that asks for secrets,
- a rule that is too permissive,
- or a deterministic control that misses common leaks,

report it by opening an issue in the distribution repo (or notify the distributor via your normal channels).
**Do not include secrets or internal identifiers in the report.** Redact to placeholders first.

## High-risk failure mode: information disclosure
Assume AI tools can restate:
- internal hostnames/URLs,
- usernames/emails,
- numeric IDs,
- and other metadata.

Use the included redaction rules and (optionally) the CI scan to reduce this risk.
