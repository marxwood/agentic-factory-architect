# Factory Composition & Partnerships

## The Problem This Solves

The [Inter-MB-OS Protocol](06-inter-mb-os-protocol.md) describes how two sovereign meaning layers negotiate and execute shared work. But it assumes the Executor can fulfill the entire commission alone.

In practice, this is often not the case. Clarivate may have data validation capabilities but not the SAP integration expertise that Company X's commission requires. Agent Y — a systems integrator — has exactly that expertise.

The question becomes: **how do multiple executors compose their factories to fulfill a commission that none of them could fulfill alone?**

This is Factory Composition applied to the inter-organizational layer. It transforms the governance concept of sub-factories into a **partnership protocol** between sovereign agents.

## The Scenario

```
┌──────────────────┐
│   Company X      │
│   (Commissioner) │
│   MB-OS (X)      │
└────────┬─────────┘
         │ dual-origin commission
         ▼
┌──────────────────────────────────────────────┐
│   Clarivate (Primary Executor)               │
│   MB-OS (Clarivate)                          │
│                                              │
│   ┌────────────────────────────────────────┐ │
│   │   Clarivate Factory                    │ │
│   │                                        │ │
│   │   WU-1: Data validation    ← own      │ │
│   │   WU-2: Entity resolution  ← own      │ │
│   │   WU-3: SAP integration   ← Agent Y   │ │
│   │   WU-4: Quality scoring    ← own      │ │
│   │   WU-5: Assembly           ← own      │ │
│   │              ▲                         │ │
│   │              │ validation gate          │ │
│   │              │ (composition boundary)   │ │
│   └──────────────┼─────────────────────────┘ │
└──────────────────┼───────────────────────────┘
                   │ derived commission
                   ▼
         ┌──────────────────┐
         │   Agent Y        │
         │   (Sub-Executor)  │
         │   MB-OS (Y)       │
         │                   │
         │   ┌─────────────┐ │
         │   │ Y's Factory │ │
         │   │ SAP integ.  │ │
         │   └─────────────┘ │
         └──────────────────┘
```

Company X sees one executor: Clarivate. Company X does not need to know that Agent Y exists. Clarivate is responsible for the entire delivery — including the part Agent Y produces.

## Core Concepts

### Partnership vs. Delegation

This is a **partnership**, not a delegation. Delegation implies a hierarchical relationship — "do this because I said so." A partnership implies:

- Agent Y has its own MB-OS with its own values and constraints
- Agent Y evaluates the derived commission against its own meaning layer
- Agent Y can decline if the commission conflicts with its values
- Agent Y contributes capability that Clarivate genuinely lacks

The relationship is collaborative, not subordinate. But within the scope of the commission, Clarivate holds **primary responsibility** — because that's what Company X contracted for.

### The Three-Layer Trust Chain

```
Company X trusts Clarivate (dual-origin commission)
    Clarivate trusts Agent Y (partnership agreement + derived commission)
        Agent Y's factory executes the sub-commission
```

Trust does not transitively flow from Company X to Agent Y. Company X trusts Clarivate. Clarivate trusts Agent Y. These are separate trust relationships with separate terms.

This means:
- Company X's constraints flow through Clarivate to Agent Y — but Clarivate is responsible for enforcing them
- Agent Y's output goes to Clarivate, not to Company X — Clarivate validates before assembly
- If Agent Y violates a constraint, Clarivate is accountable to Company X — Clarivate's relationship with Agent Y is Clarivate's problem to manage

### Derived Commission

When Clarivate's Decomposition Engine determines that WU-3 (SAP integration) requires capability it doesn't have, it creates a **derived commission** for Agent Y.

