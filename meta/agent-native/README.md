# Agent-Native Engineering in Practice

The [agent-native environment](../../principles/agent-native-environment.md) principle established what agents need: precise specifications, objective verification, machine-readable context, and bounded scopes. Practitioners are now building this at scale through background and cloud agents running in complete, sandboxed development environments — shifting agent-native engineering from aspiration to operational reality.

The key emerging insight from practice is that effective background agents require two things: (1) **complete development environments** that mirror what engineers have — sandboxed VMs with full toolchains, databases, and CI access — enabling end-to-end verifiability without human intervention; and (2) a **rich context layer** giving agents access to telemetry, codebase search, internal APIs, architectural docs, feature flags, and specifications. Organizations that invest in these two capabilities report agents producing 30% or more of merged pull requests within months, while those that deploy agents without this infrastructure discover that model capability alone is insufficient.

---

| File | Description |
|------|-------------|
| [cloud-agents.md](cloud-agents.md) | Cloud agents and the background revolution — delegation, task classification, and organizational effects |
| [case-studies.md](case-studies.md) | Building in-house: Ramp and Stripe — convergent architectural requirements at scale |
| [maturity-model.md](maturity-model.md) | Agent Readiness Model — maturity levels and technical pillars |
