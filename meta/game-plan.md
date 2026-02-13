# A Game Plan

*Start small, compound fast.*

> "This shift doesn't require a radical overhaul; small, targeted workflows compound quickly as coding agents become more capable and reliable. Teams that start with well-scoped tasks, invest in guardrails, and iteratively expand agent responsibility see meaningful gains in speed, consistency, and developer focus." — [Harness Engineering](../SOURCES.md#harness-engineering)

A phased approach synthesized from the [readiness model](agent-native/maturity-model.md) progression and practitioner reports. These are not prescriptive timelines — the focus is on sequencing and dependencies. Each phase creates the prerequisites for the next.

## Phase 1: Assess and prove

Before adopting agents, build the infrastructure that makes correctness demonstrable.

- **Score your codebase** against the [readiness model](agent-native/maturity-model.md) pillars. L3 (Standardized) across the ten pillars is the minimum bar for production autonomous operation — processes defined, documented, and enforced through automation. Most codebases will find L3 gaps.
- **Build proving loops** for your most critical paths. If you cannot prove the system works today, you cannot prove agents have not broken it tomorrow. Start with service-level invariants, golden signals, and synthetic journeys for the workflows that matter most.
- **Invest in validation infrastructure.** CI/CD that runs automatically, test suites with clear pass/fail signals, observability that makes outcomes measurable. The evidence suggests these investments pay for themselves independent of agent adoption — they are good engineering practice that also happens to be an agent prerequisite.

## Phase 2: Prepare the environment

Create the conditions that make agents effective.

- **Create AGENTS.md** and structured documentation that agents can consume — how to build, test, and lint; how the codebase is organized; what evidence must accompany a pull request.
- **Make organizational knowledge machine-accessible.** Expose internal tools via CLI or MCP. Wire in context sources: telemetry, codebase search, internal APIs, feature flags, architectural documentation.
- **Set up sandboxed development environments** that mirror what engineers use — complete toolchains, databases, test runners, and CI access. The agent should operate in the same environment as a human engineer, not a stripped-down container.
- **Designate "agents captains"** — [Brockman's](../SOURCES.md#retooling-for-agentic-development) term for engineers who drive adoption within their teams, trying tools first, creating skills, and establishing conventions.
- **Identify which practices to adopt** using the [practices catalog](agent-native/practices/README.md) — linters as architectural guardrails, codebase indexing for cross-repository visibility, and internal plugin marketplaces for reusable agent capabilities. These practices are organized by scope, from individual repository to organization-wide, so teams can start with what fits their current maturity level.

## Phase 3: Start delegating

Begin with tasks where the environment provides clear feedback.

- **Start with simple, one-shottable tasks** — well-scoped, clear specifications, strong test coverage, unambiguous done criteria. [Pignanelli's](../SOURCES.md#agent-native-engineering) task classification provides the framework: simple (one-shottable), manageable (background agents with feedback cycles), complex (engineer-managed with synchronous agents). Start at the bottom.
- **Measure.** What percentage of PRs come from agents? What is the rework rate? How does cycle time compare? These metrics inform whether the environment is ready for more complex delegation.
- **Let adoption be organic.** [Ramp](../SOURCES.md#why-we-built-our-own-background-agent) reached approximately 30% agent-written PRs without mandating usage — engineers adopted the tool because it worked. Forced adoption before the environment is ready creates frustration and undermines confidence in the approach.

## Phase 4: Shift the culture

As agent adoption matures, the organizational model changes.

- **Move review upstream.** Review specifications and plans, not implementation code. This is where human leverage is highest and where errors compound most.
- **Encode expertise as playbooks** and reusable workflows. Each senior engineer's knowledge becomes a durable organizational asset rather than a personal capability.
- **Expand access.** When agents can be invoked through Slack, Jira, or web interfaces, non-engineers — PMs, designers, support — can initiate engineering work directly. Both [Ramp](../SOURCES.md#why-we-built-our-own-background-agent) and [Dabit](../SOURCES.md#the-cloud-agent-thesis) report this as a significant organizational multiplier.
- **Compound.** Each cycle's learnings feed the next — specifications improve, context layers deepen, proving loops tighten. This is [Klaassen's](../SOURCES.md#compound-engineering) compound step: the engineering environment should get better with every unit of work, not just produce output.

---

**See also:**
- [Takeaways for engineering leaders](takeaways.md) — the evidence and organizational shifts that motivate this game plan
- [Agent Readiness Model](agent-native/maturity-model.md) — maturity levels and technical pillars for scoring readiness
- [Agent-Native Practices](agent-native/practices/README.md) — concrete practices catalog: linters, code indexing, plugin marketplaces
- [Honest questions](questions.md) — provability, token economics, and what readiness really means
- [Agent-native engineering](agent-native/README.md) — background agents, case studies, and the maturity framework
