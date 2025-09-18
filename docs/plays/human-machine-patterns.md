---
# automatic badge generation
lifecycle: ga
last_updated: "2025-07-15"
---

# Human-Machine Interaction Patterns (HMT)

This reference defines common interaction models between humans and Generative AI tools across the software development lifecycle. These patterns shape team roles, trust boundaries, traceability, and governance posture.

## 1. Introduction

Adopting GenAI is not just about choosing a model—it's about defining *how* humans and machines work together. These patterns influence developer workflows, DevSecOps alignment, compliance posture, and trust calibration.

## 2. Interaction Patterns in Practice

| Pattern                        | Description                                                   | Benefits | Challenges |
|-------------------------------|---------------------------------------------------------------|----------|------------|
| Standalone Web Interfaces     | Browser-based, disconnected from toolchains                  | Easy to access | No traceability, encourages out-of-band use |
| IDE Plugins and Adapters      | Inline assistance in VSCode, JetBrains, etc.                 | Familiar UX | No prompt versioning, hard to share |
| AI-First IDEs / Workspaces     | Purpose-built GenAI environments (e.g., WindSurf, OpenHands) | Integrated agents | Changes team dynamics |
| Custom API Integrations       | Embedded model calls in codebases or pipelines               | High control | Requires governance |
| Agentic Platforms             | Autonomous agents handling multi-step logic                  | Automates workflows | Emergent behavior, trust risk |

## 3. Key Design Insight

> "Essentially, the human-in-the-loop approach reframes an automation problem as a Human-Computer Interaction (HCI) design problem..."
> — *Ge Wang, Stanford University*

## 4. Architectural Implications

Each pattern affects:
- Data flow boundaries
- Prompt versioning and auditability
- DevSecOps alignment
- Calibrated trust across teams

## 5. Reference

This guidance was introduced in [**Play Fundamentals for Designing an AI-Augmented Tool Chain**](fundamentals-play.md)
