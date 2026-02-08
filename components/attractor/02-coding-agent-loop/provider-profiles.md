# Provider Profiles

Different LLM providers have different conventions for code editing tools. The coding agent loop maintains provider-specific tool profiles alongside a shared set of core tools.

## Provider-Specific Tools

### OpenAI Profile

OpenAI models (GPT-5.2 series) work best with the `apply_patch` tool format:

| Tool | Purpose |
|------|---------|
| `apply_patch` / `apply_patch_v4a` | Apply unified diff patches to files |
| `read_file` | Read file contents |
| `list_directory` | List directory contents |
| `run_command` | Execute shell commands |

The `apply_patch` tool expects patches in unified diff format. The `v4a` variant adds enhanced error handling for malformed patches.

### Anthropic Profile

Anthropic models (Claude) use the `edit_file` tool:

| Tool | Purpose |
|------|---------|
| `edit_file` | Search-and-replace editing (old_string → new_string) |
| `read_file` | Read file contents |
| `list_directory` | List directory contents |
| `run_command` | Execute shell commands |

The `edit_file` tool uses exact string matching — the model specifies the text to find and the text to replace it with.

### Gemini Profile

Gemini models use standard file operations:

| Tool | Purpose |
|------|---------|
| `write_file` | Write or overwrite file contents |
| `read_file` | Read file contents |
| `list_directory` | List directory contents |
| `run_command` | Execute shell commands |

## Shared Core Tools

Regardless of provider, every session has access to:

| Tool | Purpose |
|------|---------|
| `read_file` | Read a file's contents (with optional line range) |
| `list_directory` | List files and directories |
| `run_command` | Execute a shell command and return stdout/stderr |
| `search_files` | Search file contents with regex patterns |
| `write_file` | Create a new file (or overwrite) |

The provider-specific tools (apply_patch, edit_file) are **in addition to** these core tools. They represent the provider's preferred editing mechanism.

## Profile Selection

The agent loop selects the profile based on which provider the session is configured to use (via the model stylesheet or session config):

```python
if provider == "openai":
    tools = OPENAI_TOOLS + CORE_TOOLS
elif provider == "anthropic":
    tools = ANTHROPIC_TOOLS + CORE_TOOLS
elif provider == "gemini":
    tools = GEMINI_TOOLS + CORE_TOOLS
```

## Custom Tools

Sessions can register additional tools beyond the standard profiles:

```python
session_config = SessionConfig(
    additional_tools=[my_custom_tool, another_tool]
)
```

Custom tools are appended to the provider profile's tool set.

## See Also

- [agentic-loop.md](agentic-loop.md) — How tools are invoked in the loop
- [execution-environment.md](execution-environment.md) — Where tools execute
- [tools-and-truncation.md](tools-and-truncation.md) — Output truncation for tool results
- [../03-unified-llm-client/tool-calling.md](../03-unified-llm-client/tool-calling.md) — Tool calling protocol at the LLM client level
