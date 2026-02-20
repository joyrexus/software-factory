# Glossary

Cross-cutting vocabulary for the software factory knowledge base. For Attractor-specific terms (pipeline nodes, handlers, DOT DSL), see the [Attractor glossary](components/attractor/05-reference/glossary.md).

---

### Agent Legibility {#agent-legibility}

Making a repository and its running application navigable by agents — documentation functions as a map rather than a manual, architectural invariants are mechanically enforced, and all context is repository-local. Agent legibility extends beyond source code to include runtime observability (per-worktree app boot, browser automation, ephemeral logging stacks) so agents can reason about the application in operation, not just at rest.

See [Codebase Cartography](techniques/codebase-cartography.md), [Practices](meta/agent-native/practices/README.md).

### Agent-Native Environment {#agent-native-environment}

An engineering environment designed for non-human workers rather than adapted from human-centric practices. Requires objective verification through CI and type systems, machine-readable context (e.g., AGENTS.md), small bounded scopes, and recovery paths when work drifts. Agent output quality depends more on environment quality than on agent sophistication — the environment is the lever.

See [Agent-Native Environment](principles/agent-native-environment.md), [Practices](meta/agent-native/practices/README.md).

### Agent Readiness Model {#agent-readiness-model}

A maturity framework that reframes the question from "how good is the agent?" to "how ready is the codebase?" Defines five maturity levels (Functional through Autonomous) and ten technical pillars — from Style & Validation and Testing through Task Discovery and Codebase Indexing. Advancement requires passing 80% of criteria at the current level and all previous levels.

See [Maturity Model](meta/agent-native/maturity-model.md).

### Cloud Agents {#cloud-agents}

Agents running on remote infrastructure with their own shell, IDE, and browser, accessible asynchronously through Slack, Jira, CLI, or API. The shift from synchronous pair programming to asynchronous delegation: engineers describe tasks, kick off sessions, and return to completed pull requests. Ten tasks can run in parallel, and REST APIs enable systems to trigger agent sessions automatically.

See [Cloud Agents](meta/agent-native/cloud-agents.md).

### Codebase Cartography {#codebase-cartography}

Producing structured documentation artifacts — component maps, flow narratives, contract definitions, operational runbooks — in a convention-driven `docs/` directory so agents understand not just where code lives but what the system does and how its parts relate. The goal is agent legibility: making it possible for an agent to reason about the full business domain directly from the repository.

See [Codebase Cartography](techniques/codebase-cartography.md).

### Codebase Indexing {#codebase-indexing}

Organization-wide code search, semantic navigation, and AI-assisted deep search that extend agent visibility beyond local directories. Provides deterministic enumeration across all repositories, go-to-definition and find-references across repository boundaries, and natural language queries that follow call paths. As agents produce code faster, cross-repository search becomes more critical, not less.

See [Codebase Indexing](techniques/codebase-indexing.md).

### Compound Knowledge {#compound-knowledge}

Knowledge from each completed task feeding into the next through structured, machine-readable capture — producing an organization that gets measurably better at building software with each iteration. Happens both implicitly (a well-structured codebase teaches agents how to work in it) and explicitly (living documents like AGENTS.md encode patterns, failure modes, and conventions).

See [Compound Knowledge](principles/compound-knowledge.md).

### Dark Factory {#dark-factory}

Simon Willison's framing for the software factory concept: a manufacturing facility that operates with the lights off because no humans are on the floor. Raises the central validation question — "How can you prove that software you are producing works if both the implementation and the tests are being written for you by coding agents?" — and challenges the economic model.

See [Conversation](meta/conversation.md), [Validation](principles/validation.md).

### Deliberate Naivete {#deliberate-naivete}

The discipline of questioning every inherited engineering practice to determine whether the constraint that justified it still exists. When creating a behavioral clone of a SaaS application shifts from "impractical" to "routine," or when generating code becomes nearly free, previously fixed assumptions about build-vs-buy, testing, and code review may no longer hold.

See [Deliberate Naivete](principles/deliberate-naivete.md).

### Digital Twin Universe {#digital-twin-universe}

