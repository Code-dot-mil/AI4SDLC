# AI4SDLC DoD CIO Guidance

This repository supports the creation and publication of MkDocs-based guidance developed under the **AI for Software Development Lifecycle (AI4SDLC)** initiative led by the **DoD CIO**. It is part of an ongoing effort to provide secure, mission-aware adoption practices for Generative AI (GenAI) within U.S. Department of Defense software delivery environments.

## 📚 About This Site

This site is designed using [MkDocs](https://www.mkdocs.org/) with the [Material for MkDocs theme](https://squidfunk.github.io/mkdocs-material/). It provides modular, evolving guidance—referred to as *Plays*—to help teams and leadership safely and effectively integrate GenAI into all phases of the software development lifecycle.

### Key Themes:
- Trust, traceability, and accountability in GenAI-assisted workflows
- Role-specific prompting practices and augmentation patterns
- Pipeline-integrated verification, validation, and policy enforcement
- Secure use of GenAI in classified, mission-critical, and export-controlled environments

## 🛠️ Setup Instructions

To build and serve the documentation locally:

```bash
# 1. Install MkDocs and Material theme
pip install mkdocs mkdocs-material

# 2. Serve locally
mkdocs serve

# 3. To build the static site
mkdocs build

```

## 🚧 Contribution Guidance
Contributions are welcome from DoD-affiliated collaborators. Please follow these rules:

Do not include controlled or classified information in any markdown files or assets.

Use the provided markdown patterns for callouts, footnotes, and glossary references.

Draft Plays should be added under plays/drafts/ and follow naming conventions.

All citations must follow IEEE-style footnote formatting.

For major content changes, open a discussion issue or contact a maintainer.

## 👥 Maintainers
This effort is led by contributors from the DoD CIO, supported by partners from the MITRE Corporation and the ArchAITecture Research Collaborative.

For questions or coordination:
📧 ArchAITecture@mitre.org