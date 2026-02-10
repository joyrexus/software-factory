# Agent Readiness Model

Factory.ai's Agent Readiness Model reframes the question from "how good is the agent?" to "how ready is the codebase?" It provides a structured assessment framework for evaluating whether a codebase — and the engineering environment around it — can support autonomous agent operation. The model defines five maturity levels and ten technical pillars, scored with a gating rule that prevents premature advancement.

See [SOURCES.md](../../SOURCES.md) for the three primary sources: the [introductory blog post](https://www.factory.ai/news/agent-readiness), the [detailed documentation](https://docs.factory.ai/web/agent-readiness/overview), and the [conference talk](https://www.youtube.com/watch?v=ShuJ_CN6zr4).

## Maturity Levels

The model defines five levels of codebase maturity for agent operation:

| Level | Name | Description |
|-------|------|-------------|
| L1 | Functional | Code runs but requires manual setup, no automated validation |
| L2 | Documented | Basic docs and some automation exist |
| L3 | Standardized | Processes defined, documented, enforced through automation — the minimum bar for production-grade autonomous operation |
| L4 | Optimized | Fast feedback loops, data-driven improvement, continuous measurement |
| L5 | Autonomous | Self-improving systems with sophisticated orchestration |

### Gating Rule

Advancement requires passing 80% of criteria at the current level and all previous levels. This gating rule prevents organizations from claiming L4 optimization while lacking L2 documentation.

## Technical Pillars

Ten pillars define what gets measured at each level:

1. **Style & Validation** — automated enforcement of code standards and formatting
2. **Build System** — reproducible, deterministic builds that agents can invoke without human guidance
3. **Testing** — layered test suites (unit, integration, end-to-end) with clear pass/fail signals
4. **Documentation** — structured context that agents can discover and consume
5. **Development Environment** — consistent, reproducible setup that eliminates "works on my machine"
6. **Debugging & Observability** — structured logs, traces, and error reporting agents can interpret
7. **Security** — automated scanning, dependency auditing, and policy enforcement
8. **Task Discovery** — structured backlogs and specifications agents can parse into actionable work
9. **Product & Experimentation** — feature flags, A/B testing, and rollback mechanisms for safe deployment
10. **Codebase Indexing & Search** — organization-wide code search, semantic navigation, and cross-repository impact analysis

## Connection to the Formula

The pillars map directly to the formula — **Seed → Validation → Feedback Loop**.

- **Validation stage:** Style & Validation, Build System, and Testing make validation reliable. These pillars ensure that agent output can be checked programmatically, without human judgment in the loop.
- **Seed stage:** Documentation, Development Environment, Task Discovery, and Codebase Indexing & Search ensure seeds are well-formed. Agents need structured context, reproducible environments, parseable specifications, and cross-repository visibility to produce meaningful output.
- **Feedback Loop stage:** Debugging & Observability, Security, and Product & Experimentation close the feedback loop by making outcomes measurable and recoverable. Telemetry, audit trails, and rollback mechanisms let the system learn from each iteration.

A codebase that scores well across all ten pillars is one where the formula can operate with confidence.

---

**See also:** [questions.md](../questions.md) — the provability question and proving loops | [SOURCES.md](../../SOURCES.md) — annotated bibliography
