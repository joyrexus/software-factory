# Tokens as Fuel

*Economics of agent labor.*

## The Principle

In a software factory, tokens replace engineering hours as the primary unit of production cost. This is a fundamental shift in how engineering work is budgeted, planned, and measured. A developer costs a fixed amount per month regardless of output. Tokens cost a variable amount tied directly to the volume and complexity of work produced. The economics are inverted: predictable output, variable cost.

This reframing has consequences. Planning shifts from "how many engineers do we need?" to "how many tokens will this require?" Optimization shifts from developer productivity to token efficiency. And the cost model introduces new failure modes — a poorly specified task can burn thousands of dollars in tokens while producing nothing useful.

## The Current Economics

StrongDM reports approximately $1,000/day in token costs per engineer equivalent. At $20,000/month overhead, this is a significant investment that only makes sense if agent output velocity substantially exceeds human engineer velocity.

Willison raises pointed questions about this model. At $20,000/month per engineer in token costs alone — before salaries, infrastructure, or tooling — the value proposition depends entirely on whether agent-produced output justifies the spend. He contrasts this with his own experimentation using consumer-tier plans at $200/month, questioning whether the techniques described here actually require the reported token volumes.

The honest answer is that token economics are moving fast. What costs $1,000/day in early 2025 may cost $100/day by late 2025 and $10/day by 2026. The principle matters more than the current numbers: tokens are a variable cost that should be planned and optimized like any other resource, not treated as a sunk cost or an unlimited budget.

## Planning Implications

Treating tokens as fuel means:

- **Budget by task complexity**, not by team size. A complex refactoring consumes more tokens than a simple bug fix, regardless of how many agents work on it.
- **Optimize seed quality** to reduce wasted iterations. A well-specified task converges in fewer loops, consuming fewer tokens. The cheapest tokens are the ones you don't spend.
- **Monitor token efficiency** as a leading indicator. Rising token costs per task with stable output quality signals degrading specifications or environmental problems.
- **Plan for cost curves**, not current prices. Build systems that work at current token prices but become dramatically more valuable as prices fall.

## See Also

- [The Seed](seed.md) — Seed quality directly affects token consumption
- [Shift Work](../techniques/shift-work.md) — Structuring work to optimize token usage across interactive and non-interactive modes
- [The Feedback Loop](feedback-loop.md) — Each loop iteration consumes tokens; convergence speed determines total cost
- [Deliberate Naivete](deliberate-naivete.md) — Willingness to spend tokens on approaches that were previously uneconomical
