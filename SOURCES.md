# Sources

Annotated bibliography of the four primary sources synthesized in this knowledge base.

---

## StrongDM Software Factory

**URL:** [factory.strongdm.ai](https://factory.strongdm.ai/)

StrongDM's Software Factory is the most radical implementation of the agentic development paradigm: "code must not be written by humans; code must not be reviewed by humans." The site describes principles (deliberate naivete, tokens as fuel), named techniques (digital twin universe, gene transfusion, filesystem-as-memory, semport, pyramid summaries, shift work), and three products — the Attractor pipeline orchestrator, CXDB context store, and StrongDM ID agent identity system. The key innovation is scenario-based validation using probabilistic satisfaction metrics: measuring what fraction of observed trajectories through end-to-end user stories likely satisfy the user. This knowledge base's [Attractor deep-dive](components/attractor/INDEX.md) is built primarily from StrongDM's open-source specifications.

**Key contributions:** Named principles and techniques vocabulary, digital twin universe concept, scenario-based validation, the Attractor pipeline spec.

---

## Compound Engineering

**URL:** [lethain.com/everyinc-compound-engineering](https://lethain.com/everyinc-compound-engineering/)
**Author:** Will Larson

Larson's framework identifies four steps for effective agentic development: plan, work, review, and compound. The first three are variations on known practices; the fourth — having agents summarize learnings into structured formats consulted by future planning phases — is the key innovation. Larson's central insight is that agent output quality depends more on engineering environment quality (codebase maturity, test coverage, CI/CD systems) than on agent sophistication. The framework is operationalized through workflow commands and an AGENTS.md file that instructs agents when and how to execute each step. Notably lightweight: Larson reports implementing the full framework in approximately one hour.

**Key contributions:** The "environment matters more than agent" insight, the Compound step as explicit practice, AGENTS.md as living compound knowledge.

---

## Agent-Native Development

**URL:** [factory.ai/news/build-with-agents](https://www.factory.ai/news/build-with-agents)
**Author:** Factory.ai

Factory.ai's framework starts from the observation that standard development practices are human-centric by design. Agents need different things: precise specifications, small scopes, and objective verification criteria. Their self-check heuristic — "can you identify where the agent begins reading and what artifact proves completion?" — is a practical test for specification quality. The framework includes a risk-tiered automation model (low/medium/high risk actions with graduated autonomy), an explore-plan-code-verify inner loop, and recovery patterns for when agents drift. AGENTS.md appears here too, answering three questions: how to build/test/lint, how the codebase is organized, and what evidence must accompany a pull request.

**Key contributions:** The self-check heuristic for specifications, risk-tiered automation framework, agent-native environment design principles, recovery patterns.

---

## Simon Willison's Review

**URL:** [simonwillison.net/2026/Feb/7/software-factory](https://simonwillison.net/2026/Feb/7/software-factory/)
**Author:** Simon Willison

Willison's assessment of the Software Factory concept provides essential external perspective. He frames the concept as a "dark factory" and identifies the central challenge: "How can you prove that software you are producing works if both the implementation and the tests are being written for you by coding agents?" He calls out the digital twin universe as the most impressive element and raises pointed questions about the $1,000/day token cost model. His overall verdict — "a glimpse of one potential future" — balances genuine interest in the validation patterns with skepticism about economics and competitive dynamics. The review thread and Hacker News discussion generated significant practitioner commentary that extends and challenges the source material.

**Key contributions:** The "dark factory" framing, the central validation question, cost model skepticism, independent critical assessment.
