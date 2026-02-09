# Agent-Native Environment

*Designing for non-human workers.*

## The Principle

Agents need environments designed for them. This is Factory.ai's key contribution: the recognition that standard development practices — stand-ups, kanban boards, open-ended tickets, branching strategies — are optimizations for human cognition. Agents have different needs: precise specifications, small scopes, objective verification criteria, and machine-readable context.

As Klaassen observes, agent output quality depends more on the engineering environment than on agent sophistication. A mediocre model working in a well-structured codebase with comprehensive CI, opinionated linters, and clear conventions will outperform a frontier model working in a chaotic codebase with no tests. The environment is the lever.

## What Agents Need

An agent-native environment provides:

**Objective verification.** Agents need to know whether their work is correct without asking a human. This means CI pipelines that run automatically, test suites that cover expected behavior, linters that enforce style, and type systems that catch errors at compile time. The more the environment can tell the agent "yes, this is right" or "no, this is wrong," the faster the feedback loop converges.

**Machine-readable context.** An AGENTS.md file at the repository root that answers three questions: how to build, test, and lint; how the codebase is organized at a high level; and what evidence must accompany a pull request. This replaces the onboarding conversation a senior engineer would have with a new team member.

**Small, bounded scopes.** Factory.ai emphasizes keeping tasks narrow enough that wrong assumptions surface before compounding. Underspecified requests like "fix the sign-in bug" or "improve API performance" set agents up for expensive drift. Bounded tasks like "fix the null pointer exception in AuthService.validate() by adding a guard clause, verified by the existing test suite" give agents what they need to converge.

**Recovery paths.** When agents drift — plans rewriting mid-execution, edits outside declared paths, claims without failing tests — the environment should make drift detectable and recoverable. Factory.ai's recommendation: tighten the spec, salvage good work, restart clean with improved instructions.

## The AGENTS.md Convention

Both Klaassen and Factory.ai converge on a specific artifact: a file at the repository root that serves as the agent's orientation document. Factory.ai's template covers:

- Core commands (build, test, lint, format)
- Project layout and key abstractions
- Development patterns (naming, error handling, testing conventions)
- Git workflow (branch naming, commit format, PR requirements)
- Required proof artifacts (what must pass before a PR is ready)

This file is the minimum viable agent-native environment. Without it, agents spend tokens rediscovering what any team member could tell them in five minutes.

## See Also

- [Compound Knowledge](compound-knowledge.md) — How the environment improves over time
- [Specification Discipline](../techniques/specification-discipline.md) — Writing specs that agents can act on
- [Risk-Tiered Automation](../techniques/risk-tiered-automation.md) — Graduated autonomy within the environment
- [Filesystem as Memory](../techniques/filesystem-as-memory.md) — Structuring the environment for agent cognition
- [The Seed](seed.md) — The specification that the environment helps agents interpret
