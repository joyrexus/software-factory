# Specification Discipline

*The self-check heuristic for agent-ready specifications.*

## The Technique

Before handing a task to an agent, apply Factory.ai's self-check heuristic: can you identify (1) where the agent begins reading and (2) the artifact that proves completion? If either answer is unclear, you haven't written a specification — you've written a wish.

This is the practical instantiation of the [Seed](../principles/seed.md) principle. Specification discipline is the skill that separates productive agentic workflows from expensive token-burning exercises.

## Anatomy of a Good Specification

A specification ready for agent consumption includes:

**Starting location.** Not just "the codebase" but which files, which functions, which test suites. "Start in `src/auth/validator.ts`, specifically the `validateToken()` method" gives the agent a concrete entry point.

**Failure signals.** What tells the agent it's on the wrong path? "If you find yourself modifying files outside `src/auth/`, stop and re-read the spec" prevents drift. "The existing tests in `auth.test.ts` must continue to pass" provides a regression guard.

**Boundaries.** Which files should be touched, which should not. Which patterns to follow, which to avoid. Which dependencies are acceptable, which are not.

**Done definition.** An observable artifact that proves completion. "The test `should reject expired tokens` passes" is concrete. "The auth system works better" is not.

**What the spec should not include:** internal algorithms or implementation details. The specification describes *what* and *why*, not *how*. Prescribing the implementation prevents the agent from finding a better solution.

## The Self-Check in Practice

Before submitting a specification, ask:

1. If I handed this to a capable engineer who had never seen this codebase, would they know where to start?
2. Could I verify completion by running a single command or checking a single artifact?
3. Are the boundaries clear enough that the engineer wouldn't need to ask "should I also change X?"

If any answer is no, the specification needs work. The discipline of writing good specs is itself valuable — it forces clarity of thought that benefits human implementers just as much as agent ones.

## Implements

- [The Seed](../principles/seed.md) — Specification discipline produces high-quality seeds
- [Agent-Native Environment](../principles/agent-native-environment.md) — Specs designed for agent consumption

## See Also

- [Shift Work](shift-work.md) — Specification quality determines whether a task can run non-interactively
- [Scenarios Not Tests](scenarios-not-tests.md) — Scenario-level specifications for validation
- [The Seed](../principles/seed.md) — The principle behind specification discipline
