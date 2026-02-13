# Agent-Native Practices

This section catalogs concrete practices that make engineering environments agent-native, organized by scope. **Key practices** are foundational capabilities that most organizations pursuing agent-native engineering will need. **Targeted practices** are more specific techniques for particular contexts or organizational stages.

---

## Key Practices

### Linters as Architectural Guardrails

As development shifts from engineers writing code with AI assistance to engineers orchestrating agents that build software, traditional guardrails — code review, conventions, tribal knowledge — become insufficient. Agents need machine-verifiable rules, not suggestions. Linters fill this gap by transforming human intent into machine-enforced guarantees that agents can verify and self-correct against.

[Factory.ai](../../SOURCES.md#using-linters-to-direct-agents) identifies seven categories of lint rules for agent-native codebases:

1. **Grep-ability** — consistent formatting (named exports, explicit DTOs) that makes code searchable by pattern
2. **Glob-ability** — predictable file organization enabling reliable agent placement and refactoring
3. **Architectural boundaries** — cross-layer import prevention, domain-specific allowlists, module boundary enforcement
4. **Security and privacy** — plain text secret blocking, mandatory input validation, eval prevention
5. **Testability and coverage** — colocated unit tests, network call restrictions, consistent async patterns
6. **Observability** — structured logging, error metadata, consistent telemetry naming conventions
7. **Documentation signals** — module-level docstrings for public APIs, architectural decision record links for exceptions

The lint development cycle is a repeatable five-step process: **observe** anti-patterns in reviews, logs, or metrics → **codify** the rule with severity, autofix, and test coverage → **surface** violations across the repository → **remediate** at scale using parallel agents applying codemods → **enforce** on pre-commit, CI, PR bots, and agent toolchains. Every lesson becomes an executable constraint that compounds over time.

When lint rules are custom, the error messages themselves become a teaching mechanism. Writing error messages that inject remediation instructions into agent context — explaining not just what is wrong but how to fix it — turns every lint violation into an inline coaching moment. In a human-first workflow these detailed messages might feel pedantic; with agents, they become multipliers that apply everywhere at once ([Harness Engineering](../../SOURCES.md#harness-engineering)).

A key distinction: AGENTS.md explains the "why" and provides examples; linting enforces the "how" through machine-checkable validation. Organizations adopting only one category should start with grep-ability — consistent formatting makes everything else easier for agents to discover and navigate.

### Code Indexing and Search

Agents operate in context windows scoped to local directories. They generate code that is locally correct but cannot account for the wider system without cross-repository visibility. As agents produce code faster, the surface area of change increases — more changes across more repositories means more potential for subtle cross-service breakage and duplicated implementations. The paradox is that AI coding tools make organization-wide search *more* valuable, not less.

Codebase indexing provides three distinct capabilities: **deterministic enumeration** (structured queries returning every match across the entire corpus), **semantic code navigation** (go-to-definition and find-references that resolve symbols across repository boundaries), and **AI-assisted deep search** (natural language queries that follow call paths and synthesize findings across repositories).

This practice has a full treatment in [Codebase Indexing and Search](../../../techniques/codebase-indexing.md), which covers capabilities, the connection to the Agent Readiness Model, and implementation considerations.

### Background Agents

Background and cloud agents represent a fundamentally different interaction model — from synchronous pair programming to asynchronous delegation. Engineers describe tasks, kick off sessions, and return to finished work. The infrastructure requirements (sandboxed environments, rich context layers, automated review) and organizational effects (engineers shift from coding to management, non-engineers gain access to engineering workflows) are well documented.

This practice is treated in depth in [Cloud Agents and the Background Revolution](../cloud-agents.md) and [Building In-House: Ramp and Stripe](../case-studies.md). See those files for the full analysis rather than duplicating it here.

### Application Legibility

Code legibility — grep-ability, glob-ability, consistent naming — addresses the static codebase. Application legibility addresses runtime behavior: making the running application observable and navigable by agents, not just the source code.

Agents that can only read code are operating with a fraction of the information they need. A bug report that says "the dashboard shows stale data after login" requires an agent to boot the app, reproduce the behavior, inspect network requests, and check what the UI actually renders. Making the app bootable, observable, and drivable within the agent's sandbox closes this gap.

The concrete implementation involves several layers. **Per-worktree app instances** give each agent task an isolated running application — no shared state, no port collisions, no interference between parallel tasks. **Browser automation** via Chrome DevTools Protocol enables DOM snapshots, screenshots, and navigation, allowing agents to reproduce bugs, validate fixes, and reason about UI behavior directly rather than inferring it from code. **Ephemeral local observability** — logs via LogQL, metrics via PromQL — scoped to the agent's worktree and torn down on task completion, provides runtime insight without polluting shared monitoring infrastructure.

When agents can query metrics directly, performance becomes specifiable: prompts like "ensure startup completes in under 800ms" become tractable because the agent can measure the result rather than guessing. Application legibility transforms vague quality concerns into verifiable constraints ([Harness Engineering](../../SOURCES.md#harness-engineering)).

See also: [Agent-Native Environment](../../../principles/agent-native-environment.md) — the foundational principle that application legibility implements.

### Entropy Management

Agents replicate patterns that already exist — including suboptimal ones. Without systematic intervention, drift is inevitable. One team reports spending 20% of engineer time (every Friday) manually cleaning up "AI slop" before automating the process.

The response is continuous automated cleanup at three levels. First, **golden principles** — opinionated, mechanical rules encoded directly in the repository that keep the codebase legible for future agent runs. Examples include preferring shared utility packages over hand-rolled helpers, validating boundaries rather than probing data, and enforcing consistent naming across layers. These rules serve double duty: they constrain current agent output and preserve the patterns future agents will replicate.

Second, **recurring background agent tasks** that scan for deviations, update quality grades, and open targeted refactoring PRs. Most of these PRs are reviewable in under a minute and eligible for automerge. The garbage collection metaphor applies: pay down technical debt continuously in small increments rather than letting it compound into periodic, painful cleanup sprints.

Third, a **human taste feedback loop**: review comments, refactoring PRs, and user-facing bugs are captured as documentation updates or encoded directly into tooling. When documentation falls short, the rule is promoted into code — a lint rule, a structural test, or an architectural constraint that prevents recurrence mechanically rather than through instruction alone ([Harness Engineering](../../SOURCES.md#harness-engineering)).

See also: [Agent-Driven Refactors](#agent-driven-refactors) covers the refactoring workflow itself; entropy management is the continuous scheduling layer that feeds it. [Compound Knowledge](../../../principles/compound-knowledge.md) — each cleanup cycle improves the substrate for future agent work.

---

## Targeted Practices

### Agent-Driven Refactors

Large-scale refactors — API migrations, dependency upgrades, pattern replacements — may be one of the highest-leverage applications of agent-native infrastructure because they compose three key practices into a single workflow: linter rules define the target state, code indexing enumerates every instance that needs to change, and background agents execute the transformations in parallel.

The workflow follows the lint development cycle's final two steps. [Factory.ai](../../SOURCES.md#using-linters-to-direct-agents) describes the cycle as observe → codify → surface → **remediate** at scale using parallel agents applying codemods → **enforce** on CI and agent toolchains. The remediation step is the refactor itself — agents apply the codemod to every surfaced violation simultaneously. The lint rule serves as both the specification for the change and the verification that the change was applied correctly.

Exhaustive enumeration is the critical prerequisite. As [Sourcegraph](../../SOURCES.md#code-search-at-scale) argues, finding a representative sample of affected code is insufficient for refactoring — every instance across every repository must be identified. A refactor that misses occurrences creates inconsistency that compounds over time, and agents operating in bounded context windows cannot discover instances outside their scope without cross-repository search infrastructure.

The infrastructure that makes this safe at scale is well-documented in practitioner reports. [Stripe](../../SOURCES.md#minions-stripes-one-shot-coding-agents) applies shift-left verification — local linting in under five seconds, selective test execution from 3+ million tests, and a maximum of two CI rounds per run — to every agent-generated change. [Ramp](../../SOURCES.md#why-we-built-our-own-background-agent) provides sandboxed VMs rebuilt every 30 minutes with integrated telemetry from Sentry, Datadog, and Buildkite, giving agents the feedback loops needed to verify refactor correctness. [Stripe's](../../SOURCES.md#minions-stripes-one-shot-coding-agents) deterministic context hydration ensures each agent session starts with structured knowledge of conventions and dependencies rather than discovering them at runtime.

The pattern is: **define** the target state as a machine-verifiable rule → **enumerate** all instances with cross-repository indexing → **execute** changes in parallel via background agents → **verify** each change against the rule that triggered it. Organizations already practicing linter-driven development and codebase indexing have the prerequisites in place; agent-driven refactoring is the composed workflow that those practices enable.

### Targeted Internal Tools

Internal tools — CLIs, scripts, dashboards, one-off integrations — are high-confidence starting points for agent-driven development because they sit at the intersection of several converging recommendations from independent sources.

[Pignanelli's](../../SOURCES.md#agent-native-engineering) three-level task classification provides the framework: simple (one-shottable), manageable (background agents with feedback cycles), complex (engineer-managed with synchronous agents). Internal tools sit squarely in the "simple" category — bounded scope, clear done criteria, and strong test coverage make them natural candidates for one-shot agent delegation. Starting at the bottom of the classification builds confidence and reveals environment gaps before the stakes are high.

[Brockman](../../SOURCES.md#retooling-for-agentic-development) arrives at the same conclusion from the adoption side: "try the tools" first, designate "agents captains" who drive adoption within their teams, and "inventory and expose internal tools via CLI or MCP." The recommendation to start with internal tooling is explicit — it is the first concrete action item in OpenAI's own retooling playbook.

[Klaassen's](../../SOURCES.md#compound-engineering) central insight — that agent output quality depends more on the engineering environment than on agent sophistication — explains why internal tools are the right starting point. Internal-facing work provides a controlled environment where the risk of environment immaturity is low: users are forgiving colleagues rather than paying customers, the blast radius of a defect is bounded, and the scope is narrow enough that specifications can be precise. These conditions minimize the gap between environment readiness and agent requirements.

[Dabit](../../SOURCES.md#the-cloud-agent-thesis) extends the pattern through playbooks — reusable workflows encoding expertise that anyone can trigger. Internal tools are natural playbook candidates because the workflow is self-contained: a non-engineer can invoke an agent through Slack or a web interface to build a dashboard or integration without Git knowledge or a local environment. This opens engineering capacity to the broader organization while keeping risk low.

The strategic logic: internal tools let teams build agent-delegation muscle — establishing conventions for specifications, review, and verification — in an environment where mistakes are cheap and feedback is fast. As those conventions mature, they become the foundation for delegating higher-stakes, production-facing work.

### Automated Review at Scale

As agent adoption increases, review becomes the bottleneck. More parallel agent sessions produce more pull requests, and without review infrastructure that scales proportionally, organizations create a growing backlog of unreviewed changes — the opposite of the velocity improvement they sought.

The evidence for this is convergent across multiple sources. [Pignanelli](../../SOURCES.md#agent-native-engineering) reports that code review already constitutes a larger share of engineering workflow than coding in agent-native organizations, and recommends replacing the traditional two-engineer review requirement with code review bots, agentic pentesting, and AI SRE staging oversight. [Dabit](../../SOURCES.md#the-cloud-agent-thesis) frames it as a corollary: the system that generates PRs at scale must also help triage them at scale — a complementary review agent is an architectural requirement of the cloud agent model, not an optimization.

Practitioner implementations validate this. [Stripe](../../SOURCES.md#minions-stripes-one-shot-coding-agents) applies a shift-left verification philosophy: local linting runs in under five seconds using heuristics to select relevant checks per git push, selective test execution draws from 3+ million tests, and a maximum of two CI rounds per run enforces discipline. Many test failures have autofixes applied automatically; only failures without autofixes are sent back to the agent for a single retry. The result is that most quality issues are caught before a human reviewer ever sees the PR. [Ramp](../../SOURCES.md#why-we-built-our-own-background-agent) invested in automated review tooling to keep pace with approximately 30% of merged PRs coming from their agent — the infrastructure was a prerequisite for organic adoption at that volume.

The practice has several dimensions:

- **Deterministic pre-checks** — linting, type checking, and formatting that run automatically and reject non-compliant changes before review begins
- **Selective test execution** — infrastructure to identify and run only the tests relevant to each change, enabling fast feedback even against large test suites
- **Review agents** — agents that triage incoming PRs, flag patterns that need human attention, and approve changes that meet established criteria
- **Graduated autonomy** — low-risk changes (style fixes, documentation, dependency patches) receive lighter review than high-risk changes (security boundaries, payment flows, data migrations)

This practice is a prerequisite, not an afterthought. Organizations planning to scale agent output should invest in review infrastructure concurrently with agent adoption — the [takeaways](../../takeaways.md) identify this as one of four organizational shifts that recur across the sources. See [Cloud Agents and the Background Revolution](../cloud-agents.md) for the broader context on how review fits into the delegation model.

### Internal Plugin Marketplaces

Organizations create internal catalogs of agent capabilities — skills, tools, MCP servers, hooks — that engineers can discover and install without building from scratch. The [plugin marketplace model](../../SOURCES.md#internal-plugin-marketplaces) separates capability discovery from adoption: adding a marketplace gives access to browse its catalog, but engineers choose which plugins to install individually.

The model works at three scopes: **user** (personal, across all projects), **project** (shared with collaborators via repository settings), and **local** (personal, single repository). Team distribution is built in — administrators add marketplace configuration to project settings so team members are automatically prompted to install relevant plugins when they join a repository.

Capability categories span code intelligence (LSP integration for type-aware navigation), external integrations (pre-configured MCP servers for services like GitHub, Jira, Sentry, and Slack), and development workflows (commit automation, PR review, agent SDK tooling). The organizational value lies in encoding team-specific workflows, conventions, and tool integrations as distributable, versionable packages — transforming implicit engineering practices into explicit, shareable infrastructure.

---

**See also:**
- [Cloud Agents and the Background Revolution](../cloud-agents.md) — delegation model and organizational effects
- [Building In-House: Ramp and Stripe](../case-studies.md) — convergent architectural requirements at scale
- [Agent Readiness Model](../maturity-model.md) — maturity framework for evaluating environment readiness
- [Takeaways for Engineering Leaders](../../takeaways.md) — phased adoption plan and organizational shifts
- [Codebase Indexing and Search](../../../techniques/codebase-indexing.md) — full treatment of code indexing as agent infrastructure
- [Specification Discipline](../../../techniques/specification-discipline.md) — the self-check heuristic for specs
- [Agent-Native Environment](../../../principles/agent-native-environment.md) — the foundational principle
- [Progressive Disclosure](../../../techniques/progressive-disclosure.md) — layered context management for agent consumption
