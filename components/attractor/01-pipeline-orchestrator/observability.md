# Observability

The pipeline emits structured events during execution and optionally exposes an HTTP API for monitoring and interaction.

## Event Types

Events are emitted at key points during execution:

### Lifecycle Events

| Event | When |
|-------|------|
| `pipeline.started` | Pipeline begins execution (Phase 3 complete) |
| `pipeline.completed` | Pipeline reaches exit node with all gates satisfied |
| `pipeline.failed` | Pipeline terminates with unresolved failures |
| `pipeline.resumed` | Pipeline resumes from checkpoint |

### Stage Events

| Event | When |
|-------|------|
| `node.started` | A node begins execution |
| `node.completed` | A node finishes (includes outcome) |
| `node.retrying` | A node is being retried (includes retry count) |
| `edge.selected` | An outgoing edge is selected (includes selection reason) |
| `checkpoint.saved` | A checkpoint is written |

### Parallel Events

| Event | When |
|-------|------|
| `parallel.fork` | Execution fans out to branches |
| `parallel.branch_completed` | A branch finishes |
| `parallel.join` | Fan-in completes and contexts are merged |

### Human Events

| Event | When |
|-------|------|
| `human.waiting` | A wait.human node is pending human input |
| `human.responded` | Human has provided a response |
| `human.timeout` | A wait.human node timed out |

### Checkpoint Events

| Event | When |
|-------|------|
| `checkpoint.created` | Checkpoint saved after node completion |
| `checkpoint.loaded` | Checkpoint loaded for resume |

## Event Structure

Every event includes:

| Field | Description |
|-------|-------------|
| `type` | Event type string (e.g., `node.completed`) |
| `run_id` | Pipeline run identifier |
| `timestamp` | When the event occurred |
| `node_id` | Relevant node (if applicable) |
| `data` | Event-specific payload |

## Consumption Patterns

Events can be consumed in several ways:

- **Callback functions**: Register event handlers in code
- **Event log file**: Append-only JSON lines file in the run directory
- **HTTP SSE stream**: Server-sent events for real-time monitoring
- **Webhook**: POST events to an external URL

## HTTP Server Endpoints

When running in HTTP server mode, the engine exposes:

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/pipelines` | GET | List available pipeline definitions |
| `/pipelines/{name}/run` | POST | Start a new pipeline run |
| `/runs` | GET | List active and recent runs |
| `/runs/{run_id}` | GET | Get run status and current node |
| `/runs/{run_id}/events` | GET (SSE) | Stream events in real-time |
| `/runs/{run_id}/context` | GET | Get current context store |
| `/runs/{run_id}/pending` | GET | List pending human decisions |
| `/runs/{run_id}/respond` | POST | Submit response to a human decision |
| `/runs/{run_id}/cancel` | POST | Cancel a running pipeline |

## Run Directory Structure

Each pipeline run produces a timestamped directory of artifacts:

```
runs/
└── 2026-02-08T10-30-00_my-pipeline_abc123/
    ├── pipeline.dot              # Original pipeline definition
    ├── checkpoints/
    │   ├── 001_start.json
    │   ├── 002_generate.json
    │   └── 003_validate.json
    ├── events.jsonl              # Event log (JSON lines)
    ├── nodes/
    │   ├── generate/
    │   │   ├── prompt.md         # Prompt sent to agent
    │   │   ├── response.md       # Agent response
    │   │   ├── outcome.json      # Node outcome
    │   │   └── artifacts/        # Files produced
    │   │       └── src/auth.py
    │   └── validate/
    │       ├── prompt.md
    │       ├── response.md
    │       ├── outcome.json
    │       └── artifacts/
    │           └── test_results.json
    └── status.json               # Final run status
```

## Artifact Management

Artifacts are files produced by node handlers:
- **Codergen nodes**: Source code files, test files, configuration
- **Tool nodes**: Command output, reports, data files
- **Human nodes**: Response records

Artifacts are stored in the run directory under `nodes/{node_id}/artifacts/` and referenced in the node's outcome.

## See Also

- [execution-engine.md](execution-engine.md) — Where events are emitted in the traversal loop
- [human-in-the-loop.md](human-in-the-loop.md) — HTTP endpoints for human interaction
- [checkpoints-and-resume.md](checkpoints-and-resume.md) — Checkpoint files in the run directory
- [../07-future-extensions/observability-and-ops.md](../07-future-extensions/observability-and-ops.md) — Future observability extensions
