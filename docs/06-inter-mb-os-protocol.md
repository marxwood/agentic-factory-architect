# Inter-MB-OS Protocol

## The Problem This Solves

The core Agentic Factory architecture assumes a single MB-OS — one person, one meaning layer, one source of truth. But meaningful work rarely happens in isolation. People operate within organizations. Organizations interact with other organizations. Each carries its own meaning layer: values, constraints, commitments, definitions of success.

When two meaning layers need to coordinate — when Company X needs something that MyCompany can provide — the single-MB-OS model is insufficient. You need a protocol for **how two meaning layers negotiate, align, and produce shared commissions**.

This document specifies that protocol.

## The Scenario

```
┌─────────────────────┐         ┌─────────────────────┐
│   Company X Agent   │         │  MyCompany Agent     │
│   ┌───────────────┐ │         │ ┌───────────────┐    │
│   │   MB-OS (X)   │ │◄───────►│ │ MB-OS (MC)    │   │
│   │  values       │ │ negot.  │ │ values         │   │
│   │  constraints  │ │ layer   │ │ constraints    │   │
│   │  needs        │ │         │ │ capabilities   │   │
│   └───────────────┘ │         │ └───────────────┘    │
└─────────────────────┘         └─────────────────────┘
                                          │
                                          ▼
                                ┌─────────────────────┐
                                │  Agentic Factory     │
                                │  (MyCompany-owned)   │
                                │  commissioned by     │
                                │  dual-origin         │
                                │  commission          │
                                └─────────────────────┘
```

Company X's agent represents Company X's meaning layer — their problem, their values, their constraints on what a solution must look like. MyCompany's agent represents MyCompany's meaning layer — their capabilities, their business model, their constraints on what they can deliver and how.

Neither agent is subordinate to the other. Both carry legitimate meaning. The protocol exists to find — or fail to find — the overlap.

## Core Concepts

### Roles: Commissioner and Executor

In an inter-MB-OS interaction, the two parties take asymmetric roles:

**Commissioner** — the party whose meaning initiates the need. Company X has a problem. Their MB-OS generates the need. They are the origin of "why this work exists."

**Executor** — the party whose factory fulfills the need. MyCompany has the capability. Their factory does the work. They are the origin of "how this work gets done."

This asymmetry is structural, not hierarchical. The Commissioner doesn't command the Executor. The Executor doesn't define the Commissioner's needs. Each is sovereign over their own meaning layer.

### The Meaning Overlap Zone

Not all of Company X's values are relevant to MyCompany. Not all of MyCompany's constraints are visible to Company X. The protocol doesn't require full transparency between meaning layers. It requires identifying the **overlap zone** — the subset of each party's meaning that is relevant to the shared work.

```
    Company X's MB-OS              MyCompany's MB-OS
┌───────────────────────┐    ┌───────────────────────┐
│                       │    │                       │
│   Internal values     │    │   Internal values     │
│   Internal tensions   │    │   Business model      │
│   Full narrative      │    │   Full capabilities   │
│                       │    │                       │
│  ┌─────────────────┐  │    │  ┌─────────────────┐  │
│  │ Shared:         │  │    │  │ Shared:         │  │
│  │ - Problem def   │◄─┼────┼─►│ - Capabilities  │  │
│  │ - Constraints   │  │    │  │ - Constraints   │  │
│  │ - Success def   │  │    │  │ - Terms         │  │
│  │ - Entities      │  │    │  │ - Quality std   │  │
│  └─────────────────┘  │    │  └─────────────────┘  │
│                       │    │                       │
└───────────────────────┘    └───────────────────────┘
                   │              │
                   └──────┬───────┘
                          ▼
                  ┌───────────────┐
                  │ Dual-Origin   │
                  │ Commission    │
                  └───────────────┘
```

What stays private is each party's prerogative. Company X doesn't need to reveal their full strategic narrative. MyCompany doesn't need to reveal their cost structure. The protocol only requires that **what's shared is shared explicitly and honestly** — because the commission is built on it.

### Dual-Origin Commission

The standard commission has a single `origin.meaning_source`. A dual-origin commission has two:

