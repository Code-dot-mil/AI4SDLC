# Early Adopter Story

**Team / Program:** Army SMDC CoE JFT  
**Adoption stage at time of engagement:** Embrace  
**Engagement date:** April 10, 2026  
**Target publish date:** June 2026

## Background/Context

This team was pulled by a vision to start incorporating AI in the software development lifecycle after the senior executive provided the direction for increased use. They run a compact delivery organization and needed to increase output without adding headcount. The development side has six people, and the broader team also supports cloud engineering, Kubernetes operations, infrastructure as code, and GitLab-based DevSecOps. The team had already standardized on GitLab and wanted AI to act as a force multiplier across coding, testing, infrastructure work, and troubleshooting. People were already experimenting with public AI tools in limited ways, but those tools could not safely work against the team’s full codebase or internal context.

## What Was Adopted

The team adopted AI in two stages. First, it piloted GitLab Duo with a small set of trial licenses. Later, it expanded to a broader workflow built around Anthropic models accessed through AWS Bedrock in GovCloud. That broader workflow included GitLab Duo inside GitLab and Claude Code on individual workstations, both pointed at the same backend.

The team’s approach lined up with several AI4SDLC plays:

- [**Fundamentals for Designing an AI-Augmented Tool Chain**](https://code.mil/AI4SDLC/plays/fundamentals-play/): The team picked a controlled hosting path through AWS Bedrock in GovCloud instead of relying on public SaaS tools. It also treated AI as part of the wider toolchain, not just as a coding add-on, and used it across development, operations, and infrastructure workflows.
- [**Leading Practices for Code Completion and Generation**](https://code.mil/AI4SDLC/plays/code-gen-play/): The team used AI for code generation, refactoring, large migrations, and unit test scaffolding, while keeping human review and developer accountability in the merge process. The team used AI to generate unit tests and close part of a testing gap, but it kept humans in the loop because generated tests often reflected the current implementation and the quality of the original requirements.
- [**AI-Assisted Documentation Across the SDLC**](https://code.mil/AI4SDLC/plays/documentation-play/): The team used AI to generate infrastructure diagrams and other technical documentation from infrastructure-as-code and system context.
- [**Navigating the AI Autonomy Continuum**](https://code.mil/AI4SDLC/plays/ai-autonomy_continuum_play/): In practice, the team stayed mostly in Pattern 1 - Assistive Tools and Pattern 2 - Delegated Agents. Developers used AI as an assistive tool and as a bounded teammate, but the team did not hand over merge or deployment authority. 
- [**AI Workflow Design and Governance**](https://code.mil/AI4SDLC/plays/ai_sdlc_workflows-play/): The team built shared Claude markdown files, reusable skills, and project-specific instructions so developers started with the same role, context, constraints, and coding guidance. The team controlled shared persona-like behavior through repo-managed context files, locked organizational rules, and phased rollout instead of letting every user invent a separate workflow.

The team did not just “turn it on.” It built shared practices around context, coding guidance, and team behavior. The most important practice was maintaining project-level Claude markdown files that gave the model persistent context about the application, coding rules, and project structure. The team also created reusable skills for recurring tasks such as GitLab issue creation, Kubernetes management and monitoring, and infrastructure diagram generation.

To keep output consistent, the team added coding standards and framework-specific instructions to the shared context. For example, it told the model not to generate plain HTML controls where the application expected Telerik controls. It also corrected undesirable default patterns, such as generating JavaScript with `var` instead of the team’s preferred style. The team checked these rules and context files into the codebase so every developer started with the same guidance.

## How it Was Implemented

The team began with a pilot group. GitLab Duo started with about five users. Claude Code also started small: the cloud engineer enabled it first, tested it, then handed it to the development lead for a week or two before expanding to the rest of the developers and then beyond. That phased rollout gave the team time to set standards before wider adoption.

Roles mattered. The cloud engineer acted as the primary integrator and internal expert. He configured the AWS Bedrock access path, built initial skills, researched best practices, and created top-level controls. The development lead translated those capabilities into day-to-day developer workflows and reinforced accountability for code quality. Developers used AI to write code, refactor functions, generate unit tests, and help with large migrations, but each developer still owned the code they submitted. In code review, “the AI wrote it” did not count as an explanation. As the development team lead said: "Each team member is responsible for their code... when it comes time for a code review, don't tell me that Claude said it was good... you need to be able to explain it. You need to be able to understand what you're checking in."


The team also added governance through the repo and the platform. Developers could create local skills for their own use, but project-level context files, rules, and shared skills were committed to source control. Changing those shared instructions required the same merge request process as other codebase changes. At a higher level, the cloud engineer maintained locked-down organizational rules and skills that users could not change.

## Outcomes and Metrics

The team saw faster execution on several concrete tasks. One quick win came from infrastructure documentation: AI generated a visual infrastructure diagram, including the Kubernetes cluster, in about five minutes from infrastructure-as-code artifacts. The team compared that with roughly a week of manual effort through the conventional route.

On the development side, AI helped with both narrow and broad tasks. In one case, AI-assisted refactoring reduced a JavaScript function's execution time from about one minute to about two seconds. In another, the team used AI heavily during a migration from a .NET Framework-based data processing application to a .NET Core 10 application spanning about 120 projects. Developers supervised and corrected the work, but Claude handled most of the migration effort.

| Metric | Play Source | Observed Value or Observation |
|---|---|---|
| Time savings per artifact type | [AI-Assisted Documentation Across the SDLC](https://code.mil/AI4SDLC/plays/documentation-play/) | AI generated an infrastructure diagram in about 5 minutes versus roughly 1 week of manual effort. |
| Time saved on routine coding tasks | [Fundamentals for Designing an AI-Augmented Tool Chain](https://code.mil/AI4SDLC/plays/fundamentals-play/) | AI-assisted refactoring cut the execution time of one JavaScript function from about 60 seconds to about 2 seconds. |
| Percentage of teams actively using AI in at least one SDLC phase | [Fundamentals for Designing an AI-Augmented Tool Chain](https://code.mil/AI4SDLC/plays/fundamentals-play/) | After about 3 months of Claude Code use, the team reported that almost everyone was using it for something. |
| Review delta: manual changes after GenAI suggestions | [Leading Practices for Code Completion and Generation](https://code.mil/AI4SDLC/plays/code-gen-play/) | Developers supervised and corrected AI output during migrations and routine coding work; the team did not track a formal percentage. |
| Ratio of human vs. AI-generated test cases used in production | [AI-Augmented Testing](https://code.mil/AI4SDLC/plays/testing-play/) | AI helped fill a testing gap, but humans still reviewed generated tests before use; the team did not track the ratio. |
| Autonomy level distribution | [AI Workflow Design and Governance](https://code.mil/AI4SDLC/plays/ai_sdlc_workflows-play/) | This metric can be used to track workflow health and covers the levels of autonomy that the team is using in practice. There are 6 levels that progress in the amount of autonomy that the AI is given. Level 0 is Suggest and is where the AI provides advice but makes no edits. Level 6 is Autonomy Merge/Deploy is where oversight of the AI is only provided by monitoring and exception handling. The team stayed mostly in draft/propose modes (level 1/2) with human review. It did not hand AI merge or deploy authority. |
| AI adoption and efficiency / usage visibility | [AI Workflow Design and Governance](https://code.mil/AI4SDLC/plays/ai_sdlc_workflows-play/) | A nightly CloudWatch-to-S3-to-SES report showed usage by individual profile, created a leaderboard, and gave leaders cost visibility for planning. It queries CloudWatch logs for Bedrock usage data, calculates metrics per user using everyone's inference profile, writes historical data to S3, generates an email with the rankings and cost, then sends an email via AWS simple email service (SES). |

## Lessons Learned

The team learned that rollout discipline mattered as much as model access. Starting with a small pilot gave them time to build shared context files, coding rules, and skills before usage fragmented across individuals.

The team also learned that AI could speed up work and create problems just as quickly. The model could write code fast, and it could break code fast. Unit test generation made that tradeoff visible. It helped close a major testing gap, but it also assumed the existing code was right. When requirements were vague or missing, the generated tests inherited those weaknesses.

Prompting and context setup also slowed some people down at first. Some developers adopted the tools immediately; others hesitated because they did not know how to prompt effectively or where AI fit into their workflow. The daily usage email helped managers spot low adoption and coach those users directly.

Metrics for things like line coverage, acceptance, and cybersecurity were being tracked. However, there is an opportunity to incorporate additional metrics in the future, potentially for the cognitive impact or for the activities the team is already doing, but not tracking such as the ratio of human vs. AI-generated test cases used in production and manual changes after GenAI suggestions.

What worked better than expected was the transfer of expert knowledge. The cloud engineer turned his operational know-how into reusable skills, which cut repeated questions from teammates and put specialized practices closer to the point of work. That let the team spread internal knowledge without writing long documentation first.

To dive deeper or get more information, reach out to the DoW #AI4SDLC team.