A set of in-memory behavioral clones of external services — Okta, Slack, Jira, and others — that agents can run tests against at high volume without rate limits, API costs, or non-determinism. Functions as a holdout set that the implementing agent never sees, with an independent judge evaluating whether the implementation satisfies scenarios against the twin.

See [Digital Twin Universe](techniques/digital-twin-universe.md).

### Entropy Management {#entropy-management}

Using recurring background agents to scan for deviations from architectural standards, update quality grades, and open targeted refactoring PRs — continuous preventive maintenance rather than periodic cleanup sprints. Most resulting PRs are reviewable in under a minute and eligible for automerge, turning technical debt accumulation into a steady-state process.

See [Practices](meta/agent-native/practices/README.md).

### Feedback Loop {#feedback-loop}

The cycle where a coding agent writes code, runs tests, reads errors, revises, and repeats — with each iteration improving output rather than drifting. Operates at two scales: the inner loop converges a single task toward correctness, and the outer loop accumulates knowledge across tasks, making the factory measurably better over time.

See [Feedback Loop](principles/feedback-loop.md).

### Filesystem as Memory {#filesystem-as-memory}

Using the filesystem as the primary memory substrate for coding agents: directories as namespaces, filenames as keys, file contents as values, and README files as navigable tables of contents. Unlike context windows, the filesystem doesn't forget, doesn't hallucinate, and doesn't drift — it extends agent memory indefinitely across sessions.

See [Filesystem as Memory](techniques/filesystem-as-memory.md).

### Gene Transfusion {#gene-transfusion}

Using a working implementation from one codebase as a living specification — an exemplar — for implementing analogous patterns in another. Working code carries implicit knowledge (edge cases, error handling, integration decisions) that abstract specifications tend to omit, and an agent can extract and transfer that implicit knowledge along with the explicit behavior.

See [Gene Transfusion](techniques/gene-transfusion.md).

### Human Leverage {#human-leverage}

The principle that human attention should concentrate on the highest-leverage stages of the agentic workflow. Errors in research cascade into thousands of bad lines of code; errors in plans cascade into hundreds. Therefore, human review of research and plans yields far more value than human review of code — inverting the traditional code-review-centric model.

See [Specification Discipline](techniques/specification-discipline.md), [Seed](principles/seed.md).

### Linters as Guardrails {#linters-as-guardrails}

Shifting linters from style enforcement to machine-enforced architectural guardrails that agents can verify and self-correct against. Seven categories of lint rules for agent-native codebases: grep-ability, glob-ability, architectural boundaries, security and privacy, testability, observability, and documentation signals. Custom lint error messages inject remediation instructions directly into agent context.

See [Practices](meta/agent-native/practices/README.md).

### Playbooks {#playbooks}

Reusable workflows encoding expertise that anyone — including non-engineers — can trigger to invoke agent sessions for common tasks. A structured playbook defines outcome expectations, required steps, postconditions, corrections to defaults, and forbidden actions. Playbooks turn individual expertise into organizational capability.

See [Cloud Agents](meta/agent-native/cloud-agents.md), [Practices](meta/agent-native/practices/README.md).

### Probabilistic Satisfaction {#probabilistic-satisfaction}

Measuring what fraction of observed trajectories through end-to-end user stories likely satisfy the user, rather than relying on binary pass/fail test results. The validation metric for scenario-based testing: a judge evaluates each trajectory against the scenario and reports a satisfaction probability, producing a continuous quality signal rather than a discrete gate.

See [Scenarios Not Tests](techniques/scenarios-not-tests.md), [Validation](principles/validation.md).

### Progressive Disclosure {#progressive-disclosure}

Replacing monolithic context documents with layered, navigable hierarchies: a small, stable entry point (a roughly 100-line AGENTS.md) that teaches agents where to look, pointing into structured directories they load incrementally based on task requirements. Avoids context scarcity, non-guidance when everything is "important," instant rot, and unverifiability.

See [Progressive Disclosure](techniques/progressive-disclosure.md).

### Pyramid Summaries {#pyramid-summaries}