The derived commission:
- **Traces to the parent** — it references the original commission (without exposing Company X's full meaning layer)
- **Carries only what's needed** — Agent Y sees the work unit's requirements, relevant constraints, and success criteria — not the full commission
- **Adds Clarivate's own constraints** — Clarivate may impose additional constraints on Agent Y (delivery format, timeline, interface specifications) that come from Clarivate's MB-OS, not Company X's
- **Is immutable** — same rule as any commission

```yaml
derived_commission:
  id: "dc-clarivate-agenty-sap-001"
  type: DERIVED
  parent_commission: "c-inter-001"  # reference only, not full content

  derivation:
    derived_by: "Clarivate"
    work_unit_reference: "WU-3"
    reason: "SAP integration expertise not available in Clarivate's agent registry"

  commissioner:
    party: "Clarivate"
    meaning_source: "service_delivery.partnership.capability_extension"
    constraints:
      hard:
        - "Output must conform to Clarivate's integration interface spec v2.1"
        - "All data handling must comply with EU jurisdiction (inherited from parent)"
        - "No direct contact with Company X — all communication through Clarivate"
      soft:
        - "Prefer standard SAP BAPI interfaces over custom development"

  executor:
    party: "Agent Y"
    capabilities_offered: ["sap_integration", "data_pipeline_construction", "middleware_development"]
    constraints:
      hard:
        - "Agent Y retains IP rights to its integration patterns"
        - "Implementation must use Agent Y's certified SAP connectors"
      soft:
        - "Prefer reuse of existing integration templates"

  shared:
    entities:
      - name: "SAP Integration Layer"
        commissioner_definition: "The component that connects Clarivate's validation output to Company X's SAP environment"
        executor_definition: "A middleware layer using certified SAP connectors to ingest validated data"
        agreed_definition: "A middleware component that receives Clarivate-validated data and writes it to SAP using certified BAPI connectors"

    validity_criteria:
      - criterion: "Integration handles all data types specified in Clarivate's output schema"
        evaluator: "Clarivate's technical validation gate"
      - criterion: "Write operations are idempotent (safe to retry)"
        evaluator: "Agent Y's quality assurance"
      - criterion: "Latency under 500ms per record at production volume"
        evaluator: "joint_benchmark"
```

### The Composition Boundary

The most critical architectural element in a partnership is the **composition boundary** — the validation gate that sits between Agent Y's output and Clarivate's assembly layer.

This gate is Clarivate's responsibility. It checks:

1. **Contract fulfillment** — did Agent Y deliver what the derived commission specified?
2. **Interface compliance** — does the output match Clarivate's expected integration format?
3. **Constraint inheritance** — were the constraints inherited from Company X's commission preserved?
4. **Entity consistency** — does Agent Y's output use entities consistently with how Clarivate defined them?
5. **No scope leakage** — did Agent Y's work stay within the boundaries of the derived commission?

```
Agent Y's Factory
    ↓ delivers
[Composition Boundary Gate]     ← Clarivate-owned, binary validity
    ↓ pass
Clarivate's Assembly Layer
    ↓ assembles with other work units
[Final Validation Gate]
    ↓ pass
Delivery Interface → Company X
```

If Agent Y's output fails the composition boundary gate, it's returned to Agent Y — not escalated to Company X. Clarivate manages the relationship. Company X sees only the final delivery.

## Partnership Negotiation Flow

### Phase 1: Capability Gap Detection

During decomposition, Clarivate's factory identifies a work unit that no agent in its registry can fulfill.

```
Decomposition Engine → Agent Registry: "Need SAP integration capability"
Agent Registry → Decomposition Engine: "No matching agent. Capability gap."
Decomposition Engine → Clarivate's MB-OS: "Commission requires capability we don't have.
  Options: (a) decline commission, (b) find a partner, (c) escalate to Company X"
```

### Phase 2: Partner Discovery

Clarivate's MB-OS evaluates potential partners against its own values and constraints:
- Does Agent Y have the capability?
- Is Agent Y trustworthy (history, reputation, quality standards)?
- Can Agent Y operate under the constraints inherited from Company X's commission?
- Does partnership with Agent Y conflict with any of Clarivate's values?

### Phase 3: Partnership Negotiation

Clarivate's agent and Agent Y's agent negotiate using the Inter-MB-OS Protocol — the same protocol used between Company X and Clarivate, now applied one level down.

The negotiation produces a derived commission with:
- Clear scope (only the capability gap, not the whole commission)
- Combined constraints (inherited + Clarivate's own + Agent Y's own)
- Interface specification (how Agent Y's output integrates with Clarivate's factory)
- Validity criteria for the composition boundary gate

### Phase 4: Work Graph Update

The original work graph is updated to reflect the partnership:
- WU-3 is marked as "externally executed by Agent Y"
- A composition boundary gate is inserted between WU-3 and the assembly layer
- The dependency graph is updated to reflect Agent Y's delivery timeline

This updated work graph may be shared with Company X during the review phase (Phase 7 of the Inter-MB-OS Protocol) — with or without revealing Agent Y's identity, depending on Clarivate's transparency preferences and Company X's requirements.

### Phase 5: Parallel Execution

Agent Y's factory executes the derived commission in parallel with Clarivate's own work units. Both factories operate independently — Agent Y's orchestration layer is separate from Clarivate's.

### Phase 6: Composition

Agent Y delivers. Clarivate's composition boundary gate validates. If it passes, the output flows into Clarivate's assembly layer alongside the outputs of Clarivate's own work units.

The assembly layer now has the additional challenge of composing outputs from **two different factories** — which may have different formatting conventions, terminology, or structural assumptions. This is why the derived commission must specify interface requirements precisely.

## Multi-Partner Composition

The pattern extends to more than two executors:

```
Company X (Commissioner)
    ↓
Clarivate (Primary Executor)
    ├── Own factory: WU-1, WU-2, WU-4, WU-5
    ├── Agent Y: WU-3 (SAP integration)
    └── Agent Z: WU-6 (compliance certification)
```

Each partner receives its own derived commission. Each has a composition boundary gate. Clarivate's assembly layer integrates all outputs.

**Composition depth limit:** Partnerships can nest — Agent Y could in theory partner with Agent Z2 for a sub-sub-commission. But composition depth must be **explicitly bounded**. Each level of nesting adds latency, reduces visibility, and dilutes responsibility. The governance model should specify a maximum composition depth (recommended: 2 levels from the original commissioner).

## What the Canon Requires

### Skill 5 — Hierarchical Truth Management
The hierarchy extends:
```
Company X's MB-OS
    ↓ source of truth for: need
Dual-Origin Commission (X ↔ Clarivate)
    ↓ source of truth for: the work
Clarivate's MB-OS
    ↓ source of truth for: execution approach
Derived Commission (Clarivate ↔ Agent Y)
    ↓ source of truth for: the sub-work
Agent Y's Factory
    ↓ derivative of derived commission
```

**Each layer is derivative of the one above.** Agent Y's factory cannot redefine the derived commission. The derived commission cannot contradict the original commission. The original commission cannot contradict either MB-OS's meaning layer.

If Agent Y discovers that the derived commission is inadequate — that the SAP integration requires something Clarivate didn't specify — Agent Y surfaces the gap to Clarivate. Clarivate decides whether to update the derived commission (issuing a new one) or escalate to Company X if it affects the original commission.

### Skill 6 — Responsibility Localization

| Responsibility | Owner |
|----------------|-------|
| Original need and constraints | Company X |
| Fulfilling the full commission | Clarivate |
| Selecting trustworthy partners | Clarivate |
| Validating partner output | Clarivate (composition boundary gate) |
| Delivering the derived commission | Agent Y |
| Agent Y's internal quality | Agent Y |
| Final assembly and delivery | Clarivate |
| Evaluating "is this mine?" | Company X |

**Critical rule:** Clarivate cannot deflect responsibility to Agent Y when delivering to Company X. "Agent Y didn't deliver on time" is Clarivate's problem, not Company X's. This is what makes partnership different from referral.

### Skill 9 — Explicit Change Control

When Agent Y updates its factory (new model, new SAP connector version), this is a change that affects Clarivate's composition. The change control protocol applies:

1. Agent Y declares the change to Clarivate
2. Clarivate evaluates impact on the derived commission and composition boundary
3. If the change affects validity, the derived commission may need to be re-issued
4. If the change propagates to the original commission's scope, Company X must be informed

Silent evolution in a partner's factory is just as dangerous as silent evolution in your own.

### Skill 11 — Binary Validity Judgment

The composition boundary gate is binary. Agent Y's output either meets the derived commission's criteria or it doesn't. "Close enough" at the composition boundary is how integration failures hide.

This is especially important because **integration points are where errors compound**. A 95% correct data validation layer combined with a 95% correct SAP integration layer doesn't give you 90% correct end-to-end. It gives you an unknown number of edge cases where both systems are slightly wrong in ways that interact unpredictably.

Binary validity at each boundary prevents this accumulation.

## Failure Modes Specific to Partnerships

### Responsibility Diffusion
When things go wrong, the tendency is for each party to point at the other. The protocol prevents this through explicit responsibility localization — every work unit, every constraint, every gate has a named owner. The provenance record makes this traceable.

### Interface Mismatch
Agent Y builds to their understanding of the interface spec. Clarivate expects something slightly different. The composition boundary gate catches this — but only if the interface spec in the derived commission was precise enough. Vague interface specifications are the most common source of composition failure.

### Trust Chain Erosion
Over time, partnerships can drift. Agent Y may quietly change their approach, use different subcontractors, or relax their quality standards. Without explicit change control at the partnership boundary, Clarivate may not notice until delivery fails. The governance model requires: partnership agreements are versioned artifacts, subject to the same change control as factory configurations.

### Confidentiality Leakage
Company X's commission may contain sensitive information. The derived commission should contain only what Agent Y needs. But over multiple interactions, Agent Y may accumulate enough context to reconstruct Company X's strategy. The protocol requires: derived commissions are scoped to the minimum necessary information. Historical derived commissions are archived with access controls.

### Depth Creep
Agent Y partners with Agent Z2. Agent Z2 partners with Agent Z3. Before long, the original commission is being executed five levels deep by agents that Company X has never heard of and Clarivate can barely audit. The governance model must enforce: maximum composition depth, declared at the original commission level.

## The Bigger Picture

Factory Composition as Partnership transforms the Agentic Factory architecture from an **organizational tool** into an **ecosystem protocol**.

Individual MB-OS layers (people) use factories to execute meaningful work.
Organizations use Inter-MB-OS Protocol to coordinate across meaning layers.
Partnerships use Factory Composition to combine capabilities that no single organization possesses.

The same principles hold at every level: commission-first, constraint-first, binary validity, full provenance, explicit responsibility. The architecture scales from a person managing their own attention to a network of organizations collaborating on complex problems — without losing the coherence that the meaning layer provides.

The work graph remains the negotiation artifact at every boundary. Whether it's your MB-OS commissioning your own factory, Company X commissioning Clarivate, or Clarivate commissioning Agent Y — the pattern is the same: **structured intent, explicit constraints, reviewable plan, validated execution, traceable delivery**.

This is what makes the architecture composable. Not because it's technically modular — but because the same protocol operates at every scale.
