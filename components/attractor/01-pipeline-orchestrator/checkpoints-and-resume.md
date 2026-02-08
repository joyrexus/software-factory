# Checkpoints and Resume

After each node completes, the engine saves a checkpoint. Checkpoints enable crash recovery, post-mortem analysis, and deterministic replay.

## Checkpoint Structure

A checkpoint captures the full execution state at a point in time:

| Field | Description |
|-------|-------------|
| `run_id` | Unique identifier for this pipeline run |
| `pipeline_name` | Name from the digraph declaration |
| `current_node` | ID of the node that just completed |
| `context` | Full snapshot of the key-value context store |
| `outcome` | The Outcome returned by the completed node's handler |
| `node_history` | Ordered list of (node_id, outcome) pairs for all visited nodes |
| `goal_gate_status` | Map of goal gate node IDs to their current satisfaction status |
| `timestamp` | When this checkpoint was created |
| `fidelity_state` | Current fidelity configuration and any accumulated summaries |

## What's Saved

Checkpoints must capture enough state to resume execution exactly where it left off:

- **Context store**: The complete key-value map, including all updates from every completed node
- **Graph position**: Which node just completed and which edge was selected (or will be selected on resume)
- **Goal gate tracking**: Which goal gates have been satisfied and which are still pending
- **Retry counters**: Per-node retry counts so retry limits aren't reset on resume
- **Artifact references**: Paths to files, logs, and outputs produced by completed nodes

Checkpoints do **not** save:
- In-progress handler state (a node either completed or didn't)
- LLM conversation histories (these are managed by context fidelity, not checkpoints)
- Live network connections or runtime handles

## Resume Logic

When resuming from a checkpoint:

1. **Load** the most recent checkpoint file
2. **Restore** the context store, node history, and goal gate status
3. **Position** the engine at the checkpointed node
4. **Select** the next edge (re-evaluating conditions against the stored outcome)
5. **Continue** the traversal loop normally

If the checkpoint was saved after edge selection, resume at the target node. If saved before edge selection, re-run the selection logic.

## Fidelity Degradation on Resume

Resuming from a checkpoint may lose some context richness compared to a continuous run:

| What's Preserved | What May Be Lost |
|------------------|------------------|
| Context key-value pairs | LLM conversation histories (if fidelity mode was `full`) |
| Node outcomes and artifacts | In-flight handler state |
| Goal gate satisfaction status | Warm caches and connection pools |
| Retry counters | Exact timing and ordering of parallel branches |

In practice, this means a resumed run operates at the equivalent of `compact` fidelity for the resumed node, regardless of the configured fidelity mode. Nodes after the resume point use their configured fidelity normally.

## Checkpoint Storage

The spec is agnostic about storage backend. Implementations may use:

- **Local filesystem**: JSON or binary files in the run directory (simplest)
- **Database**: For distributed execution or long-running pipelines
- **Object storage**: S3/GCS for cloud-native deployments

The key requirement is durability — a checkpoint must survive process crashes.

## Post-Mortem Analysis

Checkpoints serve as an audit trail. After a pipeline run (successful or failed), the checkpoint history shows:

- Every node visited, in order
- The outcome of each node
- How context evolved over time
- Where failures occurred and how they were handled

This supports debugging failed pipelines and understanding successful ones.

## See Also

- [execution-engine.md](execution-engine.md) — Where checkpoints are saved in the traversal loop
- [state-and-context.md](state-and-context.md) — The context store that's checkpointed
- [retry-and-failure.md](retry-and-failure.md) — Retry counters that persist across checkpoints
- [observability.md](observability.md) — Run directory structure for checkpoint files
