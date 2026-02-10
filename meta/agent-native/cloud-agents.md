# Cloud Agents and the Background Revolution

*From pair programming to delegation.*

## The Shift

Background and cloud agents represent a fundamentally different interaction model from local coding tools. Where tools like Cursor and Claude Code operate as synchronous pair programmers — sharing the developer's environment, requiring real-time steering — background agents accept task descriptions and deliver completed pull requests asynchronously. As [Dabit](../../SOURCES.md#the-cloud-agent-thesis) frames it, "cloud agents are a different category from local agents, and the differences compound."

The shift is from collaboration to delegation. Engineers describe tasks, kick off sessions, and return to finished work. Ten tasks can run in parallel, yielding ten PRs. The agent has its own shell, IDE, and browser — it does not share the developer's environment or require their attention during execution.

## Task Classification

The [General Intelligence Company](../../SOURCES.md#agent-native-engineering) introduces a three-level task classification that maps the delegation model to engineering reality:

1. **Simple (one-shottable)** — well-written prompts yield agent-coded solutions ready for merge. "Change the corner radius on this button" requires no feedback cycle.
2. **Manageable** — background agents solve these with feedback cycles, typically completed within a day. The agent produces a draft, receives comments, and iterates.
3. **Complex** — engineer-managed with synchronous coding agents, rarely exceeding one week. Only these tasks require the traditional model of a human actively coding.

The implication is that only complex tasks require synchronous human involvement. As environments mature — better test suites, richer context, more robust rulesets — tasks migrate downward in the classification. Problems that were once complex become manageable; manageable tasks become simple. The ratio of delegatable to hands-on work shifts continuously in favor of delegation.

## Organizational Effects

Both [Pignanelli](../../SOURCES.md#agent-native-engineering) and [Dabit](../../SOURCES.md#the-cloud-agent-thesis) describe organizational effects that follow from the delegation model:

**Engineers shift from coding to management and ideation.** When background agents handle the bulk of implementation, engineers spend more time decomposing problems, reviewing output, and designing systems. The General Intelligence Company reports that code review becomes a larger share of workflow than coding itself. Managers evolve into staff engineers; engineers function as both PM and technical lead for their projects.

**Non-engineers gain access to engineering workflows.** Cloud agents accessible through Slack or web interfaces allow PMs, designers, support staff, and operations teams to initiate engineering work without Git knowledge, local environments, or command-line experience. Dabit observes that this eliminates friction in fixing documentation, bugs, and small features.

**Expertise becomes encoded and reusable.** Dabit's concept of *playbooks* — reusable workflows that anyone can trigger — transforms institutional knowledge from implicit to explicit. A senior engineer's approach to a migration can be captured as a playbook that junior engineers or even non-engineers can invoke against any repository.

**Review becomes the bottleneck.** More parallel agent sessions produce more PRs, creating a scaling challenge. Pignanelli recommends replacing the traditional two-engineer review requirement with code review bots, agentic pentesting, and AI SRE staging oversight. Dabit argues that the cloud agent pattern demands a complementary review agent as a corollary.

## Infrastructure Requirements

Despite approaching the problem from different angles, both authors converge on the same infrastructure requirements for effective background agents:

- **Sandboxed execution environments** — isolated VMs or containers with full development toolchains, separated from production
- **Integration surfaces** — connections to Slack, Jira, GitHub, Sentry, Datadog, and other tools the organization already uses, so agents can gather context and report results through existing channels
- **Robust rulesets and comprehensive tests** — AGENTS.md files, linter configurations, and test suites that serve as the agent's quality guardrails, replacing the implicit standards a human engineer would know
- **Automated code review** — review agents or bots that can handle the volume of PRs that parallel agent sessions generate, preventing human reviewers from becoming the bottleneck

## Future Projections

The General Intelligence Company projects that by end of 2026: IDEs as currently known will disappear, human code review will be eliminated in favor of reviewing product and infrastructure changes, all engineers will become product managers, and delegation-first architecture will become the default. Pignanelli's assertion — "every organization must become agent native, or they will cease to exist" — frames this not as an optimization but as an existential imperative.

Whether or not this timeline proves accurate, the direction is consistent with [Klaassen's](../../SOURCES.md#compound-engineering) original insight: as environments mature, agent capability compounds, and the center of gravity shifts from implementation to design.

---

**See also:**
- [Agent-Native Environment](../../principles/agent-native-environment.md) — the foundational principle
- [Building In-House: Ramp and Stripe](case-studies.md) — concrete implementations of the background agent model
- [Agent Readiness Model](maturity-model.md) — maturity framework for evaluating environment readiness
- [Shift Work](../../techniques/shift-work.md) — interactive vs. non-interactive agent modes
