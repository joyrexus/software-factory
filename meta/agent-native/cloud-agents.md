# Cloud Agents and the Background Revolution

*From pair programming to delegation.*

## The Shift

Background and cloud agents represent a fundamentally different interaction model from local coding tools. Where tools like Cursor and Claude Code operate as synchronous pair programmers — sharing the developer's environment, requiring real-time steering — background agents accept task descriptions and deliver completed pull requests asynchronously. As [Dabit](../../SOURCES.md#the-cloud-agent-thesis) frames it, "cloud agents are a different category from local agents, and the differences compound."

The shift is from collaboration to delegation. Engineers describe tasks, kick off sessions, and return to finished work. Ten tasks can run in parallel, yielding ten PRs. The agent has its own shell, IDE, and browser — it does not share the developer's environment or require their attention during execution.

The delegation model extends beyond human-initiated requests. REST APIs enable programmatic session triggering — crash logs can automatically spawn investigation PRs, bug reports trigger reproduction sessions, and failed deployments initiate analysis — shifting agents from tools humans invoke to services systems invoke ([Dabit](../../SOURCES.md#how-cognition-uses-devin-to-build-devin)).

## What Makes Cloud Agents Effective

The shift from collaboration to delegation only works if the agents receiving delegated work can actually complete it. For cloud agents to be effective, four capabilities matter:

- **They need to understand how work actually gets done across systems.** An agent that can write code but cannot navigate the surrounding context — ticket trackers, feature flags, deployment pipelines, monitoring dashboards — is operating with a fraction of the information a human engineer has. The rich context layers that [Ramp](../../SOURCES.md#why-we-built-our-own-background-agent) and [Stripe](../../SOURCES.md#minions-stripes-one-shot-coding-agents) built (Sentry, Datadog, LaunchDarkly, Sourcegraph, 400+ MCP tools) exist precisely because writing code is only part of doing engineering work. The goal is not merely connecting to individual tools but creating a semantic layer — a unified model of how information flows, where decisions happen, and what outcomes matter — that all agents can reference.

- **They need access to a computer and tools to plan, act, recover, and solve real-world problems.** Cloud agents are not chat interfaces generating text — they have their own shell, IDE, and browser, running in sandboxed environments with full development toolchains. This is the fundamental distinction from local coding tools: the agent can build, test, deploy, and debug without sharing the engineer's machine or requiring their attention. As agents operate, past interactions become useful context that improves future performance — a direct application of the [Compound Knowledge](../../principles/compound-knowledge.md) principle, where experience accumulates into durable, reusable understanding.

- **They need to understand what good looks like, so quality improves as the work changes.** AGENTS.md files, linter configurations, test suites, and CI pipelines serve as the agent's quality model — machine-verifiable definitions of correctness that the agent can check its own work against. As [Klaassen](../../SOURCES.md#compound-engineering) observes, agent output quality depends more on this engineering environment than on agent sophistication. When the environment encodes what "good" means, every improvement to standards compounds across all future agent work. But static standards are not enough — built-in evaluation and optimization loops make quality visible and improvable, clarifying what's working and what isn't so that agents progress from impressive demos to dependable teammates. Practitioners report enforcing test-first discipline on agents — tests must fail before implementation begins — so that agents prove they understand the problem before solving it ([Harness Engineering](../../SOURCES.md#harness-engineering)). When agents fail, practitioners report the diagnostic question is not "how do we make the agent try harder?" but "what capability is missing, and how do we make it both legible and enforceable?" — treating every failure as a signal about the environment, not the model ([Harness Engineering](../../SOURCES.md#harness-engineering)).

- **They need an identity, permissions, and boundaries teams can trust.** Agents operating autonomously in production codebases must have scoped permissions, audit trails, and governance frameworks that make their actions visible and controllable. [Dabit](../../SOURCES.md#the-cloud-agent-thesis) identifies governance frameworks as an explicit organizational transformation requirement — without them, scaling agent adoption means scaling unaccountable change.

The practitioner evidence reinforces that these capabilities, not model improvements, are the primary drivers of agent effectiveness:

- **Complete development environments** that mirror what engineers have — sandboxed VMs with full toolchains, databases, and CI access — enabling end-to-end verifiability without human intervention
- **Rich context layers** giving agents access to telemetry, codebase search, internal APIs, architectural docs, feature flags, and specifications — the organizational knowledge that makes code meaningful
- Organizations that invest in both report agents producing 30% or more of merged pull requests within months; those that deploy agents without this infrastructure discover that model capability alone is insufficient
- **End-to-end delegation without manually-written code** — at least one team reports building a complete product (~1 million lines across 1,500 PRs) over five months with zero manually-written code and a team of three engineers, demonstrating that full delegation is achievable when the repository is optimized for agent legibility and architectural invariants are mechanically enforced ([Harness Engineering](../../SOURCES.md#harness-engineering))

## Task Classification

The [General Intelligence Company](../../SOURCES.md#agent-native-engineering) introduces a three-level task classification that maps the delegation model to engineering reality:

1. **Simple (one-shottable)** — well-written prompts yield agent-coded solutions ready for merge. "Change the corner radius on this button" requires no feedback cycle.
2. **Manageable** — background agents solve these with feedback cycles, typically completed within a day. The agent produces a draft, receives comments, and iterates.
3. **Complex** — engineer-managed with synchronous coding agents, rarely exceeding one week. Only these tasks require the traditional model of a human actively coding.

The implication is that only complex tasks require synchronous human involvement. As environments mature — better test suites, richer context, more robust rulesets — tasks migrate downward in the classification. Problems that were once complex become manageable; manageable tasks become simple. The ratio of delegatable to hands-on work shifts continuously in favor of delegation.

## Organizational Effects

Both [Pignanelli](../../SOURCES.md#agent-native-engineering) and [Dabit](../../SOURCES.md#the-cloud-agent-thesis) describe organizational effects that follow from the delegation model:

**Engineers shift from coding to management and ideation.** When background agents handle the bulk of implementation, engineers spend more time decomposing problems, reviewing output, and designing systems. The General Intelligence Company reports that code review becomes a larger share of workflow than coding itself. Managers evolve into staff engineers; engineers function as both PM and technical lead for their projects.

**Non-engineers gain access to engineering workflows.** Cloud agents accessible through Slack or web interfaces allow PMs, designers, support staff, and operations teams to initiate engineering work without Git knowledge, local environments, or command-line experience. Dabit observes that this eliminates friction in fixing documentation, bugs, and small features. The scope extends beyond code: dedicated data analyst agents allow non-engineers to answer metrics and operational questions through natural language, moving agent-mediated access from engineering workflows to organizational analytics ([Dabit](../../SOURCES.md#how-cognition-uses-devin-to-build-devin)).

**Expertise becomes encoded and reusable.** Dabit's concept of *playbooks* — reusable workflows that anyone can trigger — transforms institutional knowledge from implicit to explicit. A senior engineer's approach to a migration can be captured as a playbook that junior engineers or even non-engineers can invoke against any repository. Effective playbooks include outcome definitions, required steps, postconditions, corrections to default agent behavior, and forbidden actions ([Dabit](../../SOURCES.md#how-cognition-uses-devin-to-build-devin)), mapping directly to the [specification discipline](../../techniques/specification-discipline.md) — each playbook is a specification in the self-check sense: the agent knows where to begin reading and what artifact proves completion. Plans themselves become first-class versioned artifacts — execution plans with progress and decision logs checked into the repository alongside active work, completed work, and known technical debt, allowing agents to operate without relying on external context ([Harness Engineering](../../SOURCES.md#harness-engineering)).

**Agent performance compounds through structured feedback.** Session analytics that identify recurring issues and suggest improved prompts create continuous improvement loops — insights from one session inform the next ([Dabit](../../SOURCES.md#how-cognition-uses-devin-to-build-devin)). This is a concrete implementation of the [Compound Knowledge](../../principles/compound-knowledge.md) principle applied to the delegation workflow itself.

**Review becomes the bottleneck.** More parallel agent sessions produce more PRs, creating a scaling challenge. Pignanelli recommends replacing the traditional two-engineer review requirement with code review bots, agentic pentesting, and AI SRE staging oversight. Dabit argues that the cloud agent pattern demands a complementary review agent as a corollary. The most mature implementations push further: agent-to-agent review loops where agents self-review locally, request additional agent reviews, respond to feedback, and iterate until all reviewers are satisfied — humans may review but are not required to ([Harness Engineering](../../SOURCES.md#harness-engineering)). High agent throughput also shifts the merge philosophy: minimal blocking merge gates, short-lived PRs, and test flakes addressed with follow-up runs rather than blocking progress — corrections become cheap when agents can produce fixes faster than humans can review them ([Harness Engineering](../../SOURCES.md#harness-engineering)).

## Infrastructure Requirements

Despite approaching the problem from different angles, both authors converge on the same infrastructure requirements for effective background agents:

- **Sandboxed execution environments** — isolated VMs or containers with full development toolchains, separated from production
- **Integration surfaces** — connections to Slack, Jira, GitHub, Sentry, Datadog, and other tools the organization already uses, so agents can gather context and report results through existing channels
- **Robust rulesets and comprehensive tests** — AGENTS.md files, linter configurations, and test suites that serve as the agent's quality guardrails, replacing the implicit standards a human engineer would know
- **Automated code review** — review agents or bots that can handle the volume of PRs that parallel agent sessions generate, preventing human reviewers from becoming the bottleneck
- **Agent-legible technology choices** — technologies described as "boring" tend to be easier for agents to model due to composability, API stability, and representation in training data; in some cases it is cheaper to reimplement functionality than to work around opaque upstream behavior from external libraries ([Harness Engineering](../../SOURCES.md#harness-engineering))

## Sandboxed Execution Environments

The delegation model depends on a piece of infrastructure that is easy to underestimate: the sandboxed execution environment — commonly called a *devbox*. Without isolated, fully-equipped environments, the entire background agent model collapses. An agent cannot build, test, and verify code on the engineer's machine without interfering with their work, and it cannot do so in a stripped-down container without the tools to verify its own output. Devboxes solve both problems simultaneously: they give each agent session its own complete, isolated development environment, enabling safe parallel execution at scale.

### Complete Environments, Not Minimal Containers

Both [Stripe](../../SOURCES.md#minions-stripes-one-shot-coding-agents) and [Ramp](../../SOURCES.md#why-we-built-our-own-background-agent) independently concluded that agents need the same toolchains engineers use — not minimal images optimized for size. Stripe's devboxes are the same machines engineers develop on, pre-loaded with source code and services. Ramp's VMs include the complete local engineering toolkit: Vite for frontend builds, Postgres for database access, Temporal for workflow orchestration. The pattern is convergent: agents that can write code but cannot run it, test it, or interact with its dependencies produce work that cannot be verified without human intervention — defeating the purpose of delegation.

This mirrors the broader [case-study convergence](case-studies.md) on development environments: the agent needs everything an engineer would have, because it is doing everything an engineer would do.

### Pre-Warming and Image Freshness

Cold-starting a full development environment takes minutes — an unacceptable latency when the goal is rapid parallel task execution. Both organizations solve this differently:

[Stripe](../../SOURCES.md#minions-stripes-coding-agents-part-2-infrastructure) maintains warm pools of pre-configured devboxes, targeting 10-second startup from request to ready environment. The "hot-and-ready" deployment model keeps environments pre-loaded with current source code and services, so agents start with a working codebase rather than cloning and building from scratch.

[Ramp](../../SOURCES.md#why-we-built-our-own-background-agent) rebuilds VM images every 30 minutes with filesystem snapshots, ensuring each new session launches against a recent codebase state. The tradeoff is explicit: more frequent rebuilds mean fresher code but higher infrastructure cost; less frequent rebuilds mean faster launches but staler starting points.

Both approaches address the same fundamental tension between startup speed and codebase freshness — a tension that becomes acute when agents may launch dozens of parallel sessions against a rapidly-evolving monorepo.

### Isolation as Enabler

Devbox isolation operates at three levels, each enabling a different capability:

**Production isolation.** Stripe's devboxes are isolated from both production systems and the internet. An agent with full permissions inside its sandbox cannot accidentally (or through prompt injection) reach production databases, external APIs, or sensitive services. The blast radius of any failure is contained to a disposable environment.

**Session isolation.** Each agent session runs in its own environment with no shared state between parallel runs. This is what makes the "ten tasks, ten PRs" model safe — sessions cannot interfere with each other, corrupt shared resources, or create race conditions. Parallelism becomes a scaling knob rather than a coordination problem.

**Engineer isolation.** The agent operates in its own environment, not the engineer's. No local file conflicts, no port collisions, no environment pollution. The engineer can continue working — or kick off additional agent sessions — without interference.

Together, these isolation properties enable a counterintuitive design choice: agents receive *full permissions* within their sandbox. Because the environment is disposable and the blast radius is contained, there is no need for fine-grained permission restrictions that would impede the agent's ability to build, test, and debug. Isolation replaces authorization as the primary safety mechanism.

### The Infrastructure Thesis

Stripe's [Part 2 infrastructure writeup](../../SOURCES.md#minions-stripes-coding-agents-part-2-infrastructure) articulates a thesis that extends beyond devboxes: "what's good for humans is good for agents." Investments in developer infrastructure — faster builds, better tooling, more reliable test suites, smoother onboarding — compound across both human and agent consumers. Pre-warmed devboxes that give agents 10-second startup also give engineers faster environment provisioning. Complete toolchains that enable agent self-verification also enable human debugging. The same rule files that guide agent behavior guide human tools like Cursor and Claude Code.

This double-dividend effect reinforces the [agent-native environment](../../principles/agent-native-environment.md) principle's core claim: the engineering environment matters more than agent sophistication. Organizations that invest in infrastructure quality create compounding returns — every improvement benefits an expanding population of both human and agent consumers.

## Future Projections

The General Intelligence Company projects that by end of 2026: IDEs as currently known will disappear, human code review will be eliminated in favor of reviewing product and infrastructure changes, all engineers will become product managers, and delegation-first architecture will become the default. Pignanelli's assertion — "every organization must become agent native, or they will cease to exist" — frames this not as an optimization but as an existential imperative. Quantitative benchmarks give this trajectory a concrete shape: as of mid-2025, agents demonstrate roughly two hours of continuous autonomous work at moderate confidence, with capability doubling approximately every seven months ([Harness Engineering](../../SOURCES.md#harness-engineering)). If the doubling rate holds, tasks currently classified as complex migrate mechanically toward manageable within predictable timeframes — making the task classification above a snapshot, not a stable taxonomy.

Whether or not this timeline proves accurate, the direction is consistent with [Klaassen's](../../SOURCES.md#compound-engineering) original insight: as environments mature, agent capability compounds, and the center of gravity shifts from implementation to design.

---

**See also:**
- [Agent-Native Environment](../../principles/agent-native-environment.md) — the foundational principle
- [Building In-House: Ramp and Stripe](case-studies.md) — concrete implementations of the background agent model
- [Agent Readiness Model](maturity-model.md) — maturity framework for evaluating environment readiness
- [Shift Work](../../techniques/shift-work.md) — interactive vs. non-interactive agent modes
- [Agent-Native Practices](practices/README.md) — concrete practices catalog: linters, refactors, review at scale, and more
- [Progressive Disclosure](../../techniques/progressive-disclosure.md) — layered context management for agent consumption
