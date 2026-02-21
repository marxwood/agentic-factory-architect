# Glossary

Terminology used throughout the Agentic Factory Architect framework.

---

**Agentic Factory**
A coordinated system of agents that executes complex, multi-step, multi-domain work autonomously — while remaining downstream of the meaning layer that commissioned it. Not a single agent, but a structured production system with inputs, constraints, processes, outputs, and quality control. See [docs/03-factory-architecture.md](docs/03-factory-architecture.md).

**Agent Contract**
An explicit capability declaration for an agent: what it can do, what it must not do, what inputs it requires, what outputs it guarantees, and its trust level. Contracts are enforced at both assignment time and runtime. See [specs/agent-contract-spec.md](specs/agent-contract-spec.md).

**Assembly Layer**
The factory component where validated outputs from multiple branches are composed into the final deliverable through semantic composition — checking internal consistency, completeness, coherence, and traceability.

**Binary Validity Judgment**
The principle that validity is assessed as a binary (valid / not valid), never as a gradient score. "73% valid" is how violations hide. Derived from SWD Canon Skill 11.

**Coherence Engine**
An MB-OS component that continuously compares what you're doing with what you mean. Surfaces drift without judgment. A mirror, not a guilt machine.

**Commission**
The formal interface between the MB-OS and a factory. Replaces the concept of a "prompt" with a structured specification carrying: origin (meaning source), intent, entities, constraints, validity criteria, and scope boundary. Commissions are immutable once issued. See [specs/commission-spec.md](specs/commission-spec.md).

**Commission Register**
The factory's entry point where the MB-OS deposits its commission. The register holds the commission as an immutable record — if the commission needs to change, a new one is issued.

**Constraint-First Design**
The principle that constraints must be defined before optimization begins. Agents optimize aggressively; without explicit constraints, they find paths that violate things you didn't specify. Derived from SWD Canon Skill 2.

**Decision Interface**
An MB-OS component that runs choices through the meaning layer. Surfaces tensions between competing values rather than resolving them — the human decides, the system illuminates.

**Decomposition Engine**
The factory component that takes a commission and breaks it into a work graph — a directed graph of tasks with dependencies, parallelism opportunities, and merge points. The decomposition is a reviewable artifact before any agent executes.

**Delivery Interface**
The factory's output boundary, where completed work is returned to the MB-OS with: the artifact itself, a provenance record, a validity attestation, and any unresolved tensions. See [specs/delivery-interface-spec.md](specs/delivery-interface-spec.md).

**Derivative**
Something that projects from a source of truth. Factories are derivatives of the MB-OS — they execute downstream of meaning, never upstream. The critical failure mode: when a derivative starts redefining its source.

**Drift**
When actions diverge from stated values or intentions over time. The Coherence Engine detects drift. At the factory level, semantic drift detection checks whether meaning has shifted from what was commissioned.

**Entity-First Thinking**
The principle that entities (the named things a system operates on) must be explicitly defined before work begins. Agents that can't name their entities produce incoherent outputs. Derived from SWD Canon Skill 1.

**Escalation**
When an agent or gate encounters something outside its authority or capability. Rather than guessing, the issue is routed to a named entity (another agent, the human operator, or the MB-OS). Escalation is a feature, not a failure.

**Factory Governance**
The rules about how factories are created, modified, audited, and retired. Factories are versioned artifacts with explicit lifecycles. See [docs/04-factory-governance.md](docs/04-factory-governance.md).

**Gate**
See Validation Gate.

**Hierarchical Truth Management**
The principle that sources of truth flow downward and derivatives never redefine their source. The MB-OS is the source; factories are derivatives. Derived from SWD Canon Skill 5.

**MB-OS (Meaning-Based Operating System)**
The layer between you and the world that optimizes for meaning — the coherence between your actions and your deepest values. Not an app but an infrastructure layer, like how iOS isn't one app but the thing all apps run on. See [docs/02-mb-os.md](docs/02-mb-os.md).

**Meaning Layer**
The "BIOS" of the MB-OS — a structured model of your values, commitments, long arcs, and active tensions. A living document that updates through dialogue and reflection. The philosophical kernel.

**Meaning Vector**
The unit of interaction at the MB-OS level: "what actually matters to you, and why?" Replaces tasks, goals, and priorities as the fundamental unit.

**Narrative Engine**
An MB-OS component that tracks your life as a story — themes, arcs, transitions. It can surface: "the last 6 months have been about building. Your reflections suggest you're ready for integration."

**Orchestration Layer**
The factory's runtime — manages scheduling, parallelism, state tracking, escalation routing, and deadlock detection. Deliberately not intelligent: it's a scheduler with constraint awareness. Intelligence belongs in the agents and gates.

**Provenance**
The complete audit trail of how an artifact was produced: commission → decomposition → agent assignments → work products → gate evaluations → assembly → delivery. Every output can be traced to why it exists.

**Responsibility Localization**
The principle that every work unit, constraint, and gate evaluation has a single named owner. Without localized responsibility, multi-agent systems become unauditable. Derived from SWD Canon Skill 6.

**SWD Canon**
The set of structural discipline skills that bind every layer of the Agentic Factory Architect framework. Not optional best practices — the operating discipline that prevents each layer from degrading. See [docs/05-canon-integration.md](docs/05-canon-integration.md) and [github.com/marxwood/swd-canon-skills](https://github.com/marxwood/swd-canon-skills).

**Validation Gate**
A checkpoint between stages of the work graph that evaluates structural validity — not just completeness or plausibility. Gates check: commission alignment, entity integrity, constraint satisfaction, contract fulfillment, and semantic drift. They produce Pass, Return, or Escalate — never a confidence score. See [specs/validation-gate-spec.md](specs/validation-gate-spec.md).

**Work Graph**
The directed graph of tasks produced by the Decomposition Engine. Each node is a work unit with: a named entity, required inputs, capability requirement, output specification, and inherited constraints. Not a linear pipeline — supports parallelism, dependencies, and merge points.

**Work Unit**
A single node in the work graph. The atomic unit of work in a factory. Each work unit has a single responsible agent, explicit inputs and outputs, and passes through at least one validation gate before its output proceeds.
