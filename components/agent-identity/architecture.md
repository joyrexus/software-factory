# Agent Identity Architecture

*Principal types, federated auth, path-scoped permissions, and audit trails.*

## Principal Types

An agent identity system recognizes three types of principals:

**Human principals** — Engineers, architects, managers. Authenticate through corporate SSO. Hold broad permissions scoped by role. Make design decisions, approve high-risk actions, review audit trails.

**Workload principals** — CI/CD pipelines, service accounts, scheduled jobs. Authenticate through cloud IAM or service tokens. Hold narrowly-scoped permissions for specific operations. Predictable behavior, no autonomy.

**Agent principals** — Autonomous coding agents executing tasks. Authenticate through delegated credentials tied to a specific task and specification. Hold permissions scoped by path, risk tier, and task context. Autonomous within their scope, escalate outside it.

The key distinction between workload and agent principals is autonomy. A CI pipeline runs the same commands every time. An agent makes decisions — which files to modify, which approach to take, how to respond to errors. Agent identity must account for this autonomy while constraining it.

## Federated Authentication

Agent identity integrates with existing identity infrastructure rather than replacing it. An agent principal is issued credentials that:

- Trace back to the human who authorized the task
- Are scoped to the specific repository, branch, or directory the task targets
- Expire when the task completes (or after a time limit)
- Can be revoked by the authorizing human at any time

Federation means the organization doesn't need a separate identity system for agents. Agents are a new principal type within the existing identity federation, with type-appropriate authentication flows.

## Path-Scoped Permissions

Agent permissions are scoped to filesystem paths, API endpoints, or resource identifiers. An agent working on `src/auth/` has read access to the full repository (for context) but write access only to `src/auth/` and its test files. This prevents agent drift from causing damage outside the intended scope.

Path scoping aligns with [specification discipline](../../techniques/specification-discipline.md): the specification declares which paths the agent should modify, and the identity system enforces those boundaries. If the agent attempts to write outside its declared scope, the operation fails and the attempt is logged.

## Audit Trails

In a software factory, audit trails replace human code review as the primary accountability mechanism. Every agent action is logged with:

- **Principal identity** — Which agent, authorized by which human, for which task
- **Specification reference** — The seed that drove the action
- **Action detail** — What files were modified, what commands were run, what APIs were called
- **Outcome** — Whether the action succeeded, and what validation results it produced
- **Context** — The agent's reasoning at the time of the action (extracted from the agent's internal chain of thought)

This creates a complete, searchable, machine-readable history of every change the factory produces — more detailed and more reliable than human commit messages and PR descriptions.

## See Also

- [Agent Identity Overview](INDEX.md) — What agent identity is and why you need it
- [Risk-Tiered Automation](../../techniques/risk-tiered-automation.md) — How permission tiers map to risk levels
- [Attractor: Human-in-the-Loop](../attractor/01-pipeline-orchestrator/human-in-the-loop.md) — Pipeline gates that invoke human approval
