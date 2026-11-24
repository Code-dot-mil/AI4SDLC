# Early Adopter Story

**Team / Program:** Navy Forge  
**Adoption stage at time of engagement:** Evaluation  
**Engagement date:** March 2026  
**Target publish date:** August 2026

## Background/Context

Modernizing the architecture. Fewer engineers. Faster deliveries. That is the operating reality the program using the Navy Forge software factory was working inside when it started asking whether AI could help. The program had to deliver new software releases faster while still modernizing the architecture underneath. Leadership wanted to move fast inside that release rhythm without creating extra cleanup later.

The Navy program working in the Navy Forge software factory regularly brought in a panel of industry experts to review its modernization work and compare it against current software practice. Those reviews intended to keep the program honest about where it was modernizing well and where it was falling behind. In one recent session, the panel strongly proposed incorporating AI into the development workflow. The panel message was direct: if the program wanted to improve development speed, it should test AI in the places where engineering teams were spending time on repeatable, reviewable work. The panel pointed to requirements support, code assistants, code rewriting, and testing as likely candidates.

That feedback gave the program a practical next step. The program built an evaluation to answer a narrower question: where could AI help the team deliver useful work faster inside the faster release cycle, and where would it just create more review work than value? The program wanted results tied to its own codebase and constraints, not general market claims.

## What They Evaluated

The program engineers built an evaluation around nine use cases essential to the program’s day-to-day delivery work: requirements verification, inline documentation, specification generation, greenfield code generation, code segmentation and rearchitecture,
bug analysis and fixes, dependency mapping, unit test creation, and functional test creation. For greenfield code generation the evaluation was focused on Java and containerizing services. For re-factoring related use cases those were focused on C++
source code.

They tested two interaction methods: an IDE plugin and a command line interface. The plugin worked better for smaller, file-scoped tasks. The command line interface made it possible for the AI to use context from multiple files, such as refactoring across classes or reasoning across a larger part of the codebase. The team also documented successful prompts and prompt sequences to compare results across different source code projects and provide a foundation for repeatable prompt patterns.

The team judged each use case on output quality and effort/time saved. Subject matter experts reviewed the outputs, compared them against expected engineering practice, and ran generated code when appropriate. The team measured time savings when the output met the quality expectation.

## Which AI4SDLC Plays Apply?

The team’s evaluation lined up with several AI4SDLC plays:

