# Risk-Tiered Automation

*Graduated autonomy levels for agent actions.*

## The Technique

Not all agent actions carry the same risk. File edits are low-risk — easily reviewed, easily reverted. Dependency upgrades are medium-risk — automated tests catch most problems, but subtle breakage is possible. Production deployments are high-risk — consequences are immediate and potentially severe. Risk-tiered automation assigns graduated autonomy based on the blast radius of each action.

Even the most aggressive software factories have boundaries. The principle isn't "automate everything" — it's "automate at the level of confidence your validation infrastructure can support."

## The Three Tiers

Factory.ai's framework defines three tiers:

**Low risk — Automate fully.** File edits, formatting, local test runs, linting. These actions are bounded, reversible, and validated by existing infrastructure. The agent should not need permission to run `npm test` or format a file.

**Medium risk — Automate with confidence.** Commits, dependency bumps, schema dry-runs, branch creation. These actions have broader scope but are still recoverable. The agent can proceed if existing validation (CI, type checking, test suites) passes. Confidence is earned through the environment, not granted by default.

**High risk — Require human confirmation.** Destructive scripts, production data access, sensitive directory modifications, deployment triggers, irreversible operations. No amount of automated validation replaces human judgment for these actions. The agent should surface the action, explain what it intends to do, and wait for explicit approval.

## Calibrating the Tiers

The right tier for an action depends on your validation infrastructure, not on a universal rule. An organization with comprehensive CI, staging environments, and automated rollback can safely automate more at the medium tier. An organization with minimal test coverage should keep more actions at the high tier until their validation catches up.

The tiers should evolve as confidence grows. What starts as high-risk (deploying a new service) becomes medium-risk once the deployment pipeline has been exercised dozens of times without incident. What starts as medium-risk (dependency updates) might be promoted to low-risk once automated vulnerability scanning is in place.

## Implements

- [Agent-Native Environment](../principles/agent-native-environment.md) — Risk tiers are part of the environment's guardrails
- [Validation](../principles/validation.md) — The validation infrastructure determines what can be safely automated

## See Also

- [Shift Work](shift-work.md) — Non-interactive work requires well-calibrated risk tiers
- [Specification Discipline](specification-discipline.md) — Specs should declare which risk tier the task operates in
- [Attractor: Human-in-the-Loop](../components/attractor/01-pipeline-orchestrator/human-in-the-loop.md) — Pipeline-level implementation of human confirmation gates
