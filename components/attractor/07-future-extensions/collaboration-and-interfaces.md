# Collaboration and Interfaces

Extensions for human interaction, team collaboration, and user-facing interfaces beyond the core Interviewer pattern.

## Multi-User Real-Time Collaboration

Ramp's "multiplayer" model where multiple people can observe and guide an agent session simultaneously.

### Concept

- Multiple team members connect to a running pipeline or agent session
- Real-time streaming of agent activity (tool calls, file edits, reasoning)
- Any connected user can provide guidance or steering
- Shared cursor/awareness (who's watching, who's interacting)

### Implementation Approach

- WebSocket connections for real-time streaming
- Presence tracking (who's connected to this session)
- Steering queue accepts input from any connected user
- Conflict resolution when multiple users steer simultaneously (queue-based, first-in-first-out)

### When This Matters

- Complex pipeline runs where multiple team members have relevant expertise
- Onboarding: new team members observe experienced engineers guiding agents
- Debugging: multiple perspectives on a stuck pipeline

## Slack/Chat Integration

Asynchronous agent interaction via team chat.

### Concept

A Slack bot that can:
- Receive pipeline triggers (`/attractor run auth-pipeline`)
- Post progress updates as the pipeline executes
- Present `wait.human` decisions as interactive messages with buttons
- Stream agent output to a dedicated channel
- Accept steering commands (`/steer Focus on error handling first`)

### Implementation

An Interviewer implementation that uses the Slack API:

```python
class SlackInterviewer(Interviewer):
    def ask(self, question: str, choices: list[Choice]) -> str:
        # Post message with choice buttons
        # Wait for button click
        # Return selected choice
```

### Why Slack First

- Lowest friction for teams already using Slack
- Asynchronous — respond when convenient
- Natural audit trail (conversation history)
- Notifications bring attention to pending decisions

## Web-Based IDE Embedding

A hosted VS Code or similar editor for viewing and interacting with agent work.

### Concept

- Embedded editor showing the agent's workspace in real-time
- File changes appear as the agent makes them
- User can view, comment on, or override agent edits
- Side panel shows pipeline progress and node status

### Implementation

- VS Code Server or code-server for the editor
- File system events for real-time updates
- Custom VS Code extension for pipeline integration
- WebSocket bridge between editor and pipeline events

## Chrome Extension

Browser-based interaction with running pipelines.

### Concept

- Overlay on GitHub PRs showing which changes were agent-generated
- Quick access to pipeline status from any page
- One-click approval for pending human decisions
- Notification badges for pending decisions

## PR-Based Interaction

Agents participate in code review workflows.

### Concept

- Pipeline outputs become PRs automatically
- Reviewers comment on the PR; comments become steering input for the agent
- Agent responds to review comments by making changes and pushing updates
- Pipeline status appears as PR checks

### Integration Pattern

```
Pipeline completes → Creates PR → Reviewer comments
→ Comment triggers pipeline re-run with steering from comment
→ Agent addresses feedback → Updates PR
→ Cycle until approved
```

## Organization-Wide Dashboards

Metrics and monitoring across all pipeline runs.

### Concept

- Total pipeline runs, success rates, average duration
- Most-used pipeline templates
- Token usage and cost tracking
- Agent performance metrics (how often do retries succeed?)
- Team adoption metrics (who's using pipelines, how often)

### Implementation

- Aggregate events from all pipeline runs
- Time-series storage for trend analysis
- Dashboard UI (Grafana, custom web app, or similar)
- Alerting on pipeline failures or cost anomalies

## See Also

- [../01-pipeline-orchestrator/human-in-the-loop.md](../01-pipeline-orchestrator/human-in-the-loop.md) — Core Interviewer pattern
- [../01-pipeline-orchestrator/observability.md](../01-pipeline-orchestrator/observability.md) — Events and HTTP server
- [../06-comparisons/ramp-inspect.md](../06-comparisons/ramp-inspect.md) — Ramp's multiplayer model and Slack integration
- [../04-implementation-guide/decision-points.md](../04-implementation-guide/decision-points.md) — Human-in-the-loop UX decision