* [**Fundamentals for Designing an AI-Augmented Tool Chain**](https://code.mil/AI4SDLC/plays/fundamentals-play/): The program started with use cases first, not tools first. It also looked at hosting and environment constraints before deciding what could be evaluated in practice, and it framed value in terms of estimated hours saved and release-cycle impact.
* [**AI-Augmented Requirements Engineering**](https://code.mil/AI4SDLC/plays/requirements_engineering_play/): The team evaluated whether AI could trace requirements to source code and found that requirement inference and traceability were uneven.
* [**AI-Assisted Documentation Across the SDLC**](https://code.mil/AI4SDLC/plays/documentation-play/): For documentation use cases, the team treated source code as the source of truth and used AI to generate supporting artifacts from that foundation.
* [**Leading Practices for Code Completion and Generation**](https://code.mil/AI4SDLC/plays/code-gen-play/): The team tested AI on code generation, re-architecture, bug analysis, and dependency review. It also started shaping reusable prompt playbooks for recurring use cases and kept human review in the loop for generated code.
* [**AI-Augmented Testing**](https://code.mil/AI4SDLC/plays/testing-play/): The team evaluated AI for unit and functional test generation and found that AI could help with test scaffolding and edge cases, but still needed thorough human review because of truncation, hallucinated methods, and weak functional depth.

## How they are Evaluating AI Tools

The engineers devised detailed evaluation plans for each use case instead of running loose experiments. Each plan defined the environment, the available tools and models, the test cases to run, and the criteria for judging output quality by review of domain subject matter experts.

The success criteria focused on whether the output was usable and how much effort the team could save. If a tool generated content quickly but required heavy cleanup, the team did not treat that as a strong result. If a tool produced documentation, scaffolding, or draft artifacts that a skilled engineer could review and apply quickly, that counted as practical value.

The team also looked closely at workflow fit. The IDE plugin handled smaller tasks reasonably well but struggled when a task needed broader application context. The command line interface improved those cases, but context-window limits still showed up on large codebases and large-file operations. That mattered because the program was not trying to score tools in the abstract, but fit a tool into a real release workflow, at the developer IDE or inside a CI/CD pipeline.

## Outcomes and Metrics

The program is still in the evaluation phase and has completed one tool evaluation, with more planned as additional tools become available. The team reported time savings only when output quality criteria were satisfied.

|Metric|Play Source|Observed Value or Observation|
|-|-|-|
|Traceability coverage|[AI-Augmented Requirements Engineering](https://code.mil/AI4SDLC/plays/requirements_engineering_play/)|**Requirements verification** was inconsistent. The tool could describe functionality but could not reliably infer or trace the requirement.|
|Time savings per artifact type|[AI-Assisted Documentation Across the SDLC](https://code.mil/AI4SDLC/plays/documentation-play/)|**Inline documentation** was significantly faster, with a skilled engineering using AI with good prompts generating all documentation artifacts in about a minute with approximately 30 minutes of review time compared to what would normally take them 6-7 hours. It was generally accurate, though performance dropped on large classes or methods.|
|Estimated hours saved by automation|[Fundamentals for Designing an AI-Augmented Tool Chain](https://code.mil/AI4SDLC/plays/fundamentals-play/)|**Specification generation** was about 6x faster when the team used iterative prompting. This was done by providing the AI tool access to the code and asking it to generate the Open API specifications for new APIs as well as generating code from the specifications.|
|Review delta: manual changes after GenAI suggestions|[Leading Practices for Code Completion and Generation](https://code.mil/AI4SDLC/plays/code-gen-play/)|**Greenfield code generation** was faster for method completion, architecture, data modeling, and test skeletons, but algorithmic logic still needed significant human review. An engineer with good prompts was able to create scaffolding and design artifacts in 5-6 hours compared to what they estimated would have taken 40 hours to complete otherwise.|
|Post-generation defect rate / code quality signals|[Leading Practices for Code Completion and Generation](https://code.mil/AI4SDLC/plays/code-gen-play/)|**Bug analysis and fixes** were unreliable, and dependency mapping was inconsistent enough to require repeated review and re-prompting.|
|Ratio of human vs. AI-generated test cases used in production|[AI-Augmented Testing](https://code.mil/AI4SDLC/plays/testing-play/)|**Unit and functional test generation** provided a starting point; the outputs needed thorough human review because of truncation, hallucinated methods, and shallow coverage.|

Code segmentation and re-architecture and the dependency mapping use cases were very limited by token limits and by the context window. They are not included in the table above.

Based on those results, the engineers recommended quick integration for a subset of use cases, selective integration for others, and deferring or avoiding some areas given the tool's current capabilities.

The program has not yet adopted a specific tool suite; additional tools are still under evaluation, and any selection must clear hosting and security approval for the mission's impact level.

## Lessons Learned

The program learned that AI value depends on workflow fit, not headline capability. Smaller, bounded tasks such as inline documentation, specification drafting, and greenfield scaffolding produced the strongest results. Large-codebase refactoring, bug fixing, and dependency analysis produced more friction because the tool struggled with context limits and output consistency.

The team also found that iterative prompting helped. In several use cases, the best results came from a review-and-reprompt loop rather than from a single prompt. That pushed the program toward building prompt playbooks for repeatable tasks instead of treating prompting as an individual habit.

The next steps are practical: build prompt playbooks, set mandatory human review points for the higher-risk use cases, and start collecting productivity benchmarks over time so future tool decisions reflect evidence from the program’s own release environment.

The program continues to progress in utilizing the AI tools that are available in their Engineering and Development environment to increase the efficiency in engineering and software development workflows.

To dive deeper or get more information, reach out to the DoW #AI4SDLC team.