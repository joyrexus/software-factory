# Human-in-the-Loop

Attractor supports explicit human decision points through the `wait.human` handler and the Interviewer interface. This is not conversational interaction — it's structured decision-making at predefined pipeline points.

## Wait.Human Handler (shape=house)

A `wait.human` node pauses pipeline execution and presents a question to the human:

```dot
approve [shape=house label="Deploy to staging?" goal="Review changes and decide"]
approve -> deploy [label="[Y] Yes, deploy"]
approve -> revise [label="[N] No, revise"]
approve -> cancel [label="[C] Cancel pipeline"]
```

The outgoing edge labels become the choices presented to the human.

## Interviewer Interface

The Interviewer is an abstraction over how human interaction actually happens:

```python
class Interviewer:
    def ask(self, question: str, choices: list[Choice]) -> str:
        """Present a question with choices and return the human's response."""
        ...
```

### Implementations

| Implementation | Use Case |
|----------------|----------|
| `ConsoleInterviewer` | CLI-based prompts. Displays choices and reads input from stdin. |
| `AutoApproveInterviewer` | Automated testing. Always selects the first choice (or a configured default). |
| `CallbackInterviewer` | Programmatic integration. Calls a registered callback function. |
| `QueueInterviewer` | Deterministic replay. Reads responses from a pre-loaded queue. |
| `RecordingInterviewer` | Wraps another interviewer and records responses for later replay. |

Custom implementations can integrate with external systems:

- **Slack bot**: Post question as a message with reaction buttons
- **Web UI**: Render a form and wait for submission
- **Email**: Send choices and poll for reply
- **API callback**: POST to a webhook and wait for response

## Accelerator Keys

Edge labels can embed accelerator keys using `[X]` syntax:

```dot
approve -> yes_path [label="[Y] Yes, proceed"]
approve -> no_path  [label="[N] No, stop"]
```

The engine extracts these keys and uses them for fuzzy matching against human responses:

| Human Input | Matches |
|-------------|---------|
| `Y` | `[Y] Yes, proceed` |
| `y` | `[Y] Yes, proceed` (case-insensitive) |
| `yes` | `[Y] Yes, proceed` (prefix match on label text) |
| `Yes, proceed` | `[Y] Yes, proceed` (full label match) |
| `1` | First edge by order |

The matching is intentionally forgiving — the engine normalizes input and tries multiple matching strategies before giving up.

## Timeout Handling

`wait.human` nodes can have a timeout:

```dot
approve [shape=house label="Review?" timeout=3600 timeout_action=first]
```

| Attribute | Description |
|-----------|-------------|
| `timeout` | Maximum wait time in seconds |
| `timeout_action` | What to do on timeout: `first` (select first edge), `fail` (return FAILURE), `skip` (return SKIPPED) |

If no timeout is configured, the node waits indefinitely.

## Human Response as Outcome

The human's response becomes the node's outcome:

- `preferred_label` is set to the selected edge's label
- `status` is `SUCCESS`
- `context_updates` may include `_human_response` with the raw input

Edge selection then uses priority 2 (preferred label matching) to route to the chosen edge.

## HTTP Server Mode

When the engine runs in HTTP server mode (see [observability.md](observability.md)), human-in-the-loop interactions can be served as REST API endpoints:

- `GET /pipeline/{run_id}/pending` — List pending human decisions
- `POST /pipeline/{run_id}/respond` — Submit a response to a pending decision

This enables web-based UIs for human interaction without custom Interviewer implementations.

## See Also

- [node-types-and-handlers.md](node-types-and-handlers.md) — Wait.human handler description
- [edge-routing-and-conditions.md](edge-routing-and-conditions.md) — How label matching drives edge selection
- [observability.md](observability.md) — HTTP server mode for web-based interaction
- [extensibility.md](extensibility.md) — Custom Interviewer implementations
