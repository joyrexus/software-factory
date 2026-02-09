# Validation

*End-to-end proof of correctness.*

## The Principle

A software factory produces code at machine speed. Without equally fast, equally rigorous validation, it produces *bugs* at machine speed. Validation is the constraint that makes the factory safe — the proof that what was built actually works, tested close to production conditions, using end-to-end scenarios rather than narrow unit tests.

This is, as Willison frames it, potentially the most consequential question in software development right now: how can you prove that software works when both the implementation and the tests are written by agents?

## The Evolution: Tests to Scenarios to Satisfaction

Traditional validation relies on unit tests written by the same developer who wrote the code. In a software factory, this creates a circular dependency — the agent writes both code and tests, and can trivially satisfy narrow test cases without producing correct behavior.

The response is an evolution through three stages:

1. **Tests** — Assertions about specific behavior. Necessary but gameable.
2. **Scenarios** — End-to-end user stories describing observable trajectories through the system. Stored outside the codebase, written independently of implementation, resistant to reward-hacking.
3. **Satisfaction** — A probabilistic metric: what fraction of observed trajectories through scenarios likely satisfy the user? Not a binary pass/fail but a confidence level.

StrongDM's approach stores scenarios as holdout sets — like evaluation datasets in machine learning — that the implementing agent never sees. An independent judge evaluates whether the implementation satisfies the scenario. This separation of concerns is the key insight: the entity that builds must not be the entity that validates.

## Environment as Validator

As Klaassen observes, agent output quality depends more on the engineering environment than on agent sophistication. A codebase with strong types, comprehensive CI, and opinionated linters catches errors that no amount of prompt engineering can prevent. The environment itself becomes a validation layer.

Factory.ai operationalizes this through required evidence: every pull request must demonstrate that tests pass, linting is clean, and diffs are confined to agreed paths. The agent doesn't decide whether its work is good enough — the environment does.

## See Also

- [Scenarios Not Tests](../techniques/scenarios-not-tests.md) — The technique implementing this principle
- [Digital Twin Universe](../techniques/digital-twin-universe.md) — Environment-level validation infrastructure
- [The Seed](seed.md) — Quality of specification determines quality of output to validate
- [The Feedback Loop](feedback-loop.md) — Validation results feeding back into future iterations
- [Attractor](../components/attractor/README.md) — Pipeline orchestrator with goal gates implementing validation checkpoints
