# Roadmap

The path from conceptual framework to running system.

## Current State: v0.2 — Architectural Framework

The Agentic Factory Architect exists as a complete **conceptual and architectural** specification:
- The full stack is defined (PA → MB-OS → Agentic Factories)
- Factory components are specified with formal schemas
- Governance model is established
- The SWD Canon integration is mapped
- Four concrete examples demonstrate the architecture in action

**What exists:** Documentation, specifications, schemas, diagrams, examples.
**What doesn't exist yet:** Running code.

---

## Phase 1: Reference Implementation — The Commission Layer

**Goal:** A working commission system — the interface between meaning and execution.

**Deliverables:**
- Commission data structures (implementing the schema from `specs/commission-spec.md`)
- Commission lifecycle management (DRAFT → ISSUED → ACCEPTED → COMPLETED → ARCHIVED)
- Immutability enforcement
- Commission validation (are all required fields present? are constraints well-formed?)
- Simple CLI for issuing and inspecting commissions

**Why start here:** The commission is the foundation. If the commission layer is wrong, everything built on top inherits that wrongness. Getting this right first is Skill 2 (Constraint-First Design) applied to the build process itself.

**Technology considerations:**
- Language: Python or TypeScript (ecosystem support for LLM integration)
- Storage: JSON files initially (simplicity), with a clear interface for future database backing
- Validation: JSON Schema or Pydantic models

---

## Phase 2: Agent Registry + Contracts

**Goal:** A typed catalog of agents with enforceable capability contracts.

**Deliverables:**
- Agent contract data structures (implementing `specs/agent-contract-spec.md`)
- Agent registration and discovery
- Capability matching (given a work unit's requirements, find qualified agents)
- Contract enforcement at assignment time
- Simple agent wrappers for LLM-based agents (Claude, GPT, etc.)

**Why second:** Agents are the workers. Before building orchestration, you need to know what you're orchestrating. The registry must exist before the orchestration layer can assign work.

---

## Phase 3: Validation Gates

**Goal:** Working validation gates that enforce binary validity at stage transitions.

**Deliverables:**
- Gate evaluation framework (implementing `specs/validation-gate-spec.md`)
- Gate types: Commission Alignment, Entity Integrity, Constraint Satisfaction, Contract Fulfillment, Semantic Drift
- Return protocol with violation reports
- Escalation protocol with timeout handling
- Gate independence enforcement (gate cannot be operated by the producing agent)

**Why third:** Gates are the immune system. Building them before the full orchestration layer means that when orchestration comes online, quality enforcement is already in place.

---

## Phase 4: Decomposition + Orchestration

**Goal:** The ability to take a commission, decompose it into a work graph, and orchestrate execution.

**Deliverables:**
- Decomposition engine (commission → work graph)
- Work graph data structures (directed graph with dependencies, parallelism, merge points)
- Orchestration runtime (scheduling, state tracking, escalation routing)
- Integration with Agent Registry (capability matching → agent assignment)
- Integration with Validation Gates (gates at every transition)

**Why fourth:** This is the most complex component. It depends on commissions (Phase 1), agents (Phase 2), and gates (Phase 3) all existing. Building it last among the core components means it can integrate with tested, stable subcomponents.

---

## Phase 5: Assembly + Delivery

**Goal:** Semantic composition of validated outputs and structured delivery.

**Deliverables:**
- Assembly layer (merge multiple branch outputs into coherent wholes)
- Consistency, completeness, coherence, and traceability checks
- Delivery interface (implementing `specs/delivery-interface-spec.md`)
- Provenance record generation
- Validity attestation
- Unresolved tension surfacing

---

## Phase 6: End-to-End Factory

**Goal:** A complete, working agentic factory that can accept a commission and deliver a validated artifact.

**Deliverables:**
- End-to-end integration of all components
- Factory configuration format (agent manifest, gate configuration, constraint set)
- Factory versioning
- Audit trail generation
- At least one working factory type (likely Research Factory — simplest to validate)

---

## Phase 7: MB-OS Prototype

**Goal:** A minimal meaning layer that can issue commissions and evaluate deliveries.

**Deliverables:**
- Meaning layer data structures (values, commitments, tensions, narrative arcs)
- Coherence engine (basic drift detection)
- Commission generation from meaning layer
- Delivery evaluation against meaning layer
- Conversational interface for meaning elicitation

**Why last in the core phases:** The MB-OS is the most philosophically complex component. Building it after the factory layer means it has a concrete system to commission — the philosophy is grounded in working infrastructure.

---

## Future Phases

### Factory Governance System
- Factory lifecycle management (DESIGNED → VALIDATED → ACTIVE → DEPRECATED → RETIRED)
- Change control protocol
- Factory composition (sub-factories)
- Audit and compliance tools

### Multi-Factory Coordination
- Multiple factories serving one MB-OS
- Factory selection (which factory type for which commission?)
- Resource sharing across factories
- Cross-factory coherence

### MB-OS Maturation
- Narrative Engine (theme tracking, arc awareness, transition detection)
- Decision Interface (option mapping against meaning layer)
- Multiplicity support (parts/IFS integration)
- Long-term drift detection and meaning evolution

### Trust + Privacy Architecture
- Local-first data storage
- Encryption at rest for meaning layer
- Selective sharing (what parts of the meaning layer can factories see?)
- Audit access controls

---

## Principles for the Build

These principles govern how the roadmap is executed — the Canon applied to the build process:

1. **Each phase must be independently valid** — no phase depends on future phases to be useful
2. **Tests before features** — every component has validation before it has functionality
3. **Interfaces before implementations** — define the contracts between components before building the internals
4. **The framework describes its own build** — if a phase can be expressed as a commission, express it as one
5. **No silent evolution** — every change to the roadmap itself is versioned and justified
