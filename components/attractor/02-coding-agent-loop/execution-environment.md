# Execution Environment

The ExecutionEnvironment interface abstracts where tools actually run — from a local shell to a remote container.

## ExecutionEnvironment Interface

```python
class ExecutionEnvironment:
    def run_command(self, command: str, timeout: int = None) -> CommandResult:
        """Execute a shell command and return the result."""
        ...

    def read_file(self, path: str) -> str:
        """Read a file's contents."""
        ...

    def write_file(self, path: str, content: str) -> None:
        """Write content to a file."""
        ...

    def list_directory(self, path: str) -> list[str]:
        """List directory contents."""
        ...

    def file_exists(self, path: str) -> bool:
        """Check if a file exists."""
        ...
```

All tool implementations delegate to the environment for actual I/O. This means the same agent loop code works regardless of where execution happens.

## LocalExecutionEnvironment

The default implementation — runs commands in a local shell, reads/writes local files:

```python
env = LocalExecutionEnvironment(
    working_directory="/path/to/project",
    env_vars=filtered_env_vars,
    shell="/bin/bash"
)
```

### Environment Variable Filtering

The local environment filters environment variables to prevent leaking sensitive data to LLM tool calls:

- **Allowed**: `PATH`, `HOME`, `LANG`, `USER`, and explicitly allowlisted vars
- **Blocked**: API keys, tokens, secrets, and anything matching sensitive patterns
- **Custom**: Sessions can specify additional allowed/blocked patterns

## Extension Points

The spec envisions additional environment implementations:

| Environment | Description | Use Case |
|-------------|-------------|----------|
| `DockerEnvironment` | Run in a Docker container | Isolation, reproducibility |
| `KubernetesEnvironment` | Run in a K8s pod | Cloud-native, scaling |
| `WASMEnvironment` | Run in a WebAssembly sandbox | Lightweight isolation |
| `SSHEnvironment` | Run on a remote machine via SSH | Existing infrastructure |

These are **not specified in detail** — they're extension points for implementations to fill based on their deployment needs.

### Docker Example (Conceptual)

```python
env = DockerEnvironment(
    image="python:3.12-slim",
    working_directory="/workspace",
    mounts=[("/local/project", "/workspace")],
    resource_limits=ResourceLimits(cpu="2", memory="4g")
)
```

## Environment and the Agent Loop

The environment is configured per-session:

```python
session = Session(
    config=SessionConfig(goal="Implement feature X"),
    environment=LocalExecutionEnvironment(working_directory="/project")
)
```

When the Attractor pipeline creates a codergen session, it passes the appropriate environment. This is a key decision point for implementations — see [../04-implementation-guide/decision-points.md](../04-implementation-guide/decision-points.md).

## See Also

- [agentic-loop.md](agentic-loop.md) — How the loop uses the environment
- [provider-profiles.md](provider-profiles.md) — Tools that delegate to the environment
- [../04-implementation-guide/decision-points.md](../04-implementation-guide/decision-points.md) — Execution environment strategy decisions
- [../07-future-extensions/execution-environments.md](../07-future-extensions/execution-environments.md) — Cloud and sandboxed execution ideas
