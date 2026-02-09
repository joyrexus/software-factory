# Agent Identity

*Auth/authz for a mixed human-agent workforce.*

## What It Is

An agent identity system provides authentication and authorization for an organization where humans, workload identities (CI pipelines, service accounts), and autonomous agents all operate on the same systems. Without it, agents either run with human credentials (creating audit gaps) or with service accounts (creating permission gaps). Neither is acceptable in a production software factory.

The central challenge is that traditional identity systems assume principals are human. Agents don't authenticate the same way, don't have the same lifecycle, and need permissions scoped differently — by path, by risk tier, by task context. An agent identity system is purpose-built for this mixed workforce.

## Key Properties

An agent identity system for a software factory must be:

- **Entity-type-aware** — Distinguish between human principals, workload principals, and agent principals, with type-appropriate authentication and permission models.
- **Federated** — Integrate with existing identity providers (corporate SSO, cloud IAM) rather than replacing them. Agents get their own identity type within the existing federation.
- **Path-scoped** — Permissions scoped to specific directories, repositories, or resource paths. An agent working on the auth service shouldn't have access to billing infrastructure.
- **Risk-tier-aware** — Permission levels that align with [risk-tiered automation](../../techniques/risk-tiered-automation.md): low-risk actions proceed automatically, medium-risk actions require passing validation, high-risk actions require human approval.
- **Auditable** — Every agent action is logged with full context: who authorized it, what specification drove it, what the agent did, and what the outcome was. Audit trails replace human code review as the accountability mechanism.

## Reference Implementation

StrongDM ID is the reference implementation described in the Software Factory materials, extending StrongDM's existing infrastructure access platform to handle agent principals alongside humans and workloads.

## Files

| File | Description |
|------|-------------|
| [Architecture](architecture.md) | Principal types, federated auth, path-scoped permissions, audit trails |

## See Also

- [Risk-Tiered Automation](../../techniques/risk-tiered-automation.md) — Permission tiers aligned with action risk
- [Validation](../../principles/validation.md) — Audit trails as validation of agent behavior
- [Attractor](../attractor/README.md) — Pipeline orchestrator whose agents need identity
- [Context Store](../context-store/README.md) — Persistent memory that identity controls access to