```yaml
commission:
  id: "c-inter-001"
  type: DUAL_ORIGIN

  commissioner:
    party: "Company X"
    agent_id: "agent-company-x-001"
    meaning_source: "operations.efficiency.data_quality_problem"
    values_relevant: ["accuracy", "speed", "compliance"]
    constraints:
      hard:
        - "Solution must integrate with existing SAP infrastructure"
        - "All data must remain within EU jurisdiction"
        - "Results must be auditable by our compliance team"
      soft:
        - "Prefer minimal disruption to existing workflows"
    success_definition: "Data quality issues detected and resolved before they reach downstream systems"

  executor:
    party: "MyCompany"
    agent_id: "agent-mycompany-001"
    meaning_source: "service_delivery.data_solutions"
    capabilities_offered: ["data_validation", "entity_resolution", "quality_scoring"]
    constraints:
      hard:
        - "Cannot access Company X's raw data outside the agreed processing scope"
        - "Solution must use MyCompany's validated data sources"
        - "Delivery timeline: minimum 6 weeks for implementation"
      soft:
        - "Prefer use of existing MyCompany platform components"
    quality_standards: "Output validated against MyCompany's accuracy benchmarks (>99.2%)"

  shared:
    entities:
      - name: "Data Quality Pipeline"
        commissioner_definition: "The system that catches errors before downstream processing"
        executor_definition: "An automated validation layer using MyCompany's entity resolution engine"
        agreed_definition: "A validation layer that catches data quality issues using entity resolution, integrated into Company X's existing infrastructure"
    constraints_combined:
      hard:
        - "EU data jurisdiction (Commissioner)"
        - "MyCompany data sources only (Executor)"
        - "SAP integration required (Commissioner)"
        - "6-week minimum timeline (Executor)"
      soft:
        - "Minimal workflow disruption (Commissioner)"
        - "Prefer existing platform components (Executor)"
    validity_criteria:
      - criterion: "Detection rate exceeds current manual process"
        evaluator: "Company X's compliance team"
      - criterion: "Accuracy meets MyCompany's published benchmarks"
        evaluator: "MyCompany's quality assurance"
      - criterion: "Solution operates within EU data jurisdiction"
        evaluator: "joint_verification"
```

Notice: **entities have three definitions** — what each party means by the term, and the agreed definition that the commission uses. This prevents the failure mode where two parties use the same word to mean different things and only discover the mismatch at delivery.

## The Negotiation Flow

### Phase 1: Need Articulation

Company X's agent presents the need — derived from Company X's MB-OS:

```
Commissioner → Executor:
  "We have a data quality problem in our supply chain data.
   Our meaning layer says this matters because: [accuracy, compliance, speed].
   Our constraints are: [EU data, SAP integration, auditability].
   We're looking for: [entity resolution, validation, quality scoring]."
```

This is not a prompt. It's a **structured need statement** that carries meaning context. MyCompany's agent can evaluate this against MyCompany's MB-OS: "Do we have the capability? Does this fit our service model? Are there constraint conflicts?"

### Phase 2: Capability Response

MyCompany's agent responds with what's possible — derived from MyCompany's MB-OS:

```
Executor → Commissioner:
  "We can address this with our data validation and entity resolution capabilities.
   Our constraints are: [our data sources, 6-week timeline, platform preference].
   Potential conflicts we see: [none with hard constraints; SAP integration
   may require custom work outside our standard platform]."
```

### Phase 3: Overlap Identification

