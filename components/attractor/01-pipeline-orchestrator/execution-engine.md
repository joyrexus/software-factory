# Execution Engine

The execution engine is the core runtime that walks the graph, executes handlers, and manages transitions between nodes.

## 5-Phase Lifecycle

Every pipeline run proceeds through five phases:

### Phase 1: Parse
Read the DOT file and construct an in-memory graph representation (AST). Apply any registered transforms (AST modifications) at this stage.

### Phase 2: Validate
Run structural validation rules against the parsed graph:
- Exactly one start node (shape=ellipse, designated as entry)
- Exactly one exit node (shape=ellipse, designated as exit)
- All nodes reachable from start
- Exit node reachable from every non-exit node
- Valid condition expression syntax on all edges
- No duplicate node names
- All referenced handler shapes are registered

Validation produces typed diagnostics (see [extensibility.md](extensibility.md)). ERROR-severity diagnostics halt execution; WARNING and INFO are logged but don't block.

### Phase 3: Initialize
Set up the execution context:
- Create the key-value context store with initial values
- Initialize the checkpoint system
- Resolve the model stylesheet
- Set the current node to the start node

### Phase 4: Execute
Run the core traversal loop (see below).

### Phase 5: Finalize
After the exit node is reached:
- Verify all goal gates are satisfied (see [goal-gates.md](goal-gates.md))
- Save final checkpoint
- Emit completion events
- Return the pipeline outcome

## Core Traversal Loop

```python
current_node = start_node

while current_node != exit_node:
    # 1. Look up the handler for this node's shape
    handler = registry.get_handler(current_node.shape)

    # 2. Expand template variables in the node's attributes
    expanded = expand_variables(current_node, context)

    # 3. Execute the handler
    outcome = handler.execute(expanded, context)

    # 4. Update context with the outcome's context updates
    context.update(outcome.context_updates)

    # 5. Save checkpoint
    save_checkpoint(current_node, context, outcome)

    # 6. Emit execution events
    emit_event(NodeCompleted(node=current_node, outcome=outcome))

    # 7. Select outgoing edge (5-step priority)
    edge = select_edge(current_node, outcome, context)

    if edge is None:
        # No matching edge — enter failure routing
        edge = failure_route(current_node, outcome, context)

    # 8. Apply edge fidelity (affects context for next node)
    apply_fidelity(edge, context)

    # 9. Move to next node
    current_node = edge.target
```

## Edge Selection

Step 7 above uses the 5-step priority system detailed in [edge-routing-and-conditions.md](edge-routing-and-conditions.md):

1. Condition match → 2. Preferred label → 3. Suggested node IDs → 4. Weight → 5. Lexical

## Execution Ordering

Within a single path (no parallelism), execution is strictly sequential — one node at a time. The engine never speculatively executes ahead.

For parallel execution, the `parallel` handler spawns concurrent branches that each run their own traversal loop independently. See [parallelism.md](parallelism.md).

## Error Boundaries

If a handler throws an unhandled exception:

1. The exception is caught by the engine
2. An `Outcome` with `status=FAILURE` is synthesized, including the error details
3. Normal edge selection proceeds (conditions can match on `status == 'FAILURE'`)
4. If no failure edge exists, retry/failure routing activates (see [retry-and-failure.md](retry-and-failure.md))

The engine itself never crashes from a handler failure — it always converts exceptions to outcomes and continues the routing logic.

## Resume from Checkpoint

If a previous run was interrupted, the engine can resume:

1. Load the most recent checkpoint
2. Restore context and current node position
3. Re-enter the traversal loop at the checkpointed node
4. Continue execution normally

See [checkpoints-and-resume.md](checkpoints-and-resume.md) for details on what's preserved and potential fidelity degradation.

## See Also

- [graph-definition.md](graph-definition.md) — How the graph is defined
- [node-types-and-handlers.md](node-types-and-handlers.md) — What handlers do at step 3
- [edge-routing-and-conditions.md](edge-routing-and-conditions.md) — Edge selection at step 7
- [state-and-context.md](state-and-context.md) — Context updates at step 4
- [checkpoints-and-resume.md](checkpoints-and-resume.md) — Checkpoint save at step 5
