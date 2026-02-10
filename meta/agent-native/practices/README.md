# Agent-Native Practices

This section catalogs concrete practices that make engineering environments agent-native, organized by scope. **Key practices** are foundational capabilities that most organizations pursuing agent-native engineering will need. **Targeted practices** are more specific techniques for particular contexts or organizational stages. Practices with available source material include summaries; others are stubs awaiting sources.

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

A key distinction: AGENTS.md explains the "why" and provides examples; linting enforces the "how" through machine-checkable validation. Organizations adopting only one category should start with grep-ability — consistent formatting makes everything else easier for agents to discover and navigate.

### Code Indexing and Search

Agents operate in context windows scoped to local directories. They generate code that is locally correct but cannot account for the wider system without cross-repository visibility. As agents produce code faster, the surface area of change increases — more changes across more repositories means more potential for subtle cross-service breakage and duplicated implementations. The paradox is that AI coding tools make organization-wide search *more* valuable, not less.

Codebase indexing provides three distinct capabilities: **deterministic enumeration** (structured queries returning every match across the entire corpus), **semantic code navigation** (go-to-definition and find-references that resolve symbols across repository boundaries), and **AI-assisted deep search** (natural language queries that follow call paths and synthesize findings across repositories).

This practice has a full treatment in [Codebase Indexing and Search](../../../techniques/codebase-indexing.md), which covers capabilities, the connection to the Agent Readiness Model, and implementation considerations.

### Background Agents

Background and cloud agents represent a fundamentally different interaction model — from synchronous pair programming to asynchronous delegation. Engineers describe tasks, kick off sessions, and return to finished work. The infrastructure requirements (sandboxed environments, rich context layers, automated review) and organizational effects (engineers shift from coding to management, non-engineers gain access to engineering workflows) are well documented.

This practice is treated in depth in [Cloud Agents and the Background Revolution](../cloud-agents.md) and [Building In-House: Ramp and Stripe](../case-studies.md). See those files for the full analysis rather than duplicating it here.

---

## Targeted Practices

### Agent-Driven Refactors

*Stub — awaiting source material.*

Using agents to execute large-scale refactors across codebases: API migrations, dependency upgrades, pattern replacements. The combination of linter rules (to define the target state), code indexing (to enumerate all instances), and background agents (to execute changes in parallel) suggests that refactoring may be one of the highest-leverage applications of agent-native infrastructure.

### Targeted Internal Tools

*Stub — awaiting source material.*

Building lightweight internal tools — CLIs, scripts, dashboards, one-off integrations — as a high-confidence starting point for agent-driven development. Internal tools have bounded scope, forgiving users, and low blast radius, making them natural candidates for agent delegation before graduating to production-facing work.

### Internal Plugin Marketplaces

Organizations create internal catalogs of agent capabilities — skills, tools, MCP servers, hooks — that engineers can discover and install without building from scratch. The [plugin marketplace model](../../SOURCES.md#internal-plugin-marketplaces) separates capability discovery from adoption: adding a marketplace gives access to browse its catalog, but engineers choose which plugins to install individually.

The model works at three scopes: **user** (personal, across all projects), **project** (shared with collaborators via repository settings), and **local** (personal, single repository). Team distribution is built in — administrators add marketplace configuration to project settings so team members are automatically prompted to install relevant plugins when they join a repository.

Capability categories span code intelligence (LSP integration for type-aware navigation), external integrations (pre-configured MCP servers for services like GitHub, Jira, Sentry, and Slack), and development workflows (commit automation, PR review, agent SDK tooling). The organizational value lies in encoding team-specific workflows, conventions, and tool integrations as distributable, versionable packages — transforming implicit engineering practices into explicit, shareable infrastructure.

---

**See also:**
- [Cloud Agents and the Background Revolution](../cloud-agents.md) — delegation model and organizational effects
- [Building In-House: Ramp and Stripe](../case-studies.md) — convergent architectural requirements at scale
- [Agent Readiness Model](../maturity-model.md) — maturity framework for evaluating environment readiness
- [Codebase Indexing and Search](../../../techniques/codebase-indexing.md) — full treatment of code indexing as agent infrastructure
- [Specification Discipline](../../../techniques/specification-discipline.md) — the self-check heuristic for specs
- [Agent-Native Environment](../../../principles/agent-native-environment.md) — the foundational principle
