# Project Instructions

This is a hierarchical markdown knowledge base about building a software factory — an engineering organization where coding agents produce and validate software under human architectural direction. It synthesizes ideas from four sources into an integrated, navigable reference.

There is no code — only interconnected markdown files organized as a navigable hierarchy.

## Sources

- **[StrongDM Software Factory](https://factory.strongdm.ai/)** — Principles, techniques, and the Attractor pipeline orchestrator
- **[Compound Engineering](https://lethain.com/everyinc-compound-engineering/)** (Will Larson) — Plan/work/review/compound framework
- **[Agent-Native Development](https://www.factory.ai/news/build-with-agents)** (Factory.ai) — Specification discipline and environment design
- **[Simon Willison's Review](https://simonwillison.net/2026/Feb/7/software-factory/)** — Critical assessment and the "dark factory" framing

## Project Layout

```
software-factory/
├── README.md                          — "So you want to build a software factory"
├── CLAUDE.md                          — Project instructions (this file)
├── INDEX.md                           — Master table of contents
├── SOURCES.md                         — Annotated bibliography
│
├── principles/                        — Beliefs that make a software factory make sense
│   ├── INDEX.md
│   ├── seed.md                        — Specification as starting point
│   ├── validation.md                  — End-to-end proof of correctness
│   ├── feedback-loop.md               — Compounding correctness
│   ├── tokens-as-fuel.md              — Economics of agent labor
│   ├── deliberate-naivete.md          — Questioning inherited constraints
│   ├── compound-knowledge.md          — Organizational learning at machine speed
│   └── agent-native-environment.md    — Designing for non-human workers
│
├── techniques/                        — Repeatable patterns you can adopt
│   ├── INDEX.md
│   ├── digital-twin-universe.md       — Cloning external dependencies for validation
│   ├── gene-transfusion.md            — Cross-codebase pattern transfer
│   ├── filesystem-as-memory.md        — Disk as agent cognition substrate
│   ├── shift-work.md                  — Interactive vs. non-interactive modes
│   ├── semport.md                     — Semantically-aware code migration
│   ├── pyramid-summaries.md           — Reversible multi-level context compression
│   ├── scenarios-not-tests.md         — Holdout-set validation replacing unit tests
│   ├── specification-discipline.md    — The self-check heuristic for specs
│   └── risk-tiered-automation.md      — Graduated autonomy levels
│
├── components/                        — Infrastructure a software factory needs
│   ├── INDEX.md
│   ├── attractor/                     — Pipeline orchestrator (56-file deep-dive)
│   │   ├── README.md
│   │   ├── INDEX.md
│   │   └── 00-overview/ ... 07-future-extensions/
│   ├── context-store/                 — Persistent structured memory for agents
│   │   ├── INDEX.md
│   │   └── architecture.md
│   └── agent-identity/               — Auth/authz for mixed human-agent workforce
│       ├── INDEX.md
│       └── architecture.md
│
└── implementations/                   — Known implementations
    ├── INDEX.md
    └── kilroy.md                      — Local-first software factory CLI
```

Every directory contains an `INDEX.md` that lists and describes its files. The root `INDEX.md` links to all section indexes.

## Writing Guidelines

### Concept-First, Not Vendor-First

Write about principles and techniques as general concepts, not as features of a specific product. StrongDM, Factory.ai, and Larson are *sources* of ideas, not the subjects. Good: "A specification must identify where the agent begins reading and what artifact proves completion." Bad: "Factory.ai says specifications should..."

### Attribution

When a specific source contributes a key insight, attribute naturally: "As Larson observes, agent output quality depends more on the engineering environment than on agent sophistication." Use source names for provenance, not as subjects of sentences.

### Tone

Authoritative but not dogmatic. These are emerging patterns, not settled science. Use language like "the evidence suggests" or "practitioners report" rather than "you must" or "the correct approach is." Acknowledge open questions — Willison's skepticism is a feature, not a problem to overcome.

### Cross-References

Every file should include a "See Also" section linking to related content. Principles link to techniques that implement them. Techniques link to principles they embody and components that use them. Components link back to both.

## Filesystem Technique

This hierarchy follows StrongDM's own [filesystem technique](https://factory.strongdm.ai/techniques/filesystem):

- **Directories with meaningful names** reflect conceptual hierarchy
- **Markdown INDEX files** provide navigable overviews at each level
- **On-disk state** serves as a practical memory substrate — the filesystem itself is the knowledge graph

The structure is designed to be **genrefied** — reorganized as understanding deepens, following the library-science principle of restructuring information to optimize future retrieval.

## Living Document

This is not a finished artifact. It's a starting point intended to be iterated upon:

- Sections can be expanded, restructured, or split as specific areas require deeper analysis
- Implementation decisions, spikes, and learnings should be captured back into this hierarchy
- The components section is expected to grow as more factory infrastructure is documented

## Internal Consistency

When modifying any file in this project, you must:

1. **Read related documents first.** Before editing a file, read the files it cross-references (check its "See Also" section) and any files that reference it. Understand the surrounding context so your changes don't contradict or duplicate existing content.

2. **Update cross-references.** If you add, rename, move, or remove a file, update every document that links to it. Use `Grep` to find all references to the affected filename.

3. **Update INDEX files.** Every content file must be listed in its section's `INDEX.md`. If you add a new file, add it to the section INDEX. If you add a new section, add it to the root `INDEX.md` as well.

4. **Maintain the root INDEX.** The root `INDEX.md` is the authoritative table of contents. Any structural change (new sections, renamed sections, significant scope changes to a section) must be reflected there.

5. **Preserve terminology.** Use terms consistently as defined in [components/attractor/05-reference/glossary.md](components/attractor/05-reference/glossary.md). If you introduce a new term, add it to the glossary.

6. **No orphaned files.** Every markdown file must be reachable from the root INDEX through the section INDEX chain. After any structural change, verify this.
