# Shift Work

*Separating interactive from non-interactive agent modes.*

## The Technique

Not all agent work requires human presence. Shift work separates tasks into two modes: **interactive**, where the agent needs human input, feedback, or judgment calls; and **non-interactive**, where the specification is complete enough for the agent to work autonomously.

The distinction matters for both economics and workflow. Interactive work happens during working hours — the human and agent collaborate in real time, iterating on specifications, reviewing partial results, making design decisions. Non-interactive work happens on its own schedule — overnight, over weekends, during meetings — consuming tokens to produce validated output without human attention.

## How to Split Work

A task is ready for non-interactive mode when:

- The specification passes the [self-check heuristic](specification-discipline.md): you can identify where the agent starts and what artifact proves completion
- Validation is automated: tests, linters, CI, or scenarios can evaluate the output without human judgment
- The scope is bounded: the agent knows which files to touch and which to leave alone
- Failure is recoverable: if the agent produces bad output, you can discard it and try again without damage

A task requires interactive mode when:

- Design decisions remain open: the agent needs to ask "should I use approach A or B?"
- The specification is exploratory: you're using the agent to understand the problem, not yet to solve it
- Validation requires human judgment: visual design, UX decisions, architectural taste
- The stakes are high: changes to production infrastructure, security-critical code, irreversible operations

## Economic Impact

Shift work directly affects token economics. Non-interactive work can run during off-peak hours, can be parallelized across multiple agents, and can be retried without wasting human attention. Interactive work is bounded by human availability but benefits from the agent's ability to explore options faster than a human could alone.

The most token-efficient organizations maximize the ratio of non-interactive to interactive work — not by eliminating collaboration, but by front-loading interactive work (specification, design decisions) so that implementation can proceed non-interactively.

## Implements

- [Tokens as Fuel](../principles/tokens-as-fuel.md) — Shift work optimizes token consumption across time
- [The Seed](../principles/seed.md) — Non-interactive work requires complete seeds; interactive work produces them

## See Also

- [Specification Discipline](specification-discipline.md) — The skill that determines whether a task can go non-interactive
- [Risk-Tiered Automation](risk-tiered-automation.md) — Determines which non-interactive tasks need additional safeguards
