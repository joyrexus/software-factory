# Building In-House: Ramp and Stripe

*Two engineering organizations, convergent architectural requirements.*

## The Pattern

Two leading engineering organizations — Ramp and Stripe — independently built custom background coding agents, arriving at remarkably similar architectural requirements. Both concluded that off-the-shelf solutions were insufficient for their specific environments, and both converged on the same two foundational investments: complete development environments and rich context layers.

## Ramp's Inspect

[Ramp](../../SOURCES.md#why-we-built-our-own-background-agent) built Inspect, a background agent that writes code and verifies its work through integrated tools and context.

**Rationale:** "It only has to work on your code" — owning the tooling enables capabilities that commercial tools cannot match. A custom agent can be wired into the organization's specific telemetry, feature flags, and testing infrastructure in ways that a general-purpose tool cannot.

**Infrastructure:** Inspect runs in sandboxed VMs on Modal, with images rebuilt every 30 minutes to stay current with the codebase. Each session provides the complete local engineering toolkit — Vite, Postgres, Temporal — in an isolated environment that mirrors what a human engineer would have. Concurrent sessions run without resource constraints.

**Context layer:** Inspect is wired into Sentry (error tracking), Datadog (observability), LaunchDarkly (feature flags), Braintrust (quality assurance), GitHub (code and PRs), Slack (communication), and Buildkite (CI/CD). The agent does not just write code — it reviews telemetry, queries feature flags, and verifies its work through the same channels engineers use.

**Results:** Within months of deployment, approximately 30% of all pull requests merged to frontend and backend repositories were written by Inspect. This adoption was organic — no mandate, no quotas. Engineers adopted the tool because it worked. Multiplayer sessions support concurrent collaboration with attribution per contributor, enabling PMs and designers to participate directly in engineering work.

## Stripe's Minions

[Stripe](../../SOURCES.md#minions-stripes-one-shot-coding-agents) built Minions, one-shot coding agents responsible for more than 1,000 pull requests merged weekly with no human-written code.

**Rationale:** Stripe's codebase comprises hundreds of millions of lines of Ruby (with Sorbet typing, not Rails), built on homegrown libraries unfamiliar to any LLM's training data. The system moves over $1 trillion per year in payment volume. Off-the-shelf solutions cannot understand Stripe's conventions, and the stakes of incorrect code are too high for generic approaches.

**Infrastructure:** Minions run in isolated devboxes — the same machines engineers use — pre-warmed in 10 seconds, pre-loaded with Stripe code and services. Environments are isolated from production and the internet, enabling safe parallel execution. Built on a fork of Block's "goose" coding agent, Minions interleave agent loops with deterministic code operations (git, linting, testing) through customized orchestration.

**Context layer:** Toolshed, Stripe's central internal MCP server, hosts 400+ MCP tools providing access to internal docs, ticket details, build statuses, and Sourcegraph code intelligence. Minions read the same rule files as human tools (Cursor, Claude Code), with most rules conditionally applied per subdirectory. Before each run, the system deterministically executes relevant MCP tools to hydrate context — the agent starts with structured knowledge, not a blank slate.

**Verification:** A shift-left philosophy minimizes wasted compute. Local linting runs in under 5 seconds using heuristics to select relevant checks per git push. Selective test execution draws from a battery of 3+ million tests. A maximum of two CI rounds per run enforces discipline — if the agent cannot produce passing code in two attempts, the task needs human attention. Many test failures have autofixes that are applied automatically; only failures without autofixes are sent back to the agent for a single retry.

**Results:** More than 1,000 PRs merged weekly, produced entirely by Minions. Humans review the code but do not write it. Access through Slack, CLI, and web interfaces, with integration into internal documentation, feature flag, and ticketing UIs.

## Convergence

Despite building independently, in different tech stacks, at different scales, Ramp and Stripe converged on the same two requirements:

1. **Complete development environments** mirroring what engineers have — sandboxed VMs or devboxes with full toolchains, databases, test runners, and CI access. The agent does not operate in a stripped-down container; it has everything an engineer would have.

2. **Rich context layers** giving agents access to telemetry, codebase search, internal APIs, feature flags, and specifications — not just the code in front of them, but the surrounding organizational knowledge that makes the code meaningful.

These are the same two requirements that the [agent-native environment](../../principles/agent-native-environment.md) principle predicts. The principle states that agent output quality depends more on the engineering environment than on agent sophistication. Ramp and Stripe provide the evidence: both report that their infrastructure investments — not model upgrades — drove the step change in agent effectiveness.

---

**See also:**
- [Agent-Native Environment](../../principles/agent-native-environment.md) — the foundational principle these implementations validate
- [Cloud Agents and the Background Revolution](cloud-agents.md) — the conceptual framework for background agents
- [Agent Readiness Model](maturity-model.md) — maturity framework for evaluating readiness
- [Digital Twin Universe](../../techniques/digital-twin-universe.md) — the validation approach these environments enable
