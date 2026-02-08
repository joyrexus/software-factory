# Loop Detection

The agent loop detects when the model is stuck in a repetitive pattern — making the same tool calls over and over without progress.

## The Problem

LLMs can get stuck in loops:
- Reading the same file repeatedly
- Running the same failing test without changing anything
- Editing and reverting the same code block
- Alternating between two approaches without committing to either

Without detection, these loops consume tokens indefinitely until a round/turn limit is hit.

## Call Signature Tracking

Every tool call generates a signature — a hash of the tool name and its arguments:

```python
signature = hash(tool_name + sorted(arguments))
```

The loop detector maintains a sliding window of recent call signatures.

## Cycle Detection

The detector checks for three patterns:

### 1-Call Cycle

The same tool call repeated consecutively:

```
read_file("src/auth.py") → read_file("src/auth.py") → read_file("src/auth.py")
```

**Detection**: Current signature matches the previous signature.

### 2-Call Cycle

Alternating between two tool calls:

```
edit_file("src/auth.py", ...) → run_tests() → edit_file("src/auth.py", ...) → run_tests()
```

**Detection**: Current signature matches the signature from 2 calls ago, and the previous signature matches the one from 3 calls ago.

### 3-Call Cycle

A repeating pattern of three tool calls:

```
read → edit → test → read → edit → test
```

**Detection**: Signatures at positions (n, n-1, n-2) match (n-3, n-4, n-5).

## Warning Injection

When a loop is detected, the agent loop injects a warning message into the conversation:

```
⚠️ Loop detected: You appear to be repeating the same sequence of tool calls
without making progress. The following pattern has repeated {count} times:
  1. {tool_call_1}
  2. {tool_call_2}

Please try a different approach. Consider:
- Re-reading error messages more carefully
- Trying an alternative solution strategy
- Breaking the problem into smaller steps
```

The warning is injected as a user message so the model sees it on the next turn.

## Escalation

If the loop continues after warning injection:

| Occurrence | Action |
|------------|--------|
| First detection | Inject warning message |
| Second detection (same pattern) | Inject stronger warning + suggest specific alternatives |
| Third detection | Force stop the session with `LOOP_DETECTED` status |

The forced stop lets the pipeline's failure routing handle the situation (retry with a different approach, route to a human, etc.).

## False Positives

Some repetitive patterns are legitimate:
- Running tests after each edit (the edits are different, but the test command is the same)
- Reading multiple files in sequence

The signature includes arguments, so `run_tests()` after different edits has the same signature but the context is different. The detector accounts for this by requiring **consecutive** repetition of the full cycle, not just individual tool reuse.

## See Also

- [agentic-loop.md](agentic-loop.md) — Where loop detection sits in the loop
- [session-and-config.md](session-and-config.md) — Round/turn limits as backup safety nets
- [../01-pipeline-orchestrator/retry-and-failure.md](../01-pipeline-orchestrator/retry-and-failure.md) — How the pipeline handles loop-detected failures
