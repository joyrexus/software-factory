# So You Want to Build a Software Factory

## The Moment

In late 2024, something shifted. Long-horizon agentic coding workflows — the kind where an LLM writes code, runs tests, reads errors, and iterates — crossed an inflection point. Models began compounding correctness instead of error. A coding agent could work for thirty minutes, an hour, longer, and the code kept getting *better* rather than drifting into hallucinated nonsense. Multiple organizations, working independently, arrived at the same conclusion: the "dark factory" — where agents produce code, agents review code, and humans architect systems and validate outcomes — was no longer science fiction. It was an engineering challenge.

This knowledge base is for software architects who've moved past "should we use AI for coding?" and are now asking a harder question: *what does an organization look like when agents do most of the coding?*

This knowledge base explores and assesses the software factory concept — it does not advocate for it. The goal is to give organizations considering agentic engineering a clear-eyed map of the emerging patterns, open questions, and trade-offs involved.

## [The Paradigm](PARADIGM.md)

Three intellectual threads — compound engineering, agent-native development, and the software factory vision — are converging on the same destination from different starting points.

## The Formula

The shared insight, distilled across all three lineages:

> **Seed** &rarr; **Validation** &rarr; **Feedback Loop**. **Tokens** are the fuel.

Every specification is a **seed** — the quality of the starting input determines the ceiling of the output. Structured planning, specification self-check heuristics, and scenario-driven specs all converge here. Every run must end with **validation** — proof the output works, close to production, using end-to-end scenarios rather than narrow unit tests. Every validated output feeds back as input for the next iteration, creating a **feedback loop** that compounds correctness. And the entire system runs on **tokens** — a variable-cost, predictable-output fuel source that replaces fixed-cost engineering headcount.

If this formula holds, the question becomes what infrastructure you need to make it reliable.

## [The Honest Questions](QUESTIONS.md)

Essential design constraints disguised as skepticism — on proving agent-written code works, on economics, and on competitive moats.

## [The Conversation](CONVERSATION.md)

An active, evolving conversation in the engineering community — practitioner perspectives, counterarguments, and real-world experience reports.
