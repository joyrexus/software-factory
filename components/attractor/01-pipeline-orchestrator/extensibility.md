# Extensibility

Attractor provides four extension points: transforms, custom handlers, custom lint rules, and custom interviewer implementations.

## Transform Interface

Transforms modify the parsed graph AST before validation. They run during Phase 1 (Parse) of the [execution lifecycle](execution-engine.md).

```python
class Transform:
    def apply(self, graph: Graph) -> Graph:
        """Modify the graph AST and return the (possibly modified) graph."""
        ...
```

Transforms can:
- Add nodes or edges (e.g., inject logging nodes between every pair)
- Modify attributes (e.g., set default retry counts)
- Restructure subgraphs (e.g., expand macros)

### Built-In Transforms

| Transform | Purpose |
|-----------|---------|
| `VariableExpansionTransform` | Expands `$variable` references in attributes |
| `DefaultAttributeTransform` | Applies default attribute values to nodes missing them |
| `StylesheetResolutionTransform` | Resolves model stylesheet rules into per-node attributes |

### Custom Transforms

Register transforms before pipeline execution:

```python
engine.register_transform(MyCustomTransform())
```

Transforms execute in registration order. Each receives the graph as modified by previous transforms.

## Custom Handler Registration

Register new node types by mapping shapes to handler implementations:

```python
engine.register_handler("my_shape", MyCustomHandler())
```

The shape name must not conflict with built-in shapes unless you intentionally want to override the default behavior.

Custom handlers must:
- Accept `(node: Node, context: Context)` arguments
- Return a valid `Outcome`
- Be thread-safe (for use in parallel branches)
- Handle their own errors and timeouts

## Validation Rules

The validation system (Phase 2) runs a set of lint rules against the parsed graph. Each rule produces diagnostics.

### Diagnostic Model

```python
class Diagnostic:
    severity: Severity     # ERROR, WARNING, INFO
    rule_id: str           # Unique rule identifier
    message: str           # Human-readable description
    node_id: str           # Affected node (optional)
    edge: tuple            # Affected edge as (source, target) (optional)
    suggestion: str        # Suggested fix (optional)
```

### Severity Levels

| Severity | Effect |
|----------|--------|
| `ERROR` | Halts execution. The pipeline will not run. |
| `WARNING` | Logged but does not block execution. |
| `INFO` | Informational. Logged at debug level. |

### Built-In Rules

| Rule | Severity | Checks |
|------|----------|--------|
| `single-start` | ERROR | Exactly one start node |
| `single-exit` | ERROR | Exactly one exit node |
| `reachability` | ERROR | All nodes reachable from start |
| `exit-reachable` | ERROR | Exit reachable from all non-exit nodes |
| `valid-conditions` | ERROR | All edge conditions parse correctly |
| `unique-names` | ERROR | No duplicate node names |
| `known-handlers` | ERROR | All shapes have registered handlers |
| `orphan-edges` | WARNING | Edges referencing non-existent nodes |
| `unused-variables` | WARNING | `$variable` references that don't resolve |
| `missing-goal` | WARNING | Codergen nodes without a `goal` attribute |
| `high-retry-count` | INFO | Nodes with `max_retries` > 5 |

### Custom Lint Rules

Register additional rules:

```python
class MyCustomRule:
    rule_id = "my-rule"
    severity = Severity.WARNING

    def check(self, graph: Graph) -> list[Diagnostic]:
        """Return diagnostics for any issues found."""
        ...

engine.register_lint_rule(MyCustomRule())
```

## Custom Interviewer Frontends

The Interviewer interface (see [human-in-the-loop.md](human-in-the-loop.md)) can be replaced with custom implementations:

```python
engine.set_interviewer(MySlackInterviewer(webhook_url="..."))
```

Custom interviewers must implement the `ask(question, choices) -> str` interface.

## Extension Registration Order

All extensions must be registered before `engine.run()`:

1. Transforms (run during Parse)
2. Lint rules (run during Validate)
3. Handlers (invoked during Execute)
4. Interviewer (invoked during Execute, by wait.human nodes)

## See Also

- [execution-engine.md](execution-engine.md) — The 5-phase lifecycle where extensions are invoked
- [node-types-and-handlers.md](node-types-and-handlers.md) — Built-in handlers
- [human-in-the-loop.md](human-in-the-loop.md) — Interviewer interface details
- [graph-definition.md](graph-definition.md) — Graph structure that transforms operate on
