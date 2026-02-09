# 01 — Pipeline Orchestrator

The Attractor pipeline engine — a declarative workflow orchestrator that uses Graphviz DOT syntax to define and execute multi-stage AI coding workflows as directed graphs.

This section decomposes the `attractor-spec.md` into focused topics covering every aspect of the pipeline system.

## Files

1. **[graph-definition.md](graph-definition.md)** — DOT DSL subset, digraph structure, attributes, subgraphs, variable expansion
2. **[node-types-and-handlers.md](node-types-and-handlers.md)** — Shape-to-handler mapping, handler interface, each handler's behavior
3. **[edge-routing-and-conditions.md](edge-routing-and-conditions.md)** — 5-step edge selection priority, condition grammar, label/weight/condition attributes
4. **[execution-engine.md](execution-engine.md)** — 5-phase lifecycle, core traversal loop, execution semantics
5. **[state-and-context.md](state-and-context.md)** — Key-value context store, built-in keys, Outcome model, context fidelity modes
6. **[checkpoints-and-resume.md](checkpoints-and-resume.md)** — Checkpoint structure, what's saved, resume logic, fidelity degradation
7. **[retry-and-failure.md](retry-and-failure.md)** — Retry configuration, backoff presets, failure routing priority
8. **[goal-gates.md](goal-gates.md)** — goal_gate=true semantics, terminal enforcement, retry routing
9. **[parallelism.md](parallelism.md)** — Parallel handler, context cloning, join policies, fan-in consolidation
10. **[human-in-the-loop.md](human-in-the-loop.md)** — Interviewer interface, implementations, accelerator keys, timeout handling
11. **[model-stylesheet.md](model-stylesheet.md)** — CSS-like grammar, selector specificity, declarations, override precedence
12. **[observability.md](observability.md)** — Event types, consumption patterns, HTTP endpoints, run directory structure
13. **[extensibility.md](extensibility.md)** — Transforms, custom handlers, custom lint rules, validation diagnostics

## See Also

- [../00-overview/architecture-overview.md](../00-overview/architecture-overview.md) — Where the pipeline sits in the stack
- [../02-coding-agent-loop/README.md](../02-coding-agent-loop/README.md) — The agent loop that codergen nodes invoke
- [../05-reference/dot-syntax-quick-ref.md](../05-reference/dot-syntax-quick-ref.md) — Compact DOT syntax reference card
- [../05-reference/condition-expressions.md](../05-reference/condition-expressions.md) — Full condition expression grammar
