# The Conversation

In late 2024, multiple teams working independently reported a shift in agentic coding workflows: models appeared to compound correctness rather than error. Three intellectual lineages converged on a shared formula — **Seed → Validation → Feedback Loop**, fueled by tokens — raising questions about what it means, what infrastructure it requires, what remains unproven, and how the broader community is responding.

For the full framing, see the [meta overview](README.md). For specific threads:

- **[The moment and the formula](meta.md)** — the convergence and the shared insight
- **[Three lineages](paradigm.md)** — compound engineering, agent-native development, and the software factory vision
- **[Honest questions](questions.md)** — provability, token economics, and what readiness really means
- **[Agent Readiness Model](agent-native/maturity-model.md)** — five maturity levels, ten technical pillars
- **[Agent-native engineering](agent-native/README.md)** — background agents, case studies, and the maturity framework

Horthy's "human leverage" concept inverts the traditional code review hierarchy: because errors compound upstream (a bad line of research cascades into thousands of bad lines of code; a bad line of plan cascades into hundreds), human attention should concentrate on validating research and reviewing plans rather than reviewing implementation code. This reinforces Klaassen's "plans are the new code" framing and the [seed quality](../principles/seed.md) principle — when agents write the code, the specification becomes the highest-leverage artifact.

His conclusion that the hard part is not the technology but "the team and workflow transformation" converges with what [Brockman](../SOURCES.md#retooling-for-agentic-development), [Levie](../SOURCES.md#structuring-work-agent-first), [Dabit](../SOURCES.md#the-cloud-agent-thesis), and [Pignanelli](../SOURCES.md#agent-native-engineering) have all observed from different angles: the real obstacle to agent-native engineering is organizational, not technical. Being "bullish on spec-first, agentic workflows" is not an endorsement of a tool but a bet on the culture/process/tech shift needed to transition to an AI-first coding world.

The Stanford study Horthy cites — that AI tools create rework rather than progress in brownfield codebases — provides a useful counterpoint: the agent-native environment the knowledge base advocates is precisely the response to this failure. Without structured context, agents are counterproductive in production codebases. With it, practitioners report shipping features estimated at days in hours.

Community commentary, counterarguments, and experience reports:

- [Simon Willison's review thread](../SOURCES.md#simon-willisons-review) — Willison's commentary and community responses
- [The Hacker News discussion](../SOURCES.md#simon-willisons-review) — Perspectives, counterarguments, and experience reports
- [Retooling for agentic development](../SOURCES.md#retooling-for-agentic-development) (Brockman) — OpenAI's internal approach: agents captains, AGENTS.md, agent-first codebases, quality standards, and infrastructure priorities
- [Structuring work agent-first](../SOURCES.md#structuring-work-agent-first) (Levie) — The implicit context gap between humans and agents, documentation as competitive advantage, agentic coding as leading indicator for all knowledge work
- [Agent-native engineering](../SOURCES.md#agent-native-engineering) (Pignanelli) — Three-level task classification, organizational transformation effects, the delegation-first architecture projection
- [The cloud agent thesis](../SOURCES.md#the-cloud-agent-thesis) (Dabit) — Cloud-vs-local agent taxonomy, playbooks as reusable expertise, organizational transformation prerequisites
- [Why we built our own background agent](../SOURCES.md#why-we-built-our-own-background-agent) (Ramp) — Inspect: sandboxed VMs, integrated tooling, 30% of merged PRs from agent work
- [Minions: Stripe's one-shot coding agents](../SOURCES.md#minions-stripes-one-shot-coding-agents) (Stripe) — 400+ MCP tools, isolated devboxes, 1,000+ PRs merged weekly
- [Advanced context engineering for coding agents](../SOURCES.md#advanced-context-engineering-for-coding-agents) (Horthy) — Human leverage framework: concentrate human attention on research and plan review, not code review; the hard part is team/workflow transformation, not model capability
- [Harness engineering](../SOURCES.md#harness-engineering) (OpenAI) — Zero-manual-code product development, repository-first agent legibility, capability doubling rate, TDD-for-agents
- [How Cognition uses Devin to build Devin](../SOURCES.md#how-cognition-uses-devin-to-build-devin) (Dabit) — Structured playbook patterns, event-driven agent triggering, session-to-session compounding, review agent implementation
