# The Seed

*Specification as starting point.*

## The Principle

Every agentic workflow begins with a seed — the specification that tells the agent what to build. The quality of that seed determines the ceiling of the output. A vague seed produces vague results. A precise seed, with clear starting points, success criteria, and boundaries, produces output that can be validated and iterated upon.

This is not a new insight for software engineering. What's new is how much it matters when your implementer is a language model. Human developers compensate for underspecified requirements through hallway conversations, tribal knowledge, and intuition built from years on the team. Agents have none of that. The seed is all they get.

## Convergence Across Sources

All three lineages converge on this point, though they frame it differently.

As Klaassen observes, the Plan step — decoupling research from implementation by consulting specs, PRs, and RFCs before coding begins — is what separates productive agentic workflows from expensive random walks. The agent that plans first writes code that survives review.

Factory.ai makes the specification discipline explicit with a self-check heuristic: can you identify where the agent begins reading, and can you identify the artifact that proves completion? If either answer is unclear, you haven't written a specification — you've written a wish. A good seed includes a starting location, failure signals, boundaries, and a done definition, without prescribing internal algorithms.

StrongDM takes this furthest. Their scenarios — end-to-end user stories stored outside the codebase — serve as both specification and validation target. The seed isn't just "build X," it's "build X such that these observable behaviors emerge."

## What Makes a Good Seed

A specification ready for agent consumption answers five questions:

1. **Where does the agent start?** Which files, which function, which test suite?
2. **What does done look like?** An observable artifact — a passing test, a rendered page, a successful API call.
3. **What are the boundaries?** Which files should be touched, which should not.
4. **What signals failure?** Not just "tests pass" but which tests, which error messages indicate the wrong path.
5. **What prior knowledge applies?** Links to relevant specs, PRs, or documentation the agent should consult before coding.

The discipline required to write good seeds is itself valuable. It forces the specification author to think precisely about what they want — a practice that benefits human implementers just as much as agent ones.

## See Also

- [Specification Discipline](../techniques/specification-discipline.md) — The self-check heuristic in practice
- [Validation](validation.md) — What happens after the seed germinates
- [Compound Knowledge](compound-knowledge.md) — How seeds improve over time through accumulated learnings
- [Scenarios Not Tests](../techniques/scenarios-not-tests.md) — Seeds as end-to-end scenarios