Organizing information at multiple zoom levels — a one-line description, a paragraph overview, a detailed treatment — so agents can navigate from broad to specific without losing the ability to zoom back out. Each level is a complete, coherent summary at its resolution (not a truncation of the level below), and the hierarchy is reversible: higher-level summaries can be regenerated from lower-level content.

See [Pyramid Summaries](techniques/pyramid-summaries.md).

### Risk-Tiered Automation {#risk-tiered-automation}

Assigning graduated levels of agent autonomy based on the blast radius of each action. Low-risk actions (file edits, formatting, local test runs) are fully automated; medium-risk actions (commits, dependency bumps) proceed when validation passes; high-risk actions (production deployments, irreversible operations) require explicit human confirmation. Tiers evolve as confidence grows.

See [Risk-Tiered Automation](techniques/risk-tiered-automation.md).

### Scenarios Not Tests {#scenarios-not-tests}

End-to-end user stories describing observable trajectories through a system, stored independently of the codebase and evaluated by an independent judge. Breaks the circular dependency where an agent writes both implementation and tests by functioning as holdout sets: the implementing agent never sees the scenarios, and a separate evaluator measures probabilistic satisfaction.

See [Scenarios Not Tests](techniques/scenarios-not-tests.md).

### Seed Quality {#seed-quality}

The quality of the specification — the "seed" — determines the ceiling of agent output. A good seed answers five questions: where the agent starts, what done looks like, what the boundaries are, what signals failure, and what prior knowledge applies. As Klaassen frames it, "plans are the new code."

See [Seed](principles/seed.md), [Specification Discipline](techniques/specification-discipline.md).

### Semport {#semport}

Semantic port — migrating code between languages or frameworks by preserving intent rather than syntax. The migration specification describes which behaviors to preserve, not which lines to translate, allowing the agent to re-express the "what" and "why" in the target language's idiom rather than producing a foreign-feeling transliteration.

See [Semport](techniques/semport.md).

### Shift Work {#shift-work}

Separating agent tasks into interactive mode (requiring real-time human judgment, design decisions, or exploratory specification) and non-interactive mode (complete enough for autonomous execution overnight, on weekends, or in parallel). The most token-efficient organizations front-load interactive work to produce complete specifications, then maximize non-interactive implementation.

See [Shift Work](techniques/shift-work.md).

### Software Factory {#software-factory}

An engineering organization where coding agents produce and validate software under human architectural direction, structured around a core formula: Seed (precise specification) → Validation (end-to-end proof of correctness) → Feedback Loop (compounding correctness across iterations), all fueled by tokens as the economic unit of production. The environment — not the model — is the primary lever of agent effectiveness.

See [README](README.md), [Meta](meta/README.md).

### Specification Discipline {#specification-discipline}

The practical skill of writing agent-ready specifications using a self-check heuristic: can you identify (1) where the agent begins reading and (2) the artifact that proves completion? If either is unclear, the spec is a wish, not a specification. A good spec adds starting location, failure signals, scope boundaries, and a done definition without prescribing internal implementation.

See [Specification Discipline](techniques/specification-discipline.md), [Seed](principles/seed.md).

### Task Classification {#task-classification}

A three-level framework for scoping agent work: simple tasks (one-shottable by an inline agent), manageable tasks (background agents with feedback cycles), and complex tasks (engineer-managed with synchronous agents). The classification determines the appropriate delegation mode and level of human involvement.

See [Cloud Agents](meta/agent-native/cloud-agents.md).

### Tokens as Fuel {#tokens-as-fuel}

Tokens replace engineering hours as the primary unit of production cost — a variable cost tied directly to the volume and complexity of work produced rather than fixed monthly headcount. Reframes planning from "how many engineers?" to "how many tokens?" and optimization from developer productivity to token efficiency.

See [Tokens as Fuel](principles/tokens-as-fuel.md).

### Validation {#validation}

End-to-end proof that what was built actually works, tested close to production conditions using scenarios rather than narrow unit tests. The entity that builds must not be the entity that validates — independent evaluation, whether by a judge agent or the environment itself, is the key structural safeguard against the circular dependency of agent-written tests.

See [Validation](principles/validation.md), [Scenarios Not Tests](techniques/scenarios-not-tests.md).
