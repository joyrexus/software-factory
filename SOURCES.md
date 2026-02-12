# Sources

Annotated bibliography of the primary sources synthesized in this knowledge base.

---

## StrongDM Software Factory

**URL:** [factory.strongdm.ai](https://factory.strongdm.ai/)

StrongDM's Software Factory is the most radical implementation of the agentic development paradigm: "code must not be written by humans; code must not be reviewed by humans." The site describes principles (deliberate naivete, tokens as fuel), named techniques (digital twin universe, gene transfusion, filesystem-as-memory, semport, pyramid summaries, shift work), and three products — the Attractor pipeline orchestrator, CXDB context store, and StrongDM ID agent identity system. The key innovation is scenario-based validation using probabilistic satisfaction metrics: measuring what fraction of observed trajectories through end-to-end user stories likely satisfy the user. This knowledge base's [Attractor deep-dive](components/attractor/README.md) is built primarily from StrongDM's open-source specifications.

**Key contributions:** Named principles and techniques vocabulary, digital twin universe concept, scenario-based validation, the Attractor pipeline spec.

---

## Compound Engineering

**URL:** [every.to/guides/compound-engineering](https://every.to/guides/compound-engineering)
**Author:** Kieran Klaassen

Klaassen's framework identifies four steps for effective agentic development: plan, work, review, and compound. The first three are variations on known practices; the fourth — having agents summarize learnings into structured formats consulted by future planning phases — is the key innovation. Klaassen's central insight is that agent output quality depends more on engineering environment quality (codebase maturity, test coverage, CI/CD systems) than on agent sophistication. The guide describes five adoption stages (from manual development through parallel cloud execution) and reframes the engineer's role: "plans are the new code." Each unit of engineering work should make subsequent units easier, not harder.

**Key contributions:** The "environment matters more than agent" insight, the Compound step as explicit practice, five-stage adoption model, the plan-first engineering paradigm.

---

## Agent-Native Development

**URL:** [factory.ai/news/build-with-agents](https://www.factory.ai/news/build-with-agents)
**Author:** Factory.ai

Factory.ai's framework starts from the observation that standard development practices are human-centric by design. Agents need different things: precise specifications, small scopes, and objective verification criteria. Their self-check heuristic — "can you identify where the agent begins reading and what artifact proves completion?" — is a practical test for specification quality. The framework includes a risk-tiered automation model (low/medium/high risk actions with graduated autonomy), an explore-plan-code-verify inner loop, and recovery patterns for when agents drift. AGENTS.md appears here too, answering three questions: how to build/test/lint, how the codebase is organized, and what evidence must accompany a pull request.

**Key contributions:** The self-check heuristic for specifications, risk-tiered automation framework, agent-native environment design principles, recovery patterns.

---

## Using Linters to Direct Agents

**URL:** [factory.ai/news/using-linters-to-direct-agents](https://www.factory.ai/news/using-linters-to-direct-agents)
**Author:** Factory.ai

The argument for shifting linters from style enforcement to machine-enforced architectural guardrails that agents can verify and self-correct against. As development moves from "developers writing code with AI" to "developers orchestrating agents that build software," traditional guardrails — code review, conventions, tribal knowledge — become insufficient. Agents need machine-verifiable rules, not suggestions. The article identifies seven categories of lint rules for agent-native codebases: grep-ability (consistent formatting for searchability), glob-ability (predictable file organization), architectural boundaries (cross-layer import prevention), security and privacy (secret blocking, input validation), testability and coverage (colocated tests, network call restrictions), observability (structured logging, telemetry naming), and documentation signals (module-level docstrings, ADR links). A repeatable five-step lint development cycle — observe anti-patterns, codify the rule, surface violations, remediate at scale using parallel agents, enforce on CI and agent toolchains — turns every lesson into an executable constraint that compounds over time.

**Key contributions:** Seven lint rule categories for agent-native codebases, the lint development cycle (observe → codify → surface → remediate → enforce), the distinction between AGENTS.md (explains "why") and linting (enforces "how"), grep-ability as the highest-priority starting category.

---

## Simon Willison's Review

**URL:** [simonwillison.net/2026/Feb/7/software-factory](https://simonwillison.net/2026/Feb/7/software-factory/)
**Author:** Simon Willison

Willison's assessment of the Software Factory concept provides essential external perspective. He frames the concept as a "dark factory" and identifies the central challenge: "How can you prove that software you are producing works if both the implementation and the tests are being written for you by coding agents?" He calls out the digital twin universe as the most impressive element and raises pointed questions about the $1,000/day token cost model. His overall verdict — "a glimpse of one potential future" — balances genuine interest in the validation patterns with skepticism about economics and competitive dynamics. The review thread and Hacker News discussion generated significant practitioner commentary that extends and challenges the source material.

**Key contributions:** The "dark factory" framing, the central validation question, cost model skepticism, independent critical assessment.

---

## Agent Readiness Model (Blog Post)

**URL:** [factory.ai/news/agent-readiness](https://www.factory.ai/news/agent-readiness)
**Author:** Factory.ai

Factory.ai's introduction of the Agent Readiness Model — eight technical pillars and five maturity levels for evaluating codebase readiness for autonomous development. The conceptual framework shifts the question from "how good is the agent?" to "how ready is the codebase?" and provides a structured scoring methodology for measuring and improving readiness across an organization.

**Key contributions:** The readiness-over-capability framing, maturity level progression model, pillar-based scoring methodology.

---

## Agent Readiness Model (Documentation)

**URL:** [docs.factory.ai/web/agent-readiness/overview](https://docs.factory.ai/web/agent-readiness/overview)
**Author:** Factory.ai

Detailed reference for the Agent Readiness framework — level definitions, ten technical pillars, scoring methodology, and progression criteria. Expands the blog post's conceptual overview into a complete specification with per-pillar criteria at each maturity level and the 80% gating rule for level advancement.

**Key contributions:** Full pillar definitions and per-level criteria, gating rule specification, ten-pillar taxonomy (expanded from original nine).

---

## Making Codebases Agent Ready (Talk)

**URL:** [youtube.com/watch?v=ShuJ_CN6zr4](https://www.youtube.com/watch?v=ShuJ_CN6zr4)
**Author:** Eno Reyes, Factory AI

Conference talk presenting the Agent Readiness Model — the conceptual case for measuring and improving codebase readiness for autonomous agent operation. Provides practical examples of how organizations have applied the maturity model and discusses the gap between agent capability and environment readiness.

**Key contributions:** Practitioner-oriented presentation of the readiness model, real-world application examples, the environment-readiness gap thesis.

---

## OpenClaw Memory System

**URL:** [docs.openclaw.ai/concepts/memory](https://docs.openclaw.ai/concepts/memory)

OpenClaw's agent memory architecture uses markdown files as the source of truth — daily session logs and a long-term `MEMORY.md` — with hybrid retrieval combining vector similarity and BM25 full-text search. The system implements an agentic flush mechanism where the agent itself decides when to persist working memory to disk. The "files are the source of truth" principle aligns with the [filesystem-as-memory](techniques/filesystem-as-memory.md) technique while extending it with retrieval infrastructure.

**Key contributions:** File-first memory model, hybrid retrieval combining vector similarity with full-text search, the "files are the source of truth" principle.

---

## The RAG Obituary

**URL:** [nicolasbustamante.com/p/the-rag-obituary-killed-by-agents](https://www.nicolasbustamante.com/p/the-rag-obituary-killed-by-agents)
**Author:** Nicolas Bustamante

The case that RAG is being superseded by agentic search — agents with tool access navigate relationships directly rather than retrieving embedding-matched fragments. The core critique is that RAG's chunking and embedding pipeline introduces compounding errors: lossy chunking fragments context, embedding collapse loses relational structure, and retrieval returns fragments without the connections that made them meaningful. The "agents investigate, not retrieve" framing has direct implications for [context store](components/context-store/README.md) design.

**Key contributions:** Chunking fragmentation critique, the "agents investigate, not retrieve" framing, architectural implications for context systems.

---

## Lessons from Building AI Agents

**URL:** [nicolasbustamante.com/p/lessons-from-building-ai-agents-for](https://www.nicolasbustamante.com/p/lessons-from-building-ai-agents-for)
**Author:** Nicolas Bustamante

Observations from production agent systems on S3-first architecture — using S3 as the authoritative store with databases serving as query indexes — and the value of filesystem/bash as agent-native primitives. The pattern parallels CXDB's blob CAS architecture: content-addressed immutable storage with metadata indexed separately. The filesystem tools observation reinforces the [agent-native environment](principles/agent-native-environment.md) principle.

**Key contributions:** S3 as authoritative store with database as query index pattern, filesystem tools as agent-native abstractions.

---

## CXDB

**URL:** [github.com/strongdm/cxdb](https://github.com/strongdm/cxdb)

StrongDM's open-source AI context store — the reference implementation of the [context store](components/context-store/README.md) concept described in this knowledge base. Implements a Turn DAG with blob CAS architecture: immutable turn nodes with parent references, BLAKE3 content-addressed blob storage with Zstd compression, branch-from-any-turn forking with O(1) cost, and a type registry with forward-compatible schema evolution. Includes a built-in React UI for visual debugging.

**Key contributions:** Turn DAG data model, BLAKE3 content-addressed blob storage, O(1) context forking, type registry with forward-compatible schema evolution.

---

## Retooling for Agentic Development

**URL:** [x.com/gdb/status/2019566641491963946](https://x.com/gdb/status/2019566641491963946)
**Author:** Greg Brockman

Brockman's post describes OpenAI's internal approach to retooling engineering teams for agentic software development. He reports a "step function improvement" since December 2025, with engineers shifting from using Codex for unit tests to having it write essentially all code plus operations and debugging. The post outlines six recommendations: try the tools and designate "agents captains," create skills and AGENTS.md files, inventory and expose internal tools via CLI or MCP, structure codebases to be agent-first, maintain code quality standards ("say no to slop"), and build basic infrastructure including observability and agent trajectory tracking. The framing — "not just a technical but also a deep cultural change" — reinforces that adoption depends on environment readiness rather than model capability.

**Key contributions:** The "agents captain" role, AGENTS.md as living documentation, the "say no to slop" quality principle, infrastructure priorities (observability, trajectory tracking, tool management).

---

## Harness Engineering

**URL:** [openai.com/index/harness-engineering](https://openai.com/index/harness-engineering/)
**Author:** OpenAI

An internal OpenAI team's report on building a complete product over five months with zero manually-written code — approximately one million lines of code across 1,500 PRs, produced by a small team (three engineers, later seven) averaging 3.5 PRs per engineer per day. The case study demonstrates the endpoint of the delegation model: engineering work shifted entirely from writing code to "designing environments, creating feedback loops, and building scaffolding that enables agents to work autonomously." The repository is optimized first for agent legibility — documentation functions as "a navigable map rather than a manual," architectural invariants are mechanically enforced, and all context is repository-local. The companion guide ("Building an AI-Native Engineering Team") adds a quantitative capability benchmark: agents capable of roughly two hours of continuous work at moderate confidence as of mid-2025, with capability doubling approximately every seven months. Concrete practices include test-first discipline for agents (tests must fail before implementation) and continuous automated refactoring to prevent technical debt accumulation.

**Key contributions:** Zero-manual-code product development as end-state evidence, repository-first agent legibility, capability doubling rate as planning variable, TDD-for-agents, continuous preventive refactoring.

---

## Structuring Work Agent-First

**URL:** [x.com/levie/status/2019634114874470819](https://x.com/levie/status/2019634114874470819)
**Author:** Aaron Levie

Levie observes that human workflows rely on implicit context — knowledge of projects, roles, tools, best practices — that "comes for free" by virtue of experience and large human context windows. Agents lack this implicit context, requiring deliberate effort to provide it. The post argues that agentic coding is the leading edge of a broader shift: all knowledge work will soon need to be structured so agents can "jump into a workflow and intently get up to speed." Organizations that maintain "authoritative documented approaches to how things get done" will have a significant advantage.

**Key contributions:** The implicit-vs-explicit context gap, documentation as competitive advantage for agent adoption, agentic coding as leading indicator for all knowledge work.

---

## Code Search at Scale

**URL:** [sourcegraph.com/blog/why-code-search-at-scale-is-essential-when-you-grow-beyond-one-repository](https://sourcegraph.com/blog/why-code-search-at-scale-is-essential-when-you-grow-beyond-one-repository)

The case for organization-wide code search when growing beyond a single repository. As codebases expand across hundreds or thousands of repositories, developers face a knowledge discovery bottleneck — custom patterns, subtle overrides, and cross-service dependencies remain invisible to tools scoped to local workspaces. Security and compliance scenarios demand exhaustive enumeration: when a vulnerability is disclosed, finding a representative sample of affected code is insufficient — every instance across every repository must be identified. The post argues that AI coding tools create an amplification paradox: agents that produce code faster generate more surface area for cross-service breakage, making organization-wide search more valuable, not less.

**Key contributions:** The knowledge discovery bottleneck framing, exhaustive enumeration requirement for security and compliance, the AI-amplification paradox.

---

## Agent-Native Engineering

**URL:** [generalintelligencecompany.com/writing/agent-native-engineering](https://www.generalintelligencecompany.com/writing/agent-native-engineering)
**Author:** Andrew Pignanelli, General Intelligence Company

The argument that background agents changed everything: "agent native engineering means restructuring your organization around agents as individual-contributors instead of engineers." Introduces a three-level task classification — simple (one-shottable), manageable (background agents with feedback cycles), and complex (engineer-managed with synchronous agents) — and reports that in January 2026 their engineers shipped an average of 20 PRs per day. The organizational effects are significant: managers evolve into staff engineers, code review becomes a larger share of workflow than coding, and roadmaps beyond a few weeks become ineffective. Future projections include delegation-first architecture, IDEs as we know them disappearing, and the assertion that "every organization must become agent native, or they will cease to exist."

**Key contributions:** Three-level task classification, organizational transformation effects, the delegation-first architecture projection, the "agent native or cease to exist" framing.

---

## The Cloud Agent Thesis

**URL:** [nader.substack.com/p/the-cloud-agent-thesis](https://nader.substack.com/p/the-cloud-agent-thesis)
**Author:** Nader Dabit

Cloud agents as a distinct category from local agents — running on remote infrastructure with their own shell, IDE, and browser, accessible asynchronously through Slack, Jira, CLI, or API. The key distinctions from local tools: accessibility (non-engineers can invoke agents without Git knowledge or local environments), cross-codebase capability (agents configured against every organizational repository), async parallelism (ten tasks kicked off simultaneously yielding ten PRs), and org-wide scaling through playbooks — reusable workflows encoding expertise that anyone can trigger. The organizational transformation requirements are explicit: strategic project selection, knowledge codification, adoption analytics, governance frameworks, and workflow redesign. The pattern also demands a complementary review agent to handle the volume of PRs that parallel agent sessions generate.

**Key contributions:** Cloud-vs-local agent taxonomy, playbooks as reusable expertise, the review agent corollary, organizational transformation prerequisites.

---

## How Cognition Uses Devin to Build Devin

**URL:** [nader.substack.com/p/how-cognition-uses-devin-to-build](https://nader.substack.com/p/how-cognition-uses-devin-to-build)
**Author:** Nader Dabit

A practitioner account of Cognition using their own coding agent as an autonomous teammate across the development workflow. The key contributions beyond Dabit's earlier cloud agent thesis: structural precision on playbooks (outcome definitions, required steps, postconditions, corrections to defaults, forbidden actions), event-driven agent triggering through REST APIs (crash logs spawning investigation PRs, bug reports triggering reproduction, failed deployments initiating analysis), and session analytics that create continuous improvement loops — insights from one session inform the next. The organizational scope extends beyond engineering through dedicated data analyst agents that let non-engineers answer metrics questions in natural language. The review system transforms large PRs into organized diffs with explanations, autofix capabilities, bug catching with confidence levels, and codebase-aware chat — a concrete implementation of the complementary review agent the cloud agent model demands.

**Key contributions:** Structured playbook specification patterns, event-driven programmatic agent triggering, session-to-session compounding through analytics, review agent as organized-diff system, non-engineering agent access extending to organizational analytics.

---

## Why We Built Our Own Background Agent

**URL:** [engineering.ramp.com/post/why-we-built-our-background-agent](https://engineering.ramp.com/post/why-we-built-our-background-agent)
**Author:** Ramp Engineering

Ramp's in-house background agent, Inspect, writes code and verifies its work through integrated tools and context. The rationale: "it only has to work on your code" — owning the tooling enables capabilities that off-the-shelf tools cannot match. Infrastructure runs on sandboxed VMs on Modal with images rebuilt every 30 minutes, providing the complete local engineering toolkit (Vite, Postgres, Temporal). The context layer is wired into Sentry, Datadog, LaunchDarkly, Braintrust, GitHub, Slack, and Buildkite. Within months of deployment, approximately 30% of all pull requests merged to frontend and backend repositories were written by Inspect, achieved through organic adoption rather than mandated usage. Multiplayer sessions support concurrent collaboration with attribution per contributor, enabling non-engineers to participate in engineering work.

**Key contributions:** The "only has to work on your code" rationale for building in-house, 30% agent-written PR rate through organic adoption, multiplayer collaboration sessions.

---

## Minions: Stripe's One-Shot Coding Agents

**URL:** [stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents](https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents)
**Author:** Alistair Gray, Stripe

Stripe's homegrown coding agents produce more than 1,000 pull requests merged weekly with no human-written code. The infrastructure: isolated devboxes pre-warmed in 10 seconds (the same machines engineers use), pre-loaded with Stripe code and services, isolated from production and the internet. The context layer centers on Toolshed, Stripe's central internal MCP server hosting 400+ MCP tools, providing access to internal docs, ticket details, build statuses, and Sourcegraph code intelligence. Minions read the same rule files as human tools (Cursor, Claude Code) and deterministically run relevant MCP tools pre-run to hydrate context. Verification uses a shift-left philosophy: local linting in under 5 seconds, selective test execution from a battery of 3+ million tests, and a maximum of two CI rounds per run. Built on a fork of Block's "goose" coding agent with customized orchestration interleaving agent loops with deterministic code operations.

**Key contributions:** 1,000+ weekly merged PRs at scale, Toolshed as centralized MCP infrastructure, deterministic context hydration, shift-left verification with selective testing from 3M+ tests.

---

## Advanced Context Engineering for Coding Agents

**URL:** [github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md)
**Author:** Dex Horthy

The argument that AI coding fails in production codebases not due to model limitations but due to poor context management. Introduces "human leverage" — errors in research cascade into thousands of bad lines of code, errors in plans cascade into hundreds, so human attention should concentrate on the highest-leverage stages (research validation and plan review) rather than code review. Reports shipping 35K LOC in 7 hours on an unfamiliar Rust project using a research→plan→implement workflow. Concludes that coding agents will be commoditized but "the hard part will be the team and workflow transformation" — organizations must drive the culture/process/tech shift to an AI-first coding world. Reinforces the spec-first paradigm: specs should be treated as "the real code," not discarded after agent compilation.

**Key contributions:** The human leverage framework (research > plans > code in error cascade potential), the brownfield-vs-greenfield context gap, spec-first agentic workflows as organizational imperative, the team/workflow transformation thesis.

---

## Advanced Context Engineering for Coding Agents (Talk)

**URL:** [youtube.com/watch?v=rmvDxxNubIg](https://www.youtube.com/watch?v=rmvDxxNubIg)
**Author:** Dex Horthy

Conference talk at AI Engineer Summit presenting the context engineering framework. Same core argument as the written document — structured research→plan→implement workflows with human leverage concentrated on upstream decisions.

**Key contributions:** Practitioner-oriented presentation of the context engineering framework, live demonstration of the research→plan→implement workflow.

---

## Cross-Repository Code Navigation

**URL:** [sourcegraph.com/blog/cross-repository-code-navigation](https://sourcegraph.com/blog/cross-repository-code-navigation)

The distinction between text-based search and semantic code navigation. Text search finds character patterns; semantic navigation understands relationships between code elements — resolving symbols to their actual definitions across repository boundaries, tracking which specific implementation version calling code depends on, and disambiguating identically-named functions across dozens of repositories. Language-agnostic indexing formats (such as SCIP) capture compiler-accurate semantic information — symbol definitions, external references, and package metadata — enabling go-to-definition and find-references that work across repository boundaries with version awareness.

**Key contributions:** Text search vs. semantic navigation distinction, cross-repository symbol resolution, version-aware dependency tracking.

---

## Internal Plugin Marketplaces

**URL:** [code.claude.com/docs/en/discover-plugins](https://code.claude.com/docs/en/discover-plugins)

Claude Code's plugin marketplace system as a reference implementation for internal agent tool distribution. A marketplace is a catalog of plugins — skills, agents, hooks, and MCP servers — that engineers can discover and install without building from scratch. The two-step model (add a marketplace to browse its catalog, then install individual plugins) separates capability discovery from adoption. Plugins install at three scopes: user (personal, across all projects), project (shared with collaborators via repository settings), and local (personal, single repository). Team distribution is built in: administrators add marketplace configuration to project settings so team members are automatically prompted to install when they join a repository. Capability categories include code intelligence (LSP integration for type-aware navigation), external integrations (pre-configured MCP servers for GitHub, Jira, Sentry, Slack, and others), and development workflows (commit, PR review, agent SDK tooling). The organizational value lies in encoding team-specific workflows, conventions, and tool integrations as distributable, versionable packages.

**Key contributions:** Marketplace-as-catalog model for agent tool distribution, scope-based installation (user/project/local), team-level plugin configuration for automatic onboarding, plugin categories spanning code intelligence, external integrations, and development workflows.
