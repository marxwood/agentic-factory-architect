# Agentic Factory Architect

> From Personal Assistant to Meaning-Based OS to Agentic Factories — a complete architectural framework for building AI systems that execute complex work while remaining aligned with human meaning and values.

## The Core Idea

Most AI orchestration systems are **pipeline-first**: define a sequence, assign agents, run. They handle the plumbing but not the meaning.

This architecture is **commission-first** and **constraint-first**. Every factory begins with a semantic commission from a Meaning-Based Operating System (MB-OS), carries explicit constraints at every layer, and validates outputs with binary judgment — not confidence scores.

## The Full Stack

```
AGENTIC FACTORIES          ← execution at scale
        ↑ commissioned by
MB-OS                      ← meaning layer, coherence engine
        ↑ runs on
Personal Assistant          ← surface interface
        ↑ interacts through
You
```

**Meaning commissions. Structure constrains. Factories execute. Validity is binary at every layer.**

## Quick Start

- **New here?** Start with [MANIFESTO.md](MANIFESTO.md) for the philosophy, then [ARCHITECTURE.md](ARCHITECTURE.md) for the one-page overview
- **Want the full picture?** Read [docs/01-evolution.md](docs/01-evolution.md) through [docs/06-inter-mb-os-protocol.md](docs/06-inter-mb-os-protocol.md) in order
- **Building something?** Jump to [specs/](specs/) for formal schemas and [examples/](examples/) for concrete scenarios

## Repository Structure

```
├── README.md                          # This file
├── MANIFESTO.md                       # The philosophical foundation
├── ARCHITECTURE.md                    # Single-page architecture overview
├── CHANGELOG.md                       # Version history
├── GLOSSARY.md                        # Terminology reference
├── ROADMAP.md                         # Implementation roadmap
├── docs/
│   ├── 01-evolution.md                # PA → MB-OS → Agentic Factories
│   ├── 02-mb-os.md                    # The Meaning-Based Operating System
│   ├── 03-factory-architecture.md     # Complete factory architecture
│   ├── 04-factory-governance.md       # Governance model for factories
│   ├── 05-canon-integration.md        # How SWD Canon skills bind the system
│   └── 06-inter-mb-os-protocol.md    # Multi-agent B2B negotiation protocol
├── specs/
│   ├── commission-spec.md             # Commission specification format
│   ├── agent-contract-spec.md         # Agent capability contract format
│   ├── validation-gate-spec.md        # Validation gate protocol
│   └── delivery-interface-spec.md     # Delivery interface protocol
├── diagrams/
│   ├── full-stack.mermaid             # Complete system stack
│   ├── factory-flow.mermaid           # Factory execution flow
│   └── mb-os-components.mermaid       # MB-OS internal architecture
└── examples/
    ├── research-factory.md            # Example: Research Factory
    ├── creation-factory.md            # Example: Creation Factory
    ├── decision-factory.md            # Example: Decision Factory
    └── execution-factory.md           # Example: Execution Factory (meta-recursive)
```

## Architectural Principles

Derived from the [SWD Canon](https://github.com/marxwood/swd-canon-skills):

1. **Every factory has a commission, not a prompt** — commissions carry meaning, entities, constraints, and validity criteria
2. **Every agent has a named role with bounded authority** — no generalists, explicit capability declarations
3. **Work products flow through validation gates** — binary validity checks at every transition, not just at the end
4. **The factory is inspectable at every layer** — by both humans and machines, no black boxes
5. **Change to the factory is explicit and declared** — versioned, named, traceable, no silent evolution

## The Seven Components

| Component | Purpose |
|-----------|---------|
| **Commission Register** | Immutable entry point — origin, intent, entities, constraints, validity criteria |
| **Decomposition Engine** | Breaks commission into a reviewable work graph |
| **Agent Registry** | Typed catalog of agents with capability contracts |
| **Orchestration Layer** | Runtime scheduling, state tracking, escalation routing |
| **Validation Gates** | Binary validity checking at every stage transition |
| **Assembly Layer** | Semantic composition of validated outputs into coherent wholes |
| **Delivery Interface** | Output boundary with artifact, provenance, and attestation |

## What Makes This Different

| Aspect | Existing Systems | Agentic Factory Architect |
|--------|-----------------|--------------------------|
| Starting point | Task description / prompt | Semantic commission from MB-OS |
| Quality control | Output checking at the end | Validation gates at every transition |
| Validity model | Gradient scores (0.0–1.0) | Binary: valid or not valid |
| Traceability | Logs | Full provenance chain: meaning → delivery |
| Goal stability | Goals can shift mid-execution | Immutable commissions; new goals = new commission |

## Philosophical Foundation

The MB-OS is built on a different axiom than most productivity tools:

> **Clarity precedes productivity.** Before optimizing *how* you do things, you need a stable answer to *why*.

Connected traditions: Viktor Frankl (meaning as primary motivation), Heidegger (Dasein and care-structure), IFS (multiplicity of self), Integral Theory (interior + exterior, individual + collective).

## Related

- [SWD Canon Skills](https://github.com/marxwood/swd-canon-skills) — the structural discipline that binds every layer of this architecture

## License

MIT

## Author

Marko Marinkovic ([@marxwood](https://github.com/marxwood))
