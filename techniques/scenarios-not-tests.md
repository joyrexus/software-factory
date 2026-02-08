# Scenarios Not Tests

*Holdout-set validation replacing narrow unit tests.*

## The Technique

Replace narrow, implementation-coupled unit tests with end-to-end scenarios — user stories that describe observable trajectories through the system from the outside. Store scenarios independently of the codebase, evaluate them with an independent judge, and measure satisfaction probabilistically rather than as binary pass/fail.

The problem with traditional tests in a software factory is circular validation: the agent writes the implementation and the tests, and can trivially satisfy narrow assertions without producing correct behavior. A test that checks "function returns 200" tells you nothing about whether the feature actually works for users.

Scenarios break this circularity. They describe what a user would observe — "a new user can sign up, receive a confirmation email, and log in with their credentials" — without specifying implementation details. The implementing agent never sees the scenarios; an independent judge evaluates whether the implementation satisfies them.

## Scenarios as Holdout Sets

The analogy to machine learning is deliberate. In ML, you never evaluate a model on its training data — you use a held-out evaluation set that the model hasn't seen. Scenarios serve the same function: they're the evaluation set for agent-produced code.

This is what Willison identifies as the most impressive element of the Software Factory concept. The approach treats software validation as an evaluation problem, not a verification problem. The question shifts from "does this code match this specification?" to "does this system, when exercised through realistic user journeys, produce satisfactory outcomes?"

## Satisfaction as Metric

StrongDM introduces satisfaction as a probabilistic metric: what fraction of observed trajectories through scenarios likely satisfy the user? This is a more nuanced measure than binary pass/fail. A system might satisfy 95% of login scenarios but only 60% of complex admin workflows — that tells you where to invest improvement effort.

The satisfaction metric also handles ambiguity gracefully. Real user scenarios have fuzzy boundaries — "the page loads quickly" or "the error message is helpful" — that binary tests can't capture. Probabilistic satisfaction, evaluated by an LLM-as-judge, can assess these qualitative criteria.

## Implements

- [Validation](../principles/validation.md) — Scenarios are the primary validation mechanism
- [The Feedback Loop](../principles/feedback-loop.md) — Satisfaction scores drive iterative improvement

## See Also

- [Digital Twin Universe](digital-twin-universe.md) — Infrastructure that enables high-volume scenario execution
- [Specification Discipline](specification-discipline.md) — Writing scenarios that are precise enough to evaluate
- [Attractor: Goal Gates](../components/attractor/01-pipeline-orchestrator/goal-gates.md) — Pipeline checkpoints that enforce scenario satisfaction
