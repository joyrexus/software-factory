# Compound Knowledge

*Organizational learning at machine speed.*

## The Principle

Knowledge from each task should feed into the next. This is Larson's key contribution: the Compound step, where agents summarize learnings from completed work into structured formats that future planning phases consult. The result is an organization that gets measurably better at producing software with each iteration — not through individual skill growth, but through accumulated, machine-readable institutional knowledge.

Human organizations compound knowledge too, but slowly and lossy. Tribal knowledge lives in people's heads. Post-mortems get written and forgotten. Best practices become outdated. The compound knowledge principle treats knowledge accumulation as a first-class engineering concern, not a side effect of experience.

## Implicit vs. Explicit Compounding

Knowledge compounds in two ways.

**Implicit compounding** happens through the codebase itself. Well-structured code, comprehensive tests, and consistent patterns teach the next agent how to work in that environment. As Larson observes, agent output quality depends more on the engineering environment than on agent sophistication. A mature codebase with good patterns produces better agent output than a greenfield project, because the codebase *is* compound knowledge.

**Explicit compounding** happens through deliberate knowledge capture. Larson's framework has agents write structured summaries — what was attempted, what worked, what failed, what patterns were discovered — into files that planning phases consult. Factory.ai's AGENTS.md serves a similar function: a living document that tells future agents how to work in this codebase, updated as the team learns.

The most effective factories do both. Implicit compounding sets the floor. Explicit compounding raises the ceiling.

## AGENTS.md as Living Compound Knowledge

Both Larson and Factory.ai converge on a specific mechanism: a file at the repository root that encodes compound knowledge for agents. Larson calls it AGENTS.md and structures it with workflow commands. Factory.ai's version answers three questions: how to build/test/lint, how the codebase is organized, and what evidence must accompany a pull request.

This file is the most tangible artifact of compound knowledge. It grows with the project, encoding patterns that would otherwise exist only in the heads of senior engineers. When an agent encounters a new pattern — a tricky integration, an unexpected failure mode, a configuration quirk — the learning gets written into the file, available to every future agent session.

## See Also

- [The Feedback Loop](feedback-loop.md) — The mechanism that creates compound knowledge
- [Filesystem as Memory](../techniques/filesystem-as-memory.md) — The storage substrate for compound knowledge
- [Pyramid Summaries](../techniques/pyramid-summaries.md) — Structuring compound knowledge at multiple zoom levels
- [Gene Transfusion](../techniques/gene-transfusion.md) — Compounding knowledge across codebases, not just within one
- [Agent-Native Environment](agent-native-environment.md) — The environment that compound knowledge shapes
