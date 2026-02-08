# Semport

*Semantically-aware code migration between languages and frameworks.*

## The Technique

Semport (semantic + port) is the practice of migrating code between languages or frameworks by preserving *intent* rather than *syntax*. Instead of line-by-line transpilation — which produces code that technically works but feels foreign in the target language — semport produces code that a native speaker of the target language would recognize as idiomatic.

The distinction matters because syntactic transpilation carries the source language's assumptions into the target. A Python function transpiled to Go will use maps where a Go developer would use structs, return tuples where a Go developer would use named types, and handle errors where a Go developer would propagate them. Semport captures the *what* and the *why*, then re-expresses them in the target language's idiom.

## When to Use Semport

- **Framework migrations**: Moving from one web framework to another within the same language, preserving business logic while adopting new patterns
- **Language migrations**: Porting a service from one language to another, adapting to the target language's type system, error handling, and concurrency model
- **Consolidation**: Merging functionality from multiple services (possibly in different languages) into a unified implementation

## The Agent Advantage

Semport is a task that agents excel at relative to humans. A human developer migrating between languages must hold two language's idioms in mind simultaneously, constantly translating patterns. An agent can read the source, understand the intent, and produce target-native code in a single pass — then iterate based on whether the output compiles, passes tests, and satisfies scenarios.

The key is specification quality: the semport instruction must specify *which behaviors to preserve*, not which lines to translate. "This function validates OAuth tokens and returns the user's roles" is a better semport spec than "translate this function from Python to Go."

## Implements

- [Deliberate Naivete](../principles/deliberate-naivete.md) — Questioning whether code must stay in its original language
- [Tokens as Fuel](../principles/tokens-as-fuel.md) — Migrations that would take human engineers weeks become tractable token expenditures

## See Also

- [Gene Transfusion](gene-transfusion.md) — Related technique for cross-codebase pattern transfer
- [Specification Discipline](specification-discipline.md) — Writing migration specs that preserve intent
