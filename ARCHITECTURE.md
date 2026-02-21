# Architecture Overview

A single-page overview of the Agentic Factory Architect system. For deep dives, see the individual documents linked below.

## The Stack

```
┌─────────────────────────────────────┐
│         AGENTIC FACTORIES           │  Execution at scale
│  (Commission Register, Decomp,     │  docs/03-factory-architecture.md
│   Agents, Orchestration, Gates,    │
│   Assembly, Delivery)              │
├─────────────────────────────────────┤
│              MB-OS                  │  Meaning layer
│  (Meaning Layer, Coherence Engine, │  docs/02-mb-os.md
│   Decision Interface, Narrative    │
│   Engine, Interface Layer)         │
├─────────────────────────────────────┤
│       PERSONAL ASSISTANT            │  Surface interface
│  (Conversational, task-oriented)   │  docs/01-evolution.md
├─────────────────────────────────────┤
│              YOU                    │  The meaning-maker
└─────────────────────────────────────┘
```

## Information Flow

**Downward (commissioning):**
You → PA → MB-OS → Factory

Your meaning layer generates commissions. Factories execute them. The meaning constrains what factories can do.

**Upward (delivery):**
Factory → MB-OS → PA → You

Factories deliver artifacts with provenance and attestation. The MB-OS evaluates: "Is this mine?" The PA presents results.

**Lateral (coherence):**
MB-OS continuously monitors alignment between actions and values. The Coherence Engine detects drift. The Narrative Engine tracks your arc.

## The Seven Factory Components

```
Commission Register → Decomposition Engine → Agent Registry
                                                    ↓
                                          Orchestration Layer
                                              ↓         ↑
                                        Validation Gates (return loops)
                                              ↓
                                        Assembly Layer
                                              ↓
                                       Delivery Interface
```

| # | Component | One-Line Purpose | Spec |
|---|-----------|-----------------|------|
| 1 | Commission Register | Immutable intent + constraints from MB-OS | [spec](specs/commission-spec.md) |
| 2 | Decomposition Engine | Breaks commission into reviewable work graph | — |
| 3 | Agent Registry | Typed catalog with capability contracts | [spec](specs/agent-contract-spec.md) |
| 4 | Orchestration Layer | Scheduling, parallelism, escalation | — |
| 5 | Validation Gates | Binary validity at every transition | [spec](specs/validation-gate-spec.md) |
| 6 | Assembly Layer | Semantic composition into coherent wholes | — |
| 7 | Delivery Interface | Artifact + provenance + attestation | [spec](specs/delivery-interface-spec.md) |

## Key Differentiators

**Commission-first, not prompt-first.** Every factory begins with a semantic commission carrying meaning, entities, constraints, and binary validity criteria.

**Validation gates, not output checking.** Quality is enforced at every transition, not just at the end. Gates produce Pass/Return/Escalate — never "73% valid."

**Immutable commissions.** Goals don't shift mid-execution. If meaning changes, a new commission is issued. The old one is archived.

**Full provenance.** Every output traces back through: delivery → assembly → gates → agents → decomposition → commission → meaning layer.

## The Binding Discipline

The [SWD Canon](https://github.com/marxwood/swd-canon-skills) provides the structural discipline that prevents each layer from degrading:

- **Entity-First Thinking** → names things before operating on them
- **Constraint-First Design** → bounds before optimizing
- **Binary Validity Judgment** → no partial validity at factory scale
- **Hierarchical Truth Management** → derivatives never redefine their source
- **Explicit Change Control** → no silent evolution

See [Canon Integration](docs/05-canon-integration.md) for the full skill-by-skill mapping.

## Factory Types (with examples)

| Factory Type | Purpose | Example |
|-------------|---------|---------|
| Research | Landscape mapping, entity extraction, synthesis | [research-factory.md](examples/research-factory.md) |
| Creation | Drafting, critique, revision, publication | [creation-factory.md](examples/creation-factory.md) |
| Decision | Option mapping, value tension surfacing | [decision-factory.md](examples/decision-factory.md) |
| Execution | Project decomposition, task management, delivery | [execution-factory.md](examples/execution-factory.md) |

## Governance

Factories are versioned artifacts with explicit lifecycles: `DESIGNED → VALIDATED → ACTIVE → DEPRECATED → RETIRED`. Changes go through: declare → version → validate → compare → promote/reject → archive.

See [Factory Governance](docs/04-factory-governance.md) for the full governance model.

## Inter-MB-OS: B2B Coordination

The framework extends beyond single-person use to **multi-agent organizational coordination**. When two parties (e.g., Company X and a service provider) each operate their own MB-OS, the Inter-MB-OS Protocol governs how they:

1. **Articulate needs and capabilities** — structured, not vague
2. **Identify the meaning overlap zone** — where values and constraints are compatible
3. **Draft dual-origin commissions** — carrying both parties' meaning, constraints, and validity criteria
4. **Review the work graph as a negotiation artifact** — concrete structure to agree on, not abstract promises
5. **Evaluate delivery through both meaning layers** — both parties must independently accept

This transforms the framework from personal infrastructure into an **organizational coordination protocol**.

See [Inter-MB-OS Protocol](docs/06-inter-mb-os-protocol.md) for the full specification.

### Factory Composition as Partnership

When a single executor can't fulfill a commission alone, it can compose factories with partner agents. The Primary Executor commissions sub-executors through **derived commissions** — scoped, constrained sub-commissions that carry only what the partner needs. A composition boundary gate validates partner output before assembly. The Primary Executor remains responsible for the full delivery.

This enables ecosystem-scale coordination: organizations combine capabilities across meaning layers while maintaining the same commission-first, constraint-first, binary validity protocol at every boundary.

See [Factory Composition & Partnerships](docs/07-factory-composition-partnerships.md) for the full specification.
