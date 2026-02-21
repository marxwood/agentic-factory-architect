# SWD Canon Integration

## How the Canon Binds the System

The [SWD Canon Skills](https://github.com/marxwood/swd-canon-skills) aren't a meta-layer sitting above the Agentic Factory architecture — they're the **discipline required to operate each layer correctly**. They're what keeps a human-designed meaning layer from drifting, what keeps the MB-OS honest, and what keeps Agentic Factories from becoming autonomous systems that work for themselves.

## Skill-by-Skill Mapping

### Skill 1 — Entity-First Thinking
**Factory Layer: Commission Register + Decomposition Engine**

Factories produce outputs. If the entities those outputs are about aren't clearly defined, agents will infer them — and infer them wrong. An agentic factory that can't name its entities will produce incoherent, non-composable outputs.

**Application:** Every commission must name its entities explicitly. Every work unit in the decomposition graph must declare which entity it operates on. The Assembly Layer checks entity consistency across branches.

### Skill 2 — Constraint-First Design
**Factory Layer: Commission Register + Validation Gates**

Agents optimize aggressively. Without explicit constraints, they will find paths to the goal that violate things you didn't think to specify.

> "Optimization without constraints amplifies invalid structures."

At agent scale, this happens fast and at volume. Constraints aren't a governance nicety; they're a **safety requirement**.

**Application:** Constraints are first-class citizens in the commission. They propagate to every work unit. Validation gates check constraint satisfaction with binary judgment.

### Skill 3 — Structural Validity Thinking
**Factory Layer: Validation Gates + Assembly Layer**

A factory can be performing (producing outputs, shipping results) while being **structurally invalid** — agents cutting corners, meaning drifting from the original commission.

Performance ≠ validity.

**Application:** Gates don't check "does this look good?" They check "is this structurally valid against the commission?" The Assembly Layer checks coherence, not just completeness.

### Skill 5 — Hierarchical Truth Management
**Factory Layer: MB-OS ↔ Factory Boundary**

This is the most critical integration point. The MB-OS is the **source of truth**. The factory is a derivative — it projects downward from the meaning layer, not upward.

**The failure mode:** An agentic factory starts optimizing so effectively that its outputs begin to redefine the goals. The derivative starts defining the source. **This is catastrophic.**

**Application:** Commissions are immutable. The factory cannot modify its own commission. If the factory's execution reveals that the commission was inadequate, it surfaces the gap — it doesn't fill it autonomously.

### Skill 6 — Responsibility Localization
**Factory Layer: Agent Registry + Orchestration Layer**

In a multi-agent factory, diffused responsibility is the default state. Who is responsible when an agent misinterprets the brief? Which agent owns the constraint? When the factory produces something wrong, where does correction happen?

**Application:** Every work unit has a single responsible agent. Every constraint has a named owner. Every gate evaluation is attributed. The provenance record makes responsibility traceable at every point.

### Skill 7 — Dual-Surface Awareness
**Factory Layer: All Components**

Agentic factories operate entirely at the machine surface. But their outputs are consumed by humans. The design of every agent's inputs, outputs, and constraints must be **legible to both**.

**Application:** An agent-facing spec that humans can't inspect is a black box with consequences. Every intermediate state is logged in a format that both machines can process and humans can read. The Delivery Interface includes human-readable provenance alongside machine-readable data.

### Skill 9 — Explicit Change Control
**Factory Layer: Factory Governance**

Factories evolve. Agents get updated, models change, prompts drift. Without explicit change control at the factory layer, you have no way to know whether a change in output reflects a change in your goals or a change in the factory's behavior.

**Application:** Factory configurations are versioned. Agent versions are tracked. Changes go through the governance protocol: declare → version → validate → compare → promote/reject → archive.

### Skill 11 — Binary Validity Judgment
**Factory Layer: Validation Gates**

At factory scale, "close enough" is how errors compound. A factory that accepts partial validity at each step produces outputs that are **structurally wrong by accumulation**, even if each individual step seemed reasonable.

**Application:** Validation gates return Pass, Return, or Escalate. There is no "73% valid." Within the scope of what the gate can evaluate, the judgment is binary.

## The Canon as Operating Discipline

The Canon skills aren't optional best practices. They're the **operating discipline** that prevents each layer from degrading:

| Layer | Without Canon | With Canon |
|-------|--------------|------------|
| MB-OS | Values drift silently | Values are explicit, drift is detected |
| Commission | Scope creeps | Commission is immutable, scope is bounded |
| Factory | Agents optimize into violations | Constraints are enforced at every gate |
| Delivery | Outputs look good but aren't traceable | Full provenance, binary validity |
| Governance | Factories evolve silently | Changes are explicit, versioned, validated |
