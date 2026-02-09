# Node Types and Handlers

Every node in an Attractor graph has a `shape` attribute that determines which handler executes it. The handler system is pluggable — implementations register handlers by shape name.

## Handler Interface

```python
class Handler:
    def execute(self, node: Node, context: Context) -> Outcome:
        """Execute the node's task and return an outcome."""
        ...
```

Every handler receives the node definition (including all attributes) and the shared context store. It returns an `Outcome` describing what happened (see [state-and-context.md](state-and-context.md)).

## Shape-to-Handler Mapping

| Shape | Handler | Purpose |
|-------|---------|---------|
| `box` | `codergen` | Default. Invokes an LLM coding agent session |
| `ellipse` | `terminal` | Start and exit nodes. No-op execution |
| `diamond` | `conditional` | Routes based on edge conditions without executing work |
| `hexagon` | `parallel` | Fans out to multiple branches concurrently |
| `invhouse` | `fan_in` | Waits for parallel branches to complete and consolidates |
| `house` | `wait.human` | Pauses for human input via the Interviewer interface |
| `component` | `tool` | Executes a specific tool directly (no LLM involved) |
| `tab` | `stack.manager_loop` | Manages a sub-pipeline or iterative loop |

## Handler Details

### Codergen (shape=box)

The default and most common handler. Creates a coding agent session (via the `CodergenBackend` interface) and runs it:

1. Expands template variables in the `goal` attribute (e.g., `$feature_name`)
2. Prepares the system prompt with context from prior nodes
3. Creates an agent session with the configured model (from the model stylesheet)
4. Runs the agentic loop until completion or stop condition
5. Collects artifacts (files written, test results, etc.)
6. Returns an `Outcome` with status, context updates, and artifact references

The `CodergenBackend` is the integration point with the [Coding Agent Loop](../02-coding-agent-loop/README.md).

### Terminal (shape=ellipse)

Start and exit nodes. The start node initializes the pipeline context. The exit node finalizes execution. Neither performs computational work.

### Conditional (shape=diamond)

A routing-only node. Doesn't execute any work — just evaluates its outgoing edge conditions against the current context and routes to the matching edge. Useful for explicit branching points.

### Parallel (shape=hexagon)

Fans execution out to multiple branches simultaneously:

1. Clones the current context for each outgoing branch
2. Launches all branches concurrently
3. Each branch executes independently with its own context copy

The join policy (configured on the corresponding `fan_in` node) determines when and how branches are consolidated. See [parallelism.md](parallelism.md).

### Fan-In (shape=invhouse)

The counterpart to `parallel`. Waits for branches to complete according to the join policy, then consolidates their context updates into a single merged context. See [parallelism.md](parallelism.md).

### Wait.Human (shape=house)

Pauses execution and presents a question to the human via the Interviewer interface. The outgoing edges serve as the available choices — their labels become the options presented. See [human-in-the-loop.md](human-in-the-loop.md).

### Tool (shape=component)

Executes a specific tool directly without involving an LLM. Useful for deterministic operations (run tests, deploy, check status) that don't need AI reasoning.

### Stack Manager Loop (shape=tab)

Manages iterative or recursive sub-workflows. Can invoke a sub-pipeline and loop based on outcomes.

## Custom Handlers

Implementations can register custom handlers for domain-specific node types:

```python
engine.register_handler("my_shape", MyCustomHandler())
```

Custom handlers must implement the same `Handler` interface and return valid `Outcome` objects. See [extensibility.md](extensibility.md).

## See Also

- [graph-definition.md](graph-definition.md) — How nodes are declared in DOT
- [execution-engine.md](execution-engine.md) — How handlers are invoked during traversal
- [state-and-context.md](state-and-context.md) — The Outcome model handlers return
- [../02-coding-agent-loop/agentic-loop.md](../02-coding-agent-loop/agentic-loop.md) — What happens inside a codergen session
