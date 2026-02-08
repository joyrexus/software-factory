# Steering and Context

The agent loop supports injecting additional context and guidance into the conversation through steering queues, follow-up queues, and a layered system prompt architecture.

## Steering Queue

The steering queue is a list of messages injected **before the next LLM call**. This lets external systems (the pipeline, human operators, or monitoring) redirect the agent mid-session:

```python
session.steer("Focus on the authentication module first, then handle authorization.")
session.steer("The database schema has changed — re-read schema.sql before continuing.")
```

Steering messages are:
- Consumed once (removed from the queue after injection)
- Prepended to the user message in the next turn
- Visible in the conversation history going forward

## Follow-Up Queue

Follow-up messages are injected **after each turn** completes. Unlike steering (one-shot), follow-ups persist across turns:

```python
session.add_follow_up("Remember to add type hints to all function signatures.")
session.add_follow_up("Run tests after every file modification.")
```

Follow-ups act as persistent reminders — they're appended to the conversation context on every turn.

## System Prompt Layers

The system prompt is assembled from up to 5 tiers, concatenated in order:

| Tier | Source | Content |
|------|--------|---------|
| 1 | **Base prompt** | Core agent identity and capabilities |
| 2 | **Tool instructions** | Available tools and how to use them |
| 3 | **Project instructions** | From AGENTS.md, CLAUDE.md, or similar project files |
| 4 | **Session instructions** | From the pipeline node's goal and context |
| 5 | **Runtime instructions** | Steering and follow-up messages |

Each tier adds specificity. The base prompt is generic ("You are a coding agent"); the session instructions are specific ("Implement OAuth2 login for the FastAPI backend using the existing User model").

## Project Instruction Files

The agent loop looks for project-level instruction files in the working directory:

| File | Purpose |
|------|---------|
| `AGENTS.md` | Instructions for AI agents working on this project |
| `CLAUDE.md` | Claude-specific instructions (Anthropic convention) |
| `.cursorrules` | Cursor-specific rules (recognized but not Attractor-specific) |
| `CONVENTIONS.md` | Coding conventions and style guidelines |

These files are read at session start and included in the system prompt (Tier 3). They provide project-specific context that all agent sessions inherit:

```markdown
# AGENTS.md
- Use Python 3.12+ features
- Follow PEP 8 style
- All functions must have docstrings
- Tests go in tests/ directory, mirroring src/ structure
- Use pytest, not unittest
```

## Context from Pipeline

When the Attractor pipeline creates an agent session, it injects context from the pipeline's key-value store into the system prompt:

```python
# Pipeline context
context = {
    "project_name": "MyApp",
    "language": "python",
    "completed_phases": ["setup", "database"],
    "current_phase": "authentication",
}

# Becomes part of the session instructions (Tier 4)
```

The context fidelity mode (see [../01-pipeline-orchestrator/state-and-context.md](../01-pipeline-orchestrator/state-and-context.md)) controls how much prior session history is included.

## See Also

- [agentic-loop.md](agentic-loop.md) — Where steering is injected in the loop
- [session-and-config.md](session-and-config.md) — How steering/follow-ups are configured
- [../01-pipeline-orchestrator/state-and-context.md](../01-pipeline-orchestrator/state-and-context.md) — Pipeline context and fidelity
