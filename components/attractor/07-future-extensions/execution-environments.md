# Execution Environment Extensions

Extensions to the ExecutionEnvironment interface beyond LocalExecutionEnvironment.

## Cloud-Sandboxed Execution

Modal-style VMs (like Ramp uses) for isolated agent execution.

### Concept

Each agent session runs in a dedicated cloud VM:
- **Isolation**: Untrusted code can't affect host systems
- **Reproducibility**: Clean environment for every session
- **Scaling**: Spin up as many VMs as needed
- **Security**: Network policies, resource limits, no access to production

### Implementation Sketch

```python
class ModalEnvironment(ExecutionEnvironment):
    def __init__(self, image: str, resources: ResourceSpec):
        self.sandbox = modal.Sandbox.create(image=image, ...)

    def run_command(self, command: str, timeout: int = None) -> CommandResult:
        return self.sandbox.exec(command, timeout=timeout)
```

### Trade-offs

| Pro | Con |
|-----|-----|
| Strong isolation | Cold start latency (seconds to spin up) |
| Scalable | Infrastructure cost |
| Reproducible | More complex deployment |

## Kubernetes-Native Execution

Running agent sessions as K8s jobs.

### Concept

For organizations already on Kubernetes:
- Agent sessions are K8s Jobs with defined resource limits
- Pipeline parallelism maps to multiple concurrent Jobs
- Pod security policies enforce isolation
- Persistent volume claims share filesystem between related sessions

### Use Cases

- CI/CD integration (agent sessions triggered by CI pipelines)
- Multi-tenant execution (different teams, different namespaces)
- GPU access for model-heavy tasks

## WASM-Based Sandboxing

Lightweight isolation using WebAssembly.

### Concept

For tasks that need isolation but not a full VM:
- Sub-millisecond startup (vs seconds for containers)
- Memory-safe sandbox with capability-based I/O
- Can run in-process or in a dedicated WASM runtime
- Limited to languages that compile to WASM

### Best Suited For

- Lightweight validation tasks (lint, format, static analysis)
- Quick file transformations
- Tasks where container startup latency is unacceptable

### Limitations

- Limited system call support
- No native Docker/container interaction
- Immature ecosystem for complex toolchains

## Remote SSH Execution

Running agent sessions on existing infrastructure.

### Concept

For organizations with existing servers or dev environments:
- Connect to remote machines via SSH
- Run tools on the remote machine, stream results back
- Useful for accessing GPU servers, specialized hardware, or production-like environments

### Implementation Considerations

- SSH key management and authentication
- Latency for interactive tool calls
- File synchronization between local and remote

## Environment Snapshotting and Warm Pools

Reducing startup latency for repeated similar sessions.

### Warm Pools

Pre-create a pool of ready-to-use environments:
- Clone from a base snapshot when a new session starts
- Return to pool (with cleanup) when session ends
- Keep N environments warm for instant allocation

### Snapshotting

Save environment state after common setup steps:
- Install dependencies once, snapshot, reuse for all sessions
- Snapshot after checkpoint for faster resume
- Layer snapshots (base → project → branch)

## See Also

- [../02-coding-agent-loop/execution-environment.md](../02-coding-agent-loop/execution-environment.md) — The ExecutionEnvironment interface
- [../04-implementation-guide/decision-points.md](../04-implementation-guide/decision-points.md) — Execution environment strategy decision
- [../06-comparisons/ramp-inspect.md](../06-comparisons/ramp-inspect.md) — Ramp's Modal-based approach
