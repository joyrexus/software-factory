# Observability and Ops Extensions

Extensions to the core observability system for production-grade monitoring, cost management, and pipeline analytics.

## CXDB Integration

StrongDM's [CXDB](https://factory.strongdm.ai/products/cxdb) is a conversation-native observability database designed for agent interactions.

### Concept

Instead of (or alongside) generic log aggregation, store agent conversations in a purpose-built database that understands:
- Conversation structure (turns, tool calls, results)
- Token usage per turn
- Temporal relationships between pipeline events
- Cross-session references (which pipeline run created which agent session)

### Benefits

- Query conversations by outcome, cost, duration, tool usage
- Trace a pipeline run's entire conversation tree (all sessions, all turns)
- Compare conversation patterns between successful and failed runs
- Training data extraction for model fine-tuning

## Distributed Tracing

Tracing execution across pipeline nodes, agent sessions, and LLM calls.

### Concept

Apply OpenTelemetry-style distributed tracing:

```
Pipeline Run (trace)
├── Node: generate (span)
│   ├── Agent Session (span)
│   │   ├── LLM Call 1 (span)
│   │   ├── Tool: read_file (span)
│   │   ├── LLM Call 2 (span)
│   │   └── Tool: edit_file (span)
│   └── Subagent Session (span)
│       ├── LLM Call 1 (span)
│       └── Tool: run_tests (span)
├── Node: validate (span)
│   └── Agent Session (span)
│       └── ...
└── Node: deploy (span)
```

### Benefits

- End-to-end latency visibility
- Bottleneck identification (which node takes longest?)
- Error propagation tracking (where did the failure originate?)
- Integration with existing tracing infrastructure (Jaeger, Zipkin, Datadog APM)

## Cost Tracking and Optimization

Per-node, per-pipeline token usage and cost analysis.

### Metrics to Track

| Metric | Granularity | Purpose |
|--------|-------------|---------|
| Token usage | Per LLM call | Raw consumption |
| Token cost | Per node | Cost per pipeline step |
| Cache hit rate | Per session | Caching effectiveness |
| Model selection impact | Per node class | Is the expensive model necessary? |
| Retry cost | Per node | Cost of retries vs first attempts |
| Subagent overhead | Per session | Is delegation cost-effective? |

### Optimization Insights

- Which nodes could use a cheaper model without quality loss?
- Which pipelines have the highest cost-per-success?
- Where do retries add the most cost?
- Is prompt caching working effectively across turns?

### Cost Budgets

Per-pipeline cost limits:

```dot
graph [cost_budget=50.00 cost_currency=USD]
```

When the budget is approached, the engine could:
- Switch to cheaper models (via stylesheet override)
- Reduce context fidelity
- Skip optional nodes
- Alert the operator

## Pipeline Analytics

Understanding pipeline behavior patterns across many runs.

### Convergence Rates

How many retries does each goal gate typically need?

- "The implement_auth gate succeeds on first try 40% of the time, needs 1 retry 35%, needs 2 retries 20%, fails 5%"
- This informs goal gate max_retries settings and pipeline design

### Retry Patterns

Which nodes retry most often? What errors trigger retries?

- Heat map of retry frequency across pipeline nodes
- Common error patterns that cause retries
- Correlation between context fidelity and retry rates

### Bottleneck Identification

Where do pipelines spend the most time?

- Per-node duration distributions
- Parallel branch wait times (is one branch always slower?)
- Human-in-the-loop response times

### Pipeline Comparison

Compare different pipeline designs for the same task:

- Version A (linear) vs Version B (parallel) — which has better success rate?
- Different model stylesheet configurations — impact on quality and cost

## Production Monitoring Integration

Connecting pipeline observability to existing monitoring tools.

### Sentry Integration

- Report pipeline failures as Sentry events
- Attach pipeline context (node, outcome, last LLM exchange) to error reports
- Group related failures (same node, same error pattern)

### Datadog Integration

- Custom metrics for pipeline health (runs/hour, success rate, avg duration)
- Dashboard for real-time pipeline monitoring
- Alerting on anomalies (sudden increase in failures, cost spikes)

### Prometheus/Grafana

- Expose pipeline metrics as Prometheus endpoints
- Pre-built Grafana dashboards for pipeline monitoring
- Histogram metrics for latency distributions

## See Also

- [../01-pipeline-orchestrator/observability.md](../01-pipeline-orchestrator/observability.md) — Core observability system
- [../03-unified-llm-client/caching-and-middleware.md](../03-unified-llm-client/caching-and-middleware.md) — Cost-relevant caching
- [../04-implementation-guide/decision-points.md](../04-implementation-guide/decision-points.md) — Observability infrastructure decision
- [../06-comparisons/ramp-inspect.md](../06-comparisons/ramp-inspect.md) — Ramp's production monitoring approach
