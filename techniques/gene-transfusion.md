# Gene Transfusion

*Cross-codebase pattern transfer via exemplars.*

## The Technique

Gene transfusion uses working code from one codebase as an exemplar — a living specification — for implementing similar patterns in another. Rather than writing an abstract specification of desired behavior, you point the agent at a working implementation and say "do this, but for our codebase."

Working code is the highest-fidelity specification available. It encodes not just the intended behavior but the edge cases, error handling, and integration patterns that abstract specs tend to omit. When an agent can read a reference implementation and produce an analogous implementation adapted to a different codebase's conventions, the specification problem is largely solved by example.

## How It Works

1. **Identify the exemplar** — Find a working implementation of the pattern you want. This could be in a different service, a different repository, or even a different language.
2. **Specify the adaptation** — Describe how the target codebase differs: different frameworks, conventions, dependencies, naming patterns.
3. **Let the agent transfer** — The agent reads the exemplar, understands the intent, and produces an adapted implementation that fits the target codebase's patterns.

The key insight is that the exemplar carries implicit knowledge — patterns, error handling strategies, integration decisions — that would be difficult to capture in a written specification. The agent extracts and transfers this implicit knowledge along with the explicit behavior.

## When Gene Transfusion Excels

- **Microservice ecosystems** where similar patterns (auth, logging, health checks) should be consistent across services
- **Platform migrations** where existing behavior must be preserved in a new stack
- **Onboarding new services** that should follow established patterns from mature services

## Implements

- [The Seed](../principles/seed.md) — Working code as the highest-quality seed
- [Compound Knowledge](../principles/compound-knowledge.md) — Patterns from one codebase compound into others

## See Also

- [Semport](semport.md) — A related technique focused on language/framework migration
- [Specification Discipline](specification-discipline.md) — Writing specs when no exemplar exists
- [Filesystem as Memory](filesystem-as-memory.md) — Organizing exemplars for retrieval
