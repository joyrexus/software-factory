# Components

A software factory needs three kinds of infrastructure, each filling a distinct architectural role:

**A pipeline orchestrator** coordinates multi-agent workflows — defining what work to do, in what order, with what validation checkpoints, and how to recover from failure. This is the factory's assembly line.

**A context store** provides persistent, structured memory for agents across sessions — an append-only DAG of artifacts, decisions, and learnings that agents query during planning and implementation. This is the factory's institutional memory.

**An agent identity system** handles authentication and authorization for a mixed workforce of humans, workload identities, and autonomous agents — with path-scoped permissions and audit trails that replace human code review as the accountability mechanism. This is the factory's access control.

These are architectural roles, not product prescriptions. Any software factory must fill them; the specific implementations will vary.

## Components

| Component | Description | Depth |
|-----------|-------------|-------|
| [Attractor](attractor/INDEX.md) | DOT-based pipeline orchestrator for multi-agent workflows | Full deep-dive (56 files) |
| [Context Store](context-store/INDEX.md) | Persistent structured memory for agents (DAG, deduplication, queries) | Architectural stub |
| [Agent Identity](agent-identity/INDEX.md) | Auth/authz for mixed human-agent workforce | Architectural stub |

The Attractor component is a comprehensive analysis of StrongDM's open-source pipeline orchestrator specification. The Context Store and Agent Identity sections are architectural stubs describing the role each component fills and the key design properties, based on StrongDM's CXDB and StrongDM ID descriptions.
