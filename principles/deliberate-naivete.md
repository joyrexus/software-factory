# Deliberate Naivete

*Questioning inherited constraints.*

## The Principle

Every engineering practice exists because of constraints that may no longer apply. Code review exists because humans make mistakes that other humans can catch. Sprint planning exists because human attention is scarce and must be allocated. Test pyramids exist because integration tests were slow and expensive.

Deliberate naivete is the discipline of asking "why am I doing this?" about every inherited practice — not out of arrogance, but because the constraints that justified those practices may have evaporated. When creating a behavioral clone of a significant SaaS application shifts from "always possible but never economically feasible" to "routine," the entire validation strategy changes. When generating code becomes nearly free, the entire build-vs-buy calculus changes.

StrongDM frames this as a koan: "Why am I doing this?" The answer often reveals that a practice serves a constraint that no longer exists. The practice persists out of habit, not necessity.

## What Changes

Factory.ai makes the observation explicit: standard development practices are human-centric by design. Stand-ups, kanban boards, code review rituals, branching strategies — these are optimizations for human cognition, human communication, and human error patterns. Agents have different failure modes, different throughput characteristics, and different needs.

This doesn't mean every human practice should be discarded. It means each one should be re-evaluated against the question: does this serve the goal (working software) or does this serve the old constraint (human limitations)?

Some practices survive the re-evaluation. Version control still matters. Separation of concerns still matters. Testing still matters — though the *form* of testing may change radically. Other practices dissolve: line-by-line code review by humans becomes impractical at factory output volumes and unnecessary if scenario-based validation provides stronger guarantees.

## The Risk

The risk of deliberate naivete is obvious: discarding a practice whose purpose you've misidentified. The discipline requires understanding *why* something exists before deciding it's obsolete. "We've always done it this way" is not a reason to keep doing it. But "we've always done it this way" is also not, by itself, a reason to stop.

Willison's skepticism serves as a useful check here. When practitioners claim a practice is obsolete, the burden of proof is on them to demonstrate that the new approach provides equivalent or better guarantees.

## See Also

- [Agent-Native Environment](agent-native-environment.md) — Designing for the new constraints instead of the old ones
- [Digital Twin Universe](../techniques/digital-twin-universe.md) — An example of something previously impractical becoming routine
- [Semport](../techniques/semport.md) — Questioning whether code must stay in its original language
- [Tokens as Fuel](tokens-as-fuel.md) — The economic shift that makes naivete productive
