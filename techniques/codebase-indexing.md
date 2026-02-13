# Codebase Indexing and Search

*Organization-wide code visibility as infrastructure.*

## The Technique

As an organization's codebase grows beyond a single repository, local search tools become insufficient. A developer — or an agent — working in one service cannot see how their changes affect consumers in other repositories, cannot locate all instances of a vulnerable pattern across the organization, and cannot trace a symbol's definition through transitive dependencies. Codebase indexing addresses this by providing three distinct capabilities:

**Deterministic enumeration.** Structured query languages (regex, symbol filters, diff history) that return *every* match across the entire corpus, not a sample. When a CVE drops and you need to find all uses of a vulnerable function across thousands of repositories, you need exhaustive results — not an AI-curated subset. The distinction matters: security response time directly correlates with how quickly you can enumerate all affected locations.

**Semantic code navigation.** Go-to-definition and find-references that resolve symbols to their actual definitions across repository boundaries. This is fundamentally different from text search — identical function names may exist in dozens of repositories, and navigating to the *correct* implementation requires understanding dependency graphs, package versions, and symbol ownership. Language-agnostic indexing formats capture compiler-accurate semantic information: where symbols are defined, where they're referenced, and which versions are in play.

**AI-assisted deep search.** Natural language queries that follow call paths, analyze files, and synthesize findings across repositories. This layer sits on top of deterministic search and semantic navigation, using them as tools rather than replacing them. An agent can answer "what services call this deprecated API and what would break if we removed it?" by combining structured queries with contextual analysis.

## Why It Matters for Agents

Agents operate in context windows scoped to local directories. They generate code that is locally correct — it passes the tests in front of them, follows the patterns in the files they can see. But without cross-repository visibility, they cannot know that an identical utility already exists in a shared library, that a downstream service depends on the function signature they're about to change, or that the pattern they're implementing was deprecated organization-wide last quarter.

This creates a scaling ceiling. An agent can produce excellent work within a single repository, but the moment its changes need to account for the wider system — blast radius analysis, dependency impact, pattern consistency — it needs indexing infrastructure that extends its vision beyond the local workspace.

The paradox is that AI coding tools make large-scale search *more* valuable, not less. As agents produce code faster, the surface area of change increases. More changes across more repositories means more potential for subtle cross-service breakage, more instances of duplicated implementations, and greater need for organization-wide pattern enforcement. The faster agents produce code, the more important it becomes to index and search what they've produced.

## Capabilities

**Cross-repository impact analysis.** Before changing an API, enumerate every consumer across the organization. Before modifying a shared library, trace every dependent service and the specific versions they pin. This transforms "what might break?" from a best-guess question into a deterministic query.

**Exhaustive vulnerability enumeration.** When a security vulnerability is disclosed, find every instance of the affected pattern — across all repositories, all branches, all code hosts — in minutes rather than days. Deterministic search is non-negotiable here: a representative sample is insufficient when any single missed instance represents ongoing exposure.

**Technical debt quantification.** Measure migration progress across the organization: how many services still use the deprecated authentication library? How many have adopted the new logging standard? These questions require complete enumeration, not estimation.

**Knowledge democratization.** Support and operations teams can answer questions about the codebase without interrupting engineers. "Which service handles payment webhooks?" becomes a searchable query rather than a Slack thread. This reduces interrupt-driven context switching and distributes knowledge access beyond the engineers who originally wrote the code.

## Connection to Readiness

Codebase indexing is environment infrastructure — a precondition for scaling autonomous agent work beyond single-repository tasks. In the [Agent Readiness Model](../meta/agent-native/maturity-model.md), it maps to the Seed stage alongside Documentation, Development Environment, and Task Discovery. Agents need to discover and understand existing code before producing new code. An organization that scores well on other pillars but lacks cross-repository visibility will find that its agents produce locally correct but globally inconsistent work, duplicating existing implementations and breaking downstream consumers they cannot see.

## Implements

- [Agent-Native Environment](../principles/agent-native-environment.md) — Cross-repository visibility is part of the infrastructure agents need to reason about systems, not just files
- [Compound Knowledge](../principles/compound-knowledge.md) — Indexed code becomes searchable organizational knowledge that compounds as the codebase grows

## See Also

- [Filesystem as Memory](filesystem-as-memory.md) — Local file structure as agent memory; codebase indexing extends this across repository boundaries
- [Pyramid Summaries](pyramid-summaries.md) — Multi-level compression within hierarchies; indexing provides the search layer across those hierarchies
- [Codebase Cartography](codebase-cartography.md) — The semantic layer that complements indexing: indexing finds code, cartography explains systems
- [Agent Readiness Model](../meta/agent-native/maturity-model.md) — Codebase indexing as a technical pillar for measuring readiness
