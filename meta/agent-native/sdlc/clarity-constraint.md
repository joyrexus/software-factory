# The Clarity Constraint

*Six claims about why specs are the central artifact — and what they add up to.*

---

Practitioners building with coding agents tend to converge on similar language: specs matter, intent must be explicit, the definition of done should precede the first line of code. These observations are easy to treat as conventional wisdom — sensible advice that doesn't need much examination. But the claims are not all saying the same thing, even when they gesture in the same direction.

This document sorts them by the actual argument each is making, assesses what each one really claims, and synthesizes what they add up to. The result is not a list of best practices but a single coherent position about why the [Behavioral Control Plane](../../../GLOSSARY.md#behavioral-control-plane) is structurally necessary rather than merely useful.

---

## The Claims

### Claim 1: The bottleneck is always clarity, not capability

> "The scarce resource has always been clarity: knowing what to build, defining boundaries, making implicit knowledge explicit, recognizing failure modes before they become production incidents."

This is the foundational claim, and it is not primarily an argument about AI. It is an argument about software development itself. The constraint on building software well has never been the ability to write code — it has been the ability to specify what the code should do. Skilled engineers have always spent more time achieving clarity about requirements, boundaries, and edge cases than actually writing implementations.

AI removes the implementation constraint. Generating code becomes cheap. But generating *wrong* code remains as consequential as ever — more so, because wrong code now arrives faster and in greater volume. The clarity constraint does not go away when implementation accelerates; it becomes the dominant bottleneck by elimination.

The rest of the claims in this document are downstream of this one.

### Claim 2: Specs shift when failure is discovered

> "Make the definition of 'done' explicit before any code is written, and spot failure modes at the plan level instead of in generated output."

This is a temporal argument. The claim is not just that explicitness is good — it is that the *time* at which clarity is achieved determines its cost. A failure mode identified in a spec costs a line edit. The same failure mode identified in generated output costs a debugging session, a revert, and possibly a production incident. The spec is the cheapest place to be wrong.

This maps directly to the [Seed Quality](../../../GLOSSARY.md#seed-quality) principle: errors compound upstream. A mistake in the plan cascades into hundreds of lines of bad code. A mistake caught at the spec stage costs almost nothing. The spec is upstream of everything — it is the point where the cost of error is lowest and the leverage over subsequent work is highest.

### Claim 3: The division of labor between humans and agents

> "Developers define what should be built and delegate the execution to their coding agents. You focus on what should be built; agents handle how it's executed."

This is a role-separation claim. Humans hold the "what" and "why"; agents hold the "how." The insight is not novel — every sufficiently abstract system draws this distinction. What is different in the agent context is the consequence of leaving the division implicit.

An agent with no spec will make "what" decisions constantly. Intent gaps do not pause execution; they get filled with implementation choices nobody authorized. The agent is not being disobedient — it is doing exactly what it was asked to do, under conditions of insufficient specification. The spec is the mechanism by which the division of labor is enforced structurally rather than just intended conceptually. Without it, the boundary between human judgment and agent execution collapses into the implementation.

### Claim 4: The spec is bidirectional

> "Work should start from a spec that evolves as you make progress. As code changes, your coding agent reads from and updates the spec so every human and agent stays aligned."

> "The spec isn't a human artifact or an agent artifact. Both sides read from it and write to it."

Traditional specs are write-once human artifacts: authored before development, consulted occasionally during implementation, abandoned as the code diverges from the original document. These claims describe a different artifact entirely. Information flows in both directions: humans write intent; agents write back what was actually built, what decisions were made, what constraints were discovered during implementation. The spec stays current not because someone is maintaining documentation but because updating the spec is part of executing the work.

The deeper structural claim is that the spec is the *synchronization substrate* between human intent and agent execution. It is not documentation layered on top of code; it is the medium through which both sides stay coordinated. Keeping spec and implementation aligned — what the [Spec Graph](../../../GLOSSARY.md#spec-graph) approach calls spec-code drift — is a form of entropy that accumulates whenever this bidirectional discipline breaks down.

### Claim 5: Multi-agent coherence requires a shared spec

> "If you don't make the goal, constraints, and success criteria explicit, each agent will interpret the task in isolation. That's how you end up with five implementations that technically work and still don't fit together."

The previous claims apply to single-agent development. This one is specific to parallelism and is distinct in kind, not just degree. When one agent works without a spec, that agent makes unilateral decisions. When five agents work in parallel without a shared spec, each makes an independent set of unilateral decisions — with high probability of contradictory assumptions, incompatible interfaces, and implementations that satisfy local correctness without satisfying system coherence.

A missing spec in a multi-agent context is not a multiplier of productivity; it is a multiplier of inconsistency. Parallelism amplifies whatever coordination mechanism is in place. A shared [Executable Spec](../../../GLOSSARY.md#executable-spec) is the mechanism that makes parallel agents productive rather than divergent.

### Claim 6: A good spec has two components — human judgment and system context

> "Specs that work make two things explicit: human judgment about what success means and which constraints matter, and system context about the patterns, dependencies, and failure modes the code needs to respect."

> "Treat the spec as the system of record, not an annotation layered on top of generated code. The spec is where intent lives, where work is divided, and where correctness is evaluated."

This is the most technically precise claim in the set, and the one most tooling gets wrong. Most spec-driven approaches treat spec-writing as a purely human task: an engineer or product manager documents requirements, an agent implements them. That model captures only half the spec.

A complete spec requires two distinct inputs:

**Human judgment** — what success looks like, which constraints are non-negotiable, where the scope boundaries are, what the acceptable failure modes are. These are things only the engineer or PM can determine; no amount of codebase analysis produces them.

**System context** — existing patterns, known failure modes, current API contracts, the tribal knowledge that experienced engineers carry in their heads but nowhere in the codebase. This is the half that human-only spec authoring tends to miss, because it requires the kind of comprehensive codebase awareness that is difficult to maintain manually.

The gap between these two halves is where AI-assisted spec generation becomes genuinely useful. An agent with deep codebase access can surface system context that a human writing a spec from memory would miss or misremember. The human contributes judgment; the agent contributes grounding. A spec written without codebase context is aspirational; a spec grounded in system context is executable.

The "spec as system of record" formulation follows from this: if the spec is where correctness is evaluated, it must contain enough system context to make that evaluation meaningful — not just what was intended, but what the system actually constrains and what patterns the implementation must respect.

---

## The Synthesis

Read together, these claims describe a single coherent position:

Clarity about what to build has always been the binding constraint in software development. AI removes the implementation constraint while leaving the clarity constraint in place — and makes the consequences of insufficient clarity more severe, because agents execute at a speed and scale where ambiguity amplifies rather than self-corrects.

The spec is the artifact that operationalizes clarity. But a spec that works is not a human-authored requirements document handed to an agent and then forgotten. It is a bidirectional, living artifact that externalizes human judgment, encodes system context, stays current as implementation reveals new information, and serves as the shared substrate that keeps parallel agents coherent.

The deeper insight: **the spec is not upstream of the work — it is the work, at a level of abstraction that scales with agent capability.** Writing a good spec is not a preliminary step before engineering begins. It is engineering. The code is the compilation artifact.

---

## What This Means for the Framework

The six claims collectively justify the Behavioral Control Plane as a structural necessity. Mapped to the architecture described in [spec-driven-architecture.md](spec-driven-architecture.md):

The [Spec Graph](../../../GLOSSARY.md#spec-graph) is the system-context half of a complete spec — it externalizes the patterns, dependencies, and failure modes that traditionally live in engineers' heads. The [Executable Spec](../../../GLOSSARY.md#executable-spec) YAML captures the human-judgment half — invariants, effects, and success criteria that an agent cannot infer from code alone.

The gap the claims point to — tribal knowledge teams carry informally — is the hardest element to address in a spec authoring workflow. The [`decisions:` field](pilot-guide.md#spec-file-format) in the executable spec format is the right location for it: lightweight records of context, decision, and consequences that preserve the *why* behind structural choices. Eliciting this material (what existing patterns apply here? what are the known failure modes in this domain? what invariants does an adjacent spec impose?) is where an agent with deep codebase access is most useful. The agent's contribution is not writing requirements — it is surfacing the system context that makes human-authored requirements actually grounded.

---

## See Also

- [spec-driven-architecture.md](spec-driven-architecture.md) — the four-layer architecture these claims justify
- [pilot-guide.md](pilot-guide.md) — concrete spec format with `decisions:` field for capturing tribal knowledge
- [Specification Discipline](../../../techniques/specification-discipline.md) — the self-check heuristic for writing agent-ready specs
- [Seed Quality](../../../GLOSSARY.md#seed-quality) — the principle that spec quality sets the ceiling on agent output
- [Validation](../../../principles/validation.md) — independent behavioral verification that the spec enables
- [Behavioral Control Plane](../../../GLOSSARY.md#behavioral-control-plane) — the governance layer the spec graph instantiates
- [Agent-Native Engineering](../README.md) — the parent directory framing what agents need to perform well
