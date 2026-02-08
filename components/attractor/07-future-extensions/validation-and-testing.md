# Validation and Testing Extensions

Extensions to the validation model beyond the core goal gates and retry routing.

## Digital Twin Universe (DTU)

StrongDM's [DTU technique](https://factory.strongdm.ai/techniques/dtu) creates behavioral clones of third-party services for testing at scale.

### The Problem

Agent-produced code interacts with external services (APIs, databases, payment processors, auth providers). Testing against real services is slow, expensive, rate-limited, and non-deterministic.

### The Idea

Build behavioral twins — not just mock responses, but models that replicate the **behavior patterns** of real services:

- Error rates and failure modes
- Latency distributions
- State management (an order created in one call appears in subsequent queries)
- Edge cases (rate limiting, pagination, timeouts, malformed responses)

### Integration with Attractor

DTU environments could be plugged into the execution environment layer:

```
Pipeline node goal: "Implement Stripe payment integration"
Execution environment: DTU with Stripe behavioral twin
Validation: Agent's code tested against realistic Stripe behavior
```

Goal gates could verify not just "tests pass" but "tests pass against the behavioral twin with realistic failure injection."

## Scenario-Based Validation

Moving beyond boolean tests to probabilistic satisfaction metrics.

### Current Model

Goal gates are binary: SUCCESS or FAILURE. Tests either pass or don't.

### Extended Model

Scenarios define expected behaviors with confidence thresholds:

- "The auth module handles OAuth2 flow correctly" — validated by running 100 scenarios with varied inputs, targeting >95% success rate
- "Error handling covers all edge cases" — validated by injecting 50 failure modes, targeting >90% correct handling

This shifts from "did it work?" to "how well does it work?" — more appropriate for probabilistic AI-generated outputs.

## Adversarial Testing

Agents generating edge cases and attack scenarios for agent-produced code.

### Approach

Use a separate agent session (or pipeline branch) dedicated to **breaking** the code:

1. Generate code in a codergen node
2. In a parallel branch, run an adversarial agent that tries to find bugs, security issues, and edge cases
3. Feed adversarial findings back to the generation branch for fixing
4. Iterate until the adversarial agent can't find new issues

This is essentially red-team/blue-team for code generation.

## Continuous Validation Pipelines

Validation that runs alongside development, not just at the end.

### Approach

Instead of validate-once-at-the-end, create pipeline structures that continuously validate as code evolves:

- After every file change, run relevant tests
- After every module completion, run integration tests
- After the full implementation, run the adversarial agent

This maps naturally to Attractor's graph structure — validation nodes woven throughout the pipeline, not just at the exit.

## See Also

- [../01-pipeline-orchestrator/goal-gates.md](../01-pipeline-orchestrator/goal-gates.md) — Current goal gate mechanics
- [../01-pipeline-orchestrator/parallelism.md](../01-pipeline-orchestrator/parallelism.md) — Parallel branches for adversarial testing
- [../00-overview/software-factory-context.md](../00-overview/software-factory-context.md) — DTU in the Software Factory vision
- [../06-comparisons/ramp-inspect.md](../06-comparisons/ramp-inspect.md) — Ramp's production monitoring verification approach
