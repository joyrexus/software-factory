# The Feedback Loop

*Compounding correctness.*

## The Principle

The late-2024 inflection point was not about models getting smarter. It was about models getting *more reliable in loops*. When a coding agent writes code, runs tests, reads errors, revises, and repeats — and the code gets better with each iteration rather than drifting — you have a feedback loop that compounds correctness instead of error.

This is the property that makes a software factory possible. Without it, long-horizon agent workflows are random walks with escalating token costs. With it, every iteration moves closer to a validated solution, and the economics become tractable.

## The Inner Loop and the Outer Loop

The feedback loop operates at two scales.

**The inner loop** is within a single task: the agent implements, validates, reads errors, and iterates until the output satisfies its acceptance criteria. Factory.ai describes this as explore-plan-code-verify. StrongDM runs this loop until probabilistic satisfaction exceeds a threshold. The inner loop is what turns a seed into a working implementation.

**The outer loop** is across tasks: knowledge from completed work feeds into future planning, improving seed quality, validation coverage, and agent effectiveness over time. This is Klaassen's Compound step — the novel element in the framework. The outer loop is what turns a sequence of independent tasks into an improving system.

Both loops must function for a factory to work. The inner loop ensures each task converges. The outer loop ensures the factory gets better at producing convergent results.

## The Inflection Point

Before late 2024, long-horizon agentic coding workflows tended to degrade. Models would accumulate small errors, build on incorrect assumptions, and produce code that looked plausible but failed in subtle ways. The feedback loop existed but amplified error instead of correctness.

The shift — driven by improvements in models like Claude 3.5 Sonnet (October 2024) — was that iteration began to reliably improve output. This wasn't a single capability breakthrough; it was the compound effect of better instruction-following, more reliable tool use, and improved error interpretation. The loop started compounding in the right direction.

## See Also

- [Compound Knowledge](compound-knowledge.md) — The outer loop formalized as organizational practice
- [The Seed](seed.md) — What the feedback loop iterates on
- [Validation](validation.md) — What determines whether each iteration improves
- [Tokens as Fuel](tokens-as-fuel.md) — The economic cost of running the loop
