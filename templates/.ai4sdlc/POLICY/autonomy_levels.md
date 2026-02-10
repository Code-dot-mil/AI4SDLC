# Autonomy Levels (Simple Contract)

Use these levels to communicate what an AI assistant is allowed to do.

- **Level 0 — Suggest**: Provide advice only. No file edits, no command execution.
- **Level 1 — Draft Artifacts**: Draft text/code snippets, plans, checklists. Human applies changes.
- **Level 2 — Propose Changes**: Produce a patch/diff proposal + test plan. Human reviews and applies.
- **Level 3 — Sandbox Execute**: May run commands in an isolated, non-production sandbox with explicit approval.
- **Level 4 — Create PR/MR**: May open a PR/MR with changes and evidence, but cannot merge.
- **Level 5 — Autonomous Merge/Deploy**: Not recommended for most environments; requires formal governance.

**Governed output requirement:** Every governed response MUST declare:
`Autonomy level used: L0|L1|L2`

**Recommended baseline for broad adoption**: Level 1–2 with mandatory human review.