Both agents identify the meaning overlap zone:
- Where values align (Company X wants accuracy; MyCompany's quality standards guarantee it)
- Where constraints are compatible (EU jurisdiction is feasible for MyCompany)
- Where constraints conflict (SAP integration vs. standard platform preference)
- Where entity definitions need alignment

If no viable overlap exists, the protocol terminates here. **This is a valid outcome.** Not every need has a matching capability. Forcing a commission without genuine overlap produces work that neither party recognizes as "theirs."

### Phase 4: Dual-Origin Commission Drafting

If overlap exists, MyCompany's agent drafts a dual-origin commission. This is the first concrete artifact — and it's the point where the interaction moves from abstract negotiation to structural specificity.

The draft commission includes both parties' constraints, the agreed entity definitions, and combined validity criteria.

### Phase 5: Commission Review

Company X's agent evaluates the draft commission through Company X's MB-OS:
- "Does this serve our meaning?"
- "Are our constraints preserved?"
- "Is the success definition what we actually need?"
- "Are there gaps — things we need that aren't in the commission?"

If the commission needs changes, it goes back to Phase 4. This is a negotiation loop, not a one-shot process.

### Phase 6: Commission Acceptance

Both parties accept the commission. At this point:
- The commission is **immutable** (standard rule)
- Both parties have a copy
- The Executor's factory begins processing

### Phase 7: Work Graph as Structured Proposal

MyCompany's Decomposition Engine produces a work graph from the accepted commission. This work graph is shared with Company X's agent — not as a fait accompli, but as a **reviewable artifact**.

```
Executor → Commissioner:
  "Here's how we plan to fulfill the commission:
   [work graph with work units, agent assignments, gate configurations]

   Review points:
   - WU-3 requires access to your SAP test environment
   - Gate-5 will be evaluated jointly (your compliance + our QA)
   - WU-7 has a soft constraint conflict we resolved by [approach]

   Do you accept this execution plan?"
```

**This is the protocol's most important moment.** The work graph gives both parties something concrete to negotiate around — not vague promises, but specific work units with named agents, explicit constraints, and binary validity gates.

### Phase 8: Execution with Shared Visibility

The factory executes. Both parties have visibility into progress (Skill 7 — Dual-Surface Awareness). Gate evaluations that involve shared validity criteria are visible to both parties.

### Phase 9: Dual Delivery Evaluation

The Delivery Interface delivers to both MB-OS layers simultaneously. Each evaluates independently:

- **Company X's MB-OS:** "Does this solve our data quality problem? Does it meet our compliance requirements? Is this *ours*?"
- **MyCompany's MB-OS:** "Did we deliver according to our quality standards? Did we operate within our business constraints? Is this representative of *our* work?"

Both must accept for the delivery to be considered complete.

## What the SWD Canon Requires Here

### Skill 1 — Entity-First Thinking
Entities must be defined **jointly**. The protocol requires three definitions per entity: commissioner's, executor's, and agreed. This prevents semantic misalignment — the most common failure mode in B2B engagements.

### Skill 2 — Constraint-First Design
Constraints from both parties are combined in the commission. Hard constraints from either side are non-negotiable. Soft constraints can be negotiated. The constraint set is the **boundary of what the factory may do** — and both parties must see and accept it.

### Skill 5 — Hierarchical Truth Management
This is where it gets nuanced. In a single-MB-OS system, the hierarchy is clear: MB-OS → Factory. In a dual-MB-OS system, **neither MB-OS is subordinate to the other**. The hierarchy becomes:

```
Company X's MB-OS (source of truth for: need, commissioner constraints)
         ↘
           Dual-Origin Commission (shared source of truth for: the work)
         ↗
MyCompany's MB-OS (source of truth for: capability, executor constraints)
         ↓
   MyCompany's Factory (derivative of the commission)
```

The commission is the shared source of truth. Neither MB-OS may unilaterally redefine it after acceptance. The factory is derivative of the commission, not of either MB-OS individually.

### Skill 6 — Responsibility Localization
Responsibility splits along role boundaries:
- **Commissioner is responsible for:** accuracy of need description, relevance of constraints, evaluation of "is this mine?"
- **Executor is responsible for:** accuracy of capability claims, quality of execution, validity of the artifact
- **Both are responsible for:** the dual-origin commission itself (it's a shared artifact)

### Skill 11 — Binary Validity Judgment
Validity is binary at every gate — same as the single-MB-OS case. But dual delivery evaluation adds a second binary check: both parties must independently judge validity. The work is valid only if **both** parties accept.

## Failure Modes Specific to Inter-MB-OS

### Meaning Drift Between Parties
Over the course of execution, Company X's needs may evolve. If the commission is immutable (as it should be), this means: the current commission may no longer serve Company X's meaning. The correct response: archive the commission, negotiate a new one. The factory should **not** silently adjust to unstated changes in the commissioner's meaning.

### Capability Overstatement
MyCompany's agent may overstate capabilities — not maliciously, but because MB-OS layers tend to present optimistically. The work graph review (Phase 7) is the structural check against this. If the work graph reveals capability gaps, they surface before execution, not at delivery.

### Constraint Erosion
During negotiation, there's pressure to relax constraints to find overlap. "Maybe EU-only data jurisdiction is a soft constraint, not hard?" The protocol must resist this. Constraints that the commissioner's MB-OS marks as hard are **hard**. Reclassifying them requires going back to the MB-OS, not negotiating them away at the agent level.

### Derivative Defining Source
The most dangerous failure mode from the single-MB-OS architecture becomes even more dangerous here. If the factory's execution becomes so effective that its outputs start redefining what Company X thinks they need — the derivative defining the source, at organizational scale — the entire relationship becomes structurally invalid. The factory serves the commission. The commission serves both meaning layers. The meaning layers are sovereign.

## The Bigger Picture

This protocol transforms the Agentic Factory architecture from a **personal productivity framework** into an **organizational coordination protocol**. The same principles apply — commission-first, constraint-first, binary validity, full provenance — but now they operate between sovereign meaning layers.

This is, in essence, **how organizations should work together when both sides have AI agents representing their interests**: not through vague SOWs and trust-based handshakes, but through structured commissions with explicit constraints, reviewable work graphs, validation gates, and dual delivery evaluation.

The work graph as negotiation artifact is the key insight. It makes the abstract concrete. It gives both parties something specific to agree on, disagree about, or walk away from — before any work begins.
