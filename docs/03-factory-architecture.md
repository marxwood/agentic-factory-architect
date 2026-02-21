# Agentic Factory Architecture

## The Core Problem a Factory Solves

A single agent can handle a single task within a single domain. But meaningful work — the kind commissioned by an MB-OS — is almost never single-task, single-domain. It's multi-step, requires different capabilities at different stages, needs internal quality control, and must remain traceable back to the meaning that commissioned it.

A factory is the answer to: **how do you organize multiple agents to produce structurally valid work at scale, without losing coherence with the meaning layer above?**

## Architectural Principles

Derived from the [SWD Canon](https://github.com/marxwood/swd-canon-skills):

### Principle 1: Every factory has a commission, not a prompt

A prompt is stateless. A commission carries context: the originating meaning (from the MB-OS), the constraints that bind it, the entities involved, the definition of done, and the validity criteria.

The commission is the factory's source of truth. Nothing inside the factory may redefine it. *(Skill 5 — Hierarchical Truth Management)*

### Principle 2: Every agent has a named role with bounded authority

No agent operates as a generalist. Each has an explicit capability declaration: what it can do, what it must not do, what inputs it requires, what outputs it guarantees. Authority is scoped. No agent may exceed its declaration. *(Skill 4 — Design as Contract, Skill 6 — Responsibility Localization)*

### Principle 3: Work products flow through validation gates

Between stages, there are gates — not just handoffs. A gate checks structural validity against the commission's constraints. If a work product fails a gate, it doesn't proceed. It returns to the producing agent with an explicit violation report. *(Skill 11 — Binary Validity Judgment)*

### Principle 4: The factory is inspectable at every layer

No black boxes. Every intermediate state, every agent decision, every gate evaluation is logged and legible. A human can audit any stage. An agent can reason about any stage. *(Skill 7 — Dual-Surface Awareness)*

### Principle 5: Change to the factory is explicit and declared

If an agent is swapped, a model is updated, a constraint is relaxed, a gate is modified — that change is versioned, named, and traceable. There is no silent evolution of the factory. *(Skill 9 — Explicit Change Control)*

---

## The Seven Components

### 1. The Commission Register

The entry point. This is where the MB-OS deposits its commission.

**The register holds:**
- **Origin** — which part of the meaning layer this comes from (a value, a goal, a tension, a narrative arc)
- **Intent** — what the factory is being asked to produce, in semantic terms (not "write me a report" but "produce a structured analysis of X that surfaces Y under constraints Z")
- **Entities** — the named things this commission is about *(Skill 1)*
- **Constraints** — what must not happen, what is non-negotiable *(Skill 2)*
- **Validity criteria** — the binary test: how do we know the output is valid vs. merely complete?
- **Scope boundary** — what is explicitly outside this commission

**Critical rule:** The Commission Register is **immutable** once accepted. If the commission needs to change, a new commission is issued. The old one is archived, not edited. This prevents scope drift at the root.

### 2. The Decomposition Engine

Takes the commission and breaks it into a **work graph** — not a linear pipeline, but a directed graph of tasks with dependencies, parallelism opportunities, and merge points.

**Each node in the graph is a work unit with:**
- A named entity it operates on
- Required inputs (from upstream nodes or the commission itself)
- A capability requirement (what kind of agent can handle this)
- Output specification (what it must produce, in what structure)
- Constraints inherited from the commission + any constraints specific to this step

**Critical rule:** Decomposition is not execution. It is a structural claim about how the commission can be fulfilled. That claim can be wrong, and catching it here is cheaper than catching it downstream. The decomposition is a **reviewable artifact** before any agent touches anything.

### 3. The Agent Registry

A catalog of available agents, each with an explicit **capability contract** *(Skill 4)*:

- **Identity** — what this agent is, what model/system it runs on
- **Capabilities** — what it can do, what domains it covers
- **Constraints** — what it must not do, what it is not qualified for
- **Input/Output contracts** — what it requires, what it guarantees
- **Trust level** — how much autonomy it can exercise (can it make judgment calls, or must it escalate?)
- **Version** — which version of this agent is currently active

The registry is not a flat list. It's a **typed, constrained catalog**. When the Decomposition Engine produces a work graph, each node's capability requirement is matched against the registry. If no agent matches a requirement, the factory **surfaces that gap** rather than forcing a bad fit.

### 4. The Orchestration Layer

The runtime. It takes the work graph and the agent assignments and manages execution:

- **Scheduling** — which work units run now, which wait for dependencies
- **Parallelism** — independent branches execute concurrently
- **State tracking** — every work unit has a named state: `pending`, `active`, `completed`, `failed`, `returned`
- **Escalation routing** — when an agent encounters something outside its authority, it escalates rather than guesses. The escalation goes to a named entity (another agent, or the human operator via the MB-OS)
- **Timeout and deadlock detection** — if a branch stalls, the orchestrator surfaces it

**Critical design choice:** The orchestration layer is **not intelligent**. It does not make judgment calls. It is a scheduler with constraint awareness. Intelligence belongs in the agents and in the gates, not in the plumbing.

### 5. The Validation Gates

**The most important component.**

Gates sit between stages of the work graph and evaluate whether a work product is **structurally valid** — not just complete or plausible.

**A gate checks:**
- **Commission alignment** — does this output still trace back to the original commission's intent and constraints?
- **Entity integrity** — are the entities named in this output consistent with how they were defined in the commission?
- **Constraint satisfaction** — have any constraints been violated? Binary answer. *(Skill 11)*
- **Contract fulfillment** — does the output match the producing agent's output contract?
- **Semantic drift detection** — has the meaning shifted from what was commissioned? *(Skill 5)*

**Gates produce one of three outcomes:**
1. **Pass** — work product proceeds to the next stage
2. **Return** — work product goes back to the producing agent with a specific violation report
3. **Escalate** — the gate itself cannot determine validity; a human or higher-authority agent must judge

**Gates are not just QA. They are the immune system of the factory.** Without them, errors propagate and compound. With them, violations are caught at the earliest possible point.

### 6. The Assembly Layer

Where validated outputs from multiple branches are **composed** into the final deliverable. This is where the graph converges.

Assembly is not concatenation. It is **semantic composition** — ensuring that parts produced by different agents, at different times, in different branches, form a coherent whole that satisfies the original commission.

**The Assembly Layer checks:**
- **Internal consistency** — do the parts contradict each other?
- **Completeness** — is anything missing relative to the commission?
- **Coherence** — does the assembled output read/function as a unified artifact, not a collage?
- **Traceability** — can every part of the output be traced to a work unit, which traces to a commission requirement?

If assembly fails, the factory doesn't ship a broken output. It surfaces what's broken and either routes back to the relevant branch or escalates.

### 7. The Delivery Interface

The factory's output boundary. This is where the completed, validated, assembled work product is delivered back to the MB-OS.

**The delivery includes:**
- **The artifact itself** — whatever was commissioned
- **The provenance record** — a complete audit trail: commission → decomposition → agent assignments → work products → gate evaluations → assembly → delivery
- **A validity attestation** — the factory's claim that this output satisfies the commission's validity criteria
- **Unresolved tensions** — anything the factory surfaced but could not resolve (contradictions in the commission, gaps in available capability, judgment calls that were escalated but not answered)

The MB-OS receives this and evaluates it against its meaning layer. The factory says "this is valid." The MB-OS asks **"is this mine?"** — does this serve the meaning that commissioned it?

---

## The Full Flow

```
MB-OS (meaning layer)
    ↓ commission
[Commission Register]          ← immutable intent + constraints
    ↓
[Decomposition Engine]         ← work graph (reviewable plan)
    ↓
[Agent Registry] → matching    ← capability contracts
    ↓
[Orchestration Layer]          ← scheduling, state, escalation
    ↓ ↑ (return loops)
[Validation Gates]             ← binary validity, constraint checking
    ↓
[Assembly Layer]               ← semantic composition, coherence
    ↓
[Delivery Interface]           ← artifact + provenance + attestation
    ↓
MB-OS (evaluation against meaning)
```

---

## What Makes This Different from Existing Agent Orchestration

Most current agent orchestration systems (CrewAI, AutoGen, LangGraph, etc.) are pipeline-first. This architecture is commission-first and constraint-first.

| Aspect | Existing Systems | Agentic Factory Architect |
|--------|-----------------|--------------------------|
| Starting point | Task description / prompt | Semantic commission from MB-OS |
| Quality control | Output checking at the end | Validation gates at every transition |
| Validity model | Gradient scores (0.0–1.0) | Binary: valid or not valid |
| Traceability | Logs | Full provenance chain |
| Goal stability | Goals can shift mid-execution | Immutable commissions |

## The Meta-Question: Who Builds the Factory?

The factory architecture itself is an artifact — and by the Canon's own rules, it must have an owner, a version, explicit constraints, and be subject to binary validity judgment.

This means: you don't just run factories. You **design them, version them, validate them, and evolve them explicitly**.

The factory is itself an entity in the system. It has states, relationships, constraints, and a lifecycle.

Which means there's another layer: **Factory governance** — the rules about how factories are created, modified, audited, and retired. That's the Canon applied to the factory layer.
