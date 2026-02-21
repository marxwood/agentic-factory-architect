# Validation Gate Protocol Specification

## Overview

Validation Gates are the immune system of the Agentic Factory. They sit between stages of the work graph and evaluate whether a work product is **structurally valid** — not just complete or plausible. Gates enforce binary validity judgment at every transition.

## Gate Schema

```yaml
validation_gate:
  identity:
    id: string                    # Unique gate identifier
    name: string                  # Human-readable name
    version: string               # Semantic version
    gate_type: enum               # COMMISSION_ALIGNMENT | ENTITY_INTEGRITY | CONSTRAINT_SATISFACTION | CONTRACT_FULFILLMENT | SEMANTIC_DRIFT | COMPOSITE

  position:
    work_graph_edge: string       # Which edge in the work graph this gate sits on
    upstream_node: string         # The work unit that produced the input
    downstream_node: string       # The work unit that would receive the output (if passed)

  evaluation_criteria:
    checks:
      - name: string              # Name of the check
        description: string       # What this check evaluates
        type: enum                # BINARY | THRESHOLD_BINARY | COMPOSITE
        threshold: number?        # For THRESHOLD_BINARY: the binary cutoff
        weight: number?           # For COMPOSITE: relative weight of this check
        required: boolean         # If true, failure of this check = automatic Return

  input_requirements:
    - name: string
      source: string              # Where this input comes from (work product, commission, etc.)
      type: string

  output:
    decision: enum                # PASS | RETURN | ESCALATE
    report:
      summary: string             # Human-readable summary of evaluation
      checks_passed: string[]     # Which checks passed
      checks_failed: string[]     # Which checks failed
      violations: Violation[]     # Detailed violation reports
      recommendations: string[]   # For RETURN: what the producing agent should fix
      escalation_reason: string?  # For ESCALATE: why the gate can't decide

  escalation:
    target: string                # Where escalations go
    timeout: duration             # How long to wait for escalation response
    default_on_timeout: enum      # RETURN | HOLD (never default to PASS)
```

## Gate Types

### COMMISSION_ALIGNMENT
Checks whether the work product still serves the original commission's intent.

**Evaluates:**
- Does the output address what the commission asked for?
- Has the scope shifted from the commission's boundaries?
- Are anti-goals being violated?

### ENTITY_INTEGRITY
Checks whether entities in the work product are consistent with the commission's entity definitions.

**Evaluates:**
- Are all referenced entities defined in the commission?
- Are entity definitions used consistently?
- Have new entities appeared without declaration?

### CONSTRAINT_SATISFACTION
Checks whether any hard or soft constraints have been violated.

**Evaluates:**
- Hard constraints: binary pass/fail
- Soft constraints: violation triggers escalation, not automatic return
- Constraint rationale is preserved in the evaluation

### CONTRACT_FULFILLMENT
Checks whether the producing agent fulfilled its output contract.

**Evaluates:**
- Are all guaranteed outputs present?
- Do outputs match their declared types and formats?
- Were any side effects produced that weren't declared?

### SEMANTIC_DRIFT
Checks whether the meaning has shifted from what was commissioned.

**Evaluates:**
- Does the work product's framing match the commission's intent?
- Has the tone, perspective, or emphasis shifted in ways not authorized?
- Is the derivative still faithful to the source? (Skill 5)

### COMPOSITE
Combines multiple gate types into a single evaluation point. Useful for high-stakes transitions where multiple dimensions need checking simultaneously.

## Evaluation Protocol

```
1. RECEIVE work product from upstream node
2. LOAD evaluation criteria for this gate
3. LOAD relevant context (commission, entity definitions, constraints)
4. EXECUTE each check independently
5. AGGREGATE results:
   - Any required check failed → RETURN
   - All required checks passed, some optional failed → ESCALATE
   - All checks passed → PASS
6. PRODUCE evaluation report
7. ROUTE:
   - PASS → forward to downstream node
   - RETURN → send back to upstream node with violation report
   - ESCALATE → send to escalation target with context
```

## Return Protocol

When a gate returns a work product:

```yaml
return_report:
  gate_id: string
  work_unit_id: string
  producing_agent_id: string
  violations:
    - check_name: string
      expected: string           # What was expected
      actual: string             # What was found
      severity: enum             # HARD_VIOLATION | SOFT_VIOLATION
      recommendation: string     # What the agent should do differently
  return_count: integer          # How many times this work unit has been returned
  max_returns: integer           # After this many returns, escalate instead
```

**Return limits:** A work unit can only be returned a configurable number of times (default: 3). After that, the gate escalates rather than entering an infinite loop.

## Escalation Protocol

When a gate escalates:

```yaml
escalation_report:
  gate_id: string
  work_unit_id: string
  reason: enum                   # CANNOT_DETERMINE | MAX_RETURNS_EXCEEDED | SOFT_CONSTRAINT_VIOLATION | AMBIGUOUS_SCOPE
  context: string                # Full context for the escalation target
  options:                       # Possible resolutions the escalation target can choose
    - action: string
      consequence: string
  deadline: ISO-8601?            # When a response is needed
```

## Gate Independence

Gates must be **independent of the producing agent**. A gate:
- Cannot be operated by the same agent that produced the work product
- Cannot be influenced by the producing agent's confidence or self-assessment
- Cannot access the producing agent's internal state — only its output

This separation is what makes validation meaningful. Self-assessment is not validation.
