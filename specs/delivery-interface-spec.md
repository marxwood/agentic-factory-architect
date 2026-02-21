# Delivery Interface Protocol Specification

## Overview

The Delivery Interface is the factory's output boundary — where the completed, validated, assembled work product is delivered back to the MB-OS. It ensures that every delivery is traceable, attested, and transparent about both what was achieved and what remains unresolved.

## Delivery Package Schema

```yaml
delivery_package:
  header:
    delivery_id: string          # Unique delivery identifier
    commission_id: string        # The commission this fulfills
    factory_id: string           # Which factory produced this
    factory_version: string      # Which version of the factory
    timestamp: ISO-8601          # When delivery was completed
    status: enum                 # COMPLETE | PARTIAL | ESCALATED

  artifact:
    content: any                 # The actual deliverable
    format: string               # Format specification
    size: string                 # Size/length metrics
    content_hash: string         # Integrity verification hash

  provenance:
    commission_snapshot:          # Frozen copy of the commission as received
      id: string
      version: string
      intent: string
      constraints_count: integer

    decomposition:
      work_graph_version: string
      total_work_units: integer
      parallel_branches: integer

    execution_summary:
      agents_involved:
        - agent_id: string
          agent_version: string
          work_units_handled: integer

      gate_evaluations:
        total_evaluations: integer
        passes: integer
        returns: integer
        escalations: integer

      timeline:
        started: ISO-8601
        completed: ISO-8601
        total_duration: duration

    assembly:
      branches_merged: integer
      consistency_check: PASS | FAIL
      completeness_check: PASS | FAIL
      coherence_check: PASS | FAIL

  validity_attestation:
    all_criteria_met: boolean
    criteria_results:
      - criterion: string        # From the commission's validity criteria
        result: PASS | FAIL
        evaluator: string
        evidence: string         # What supports this judgment

    overall_validity: VALID | INVALID | PARTIAL
    attestation_note: string     # Any qualifications to the attestation

  unresolved_tensions:
    - tension_id: string
      description: string        # What the tension is
      source: string             # Where it was discovered
      impact: string             # How it affects the deliverable
      options: string[]          # Possible resolutions
      recommendation: string?    # Factory's suggestion, if applicable

  metadata:
    total_tokens_consumed: integer?
    total_agent_invocations: integer
    total_gate_evaluations: integer
    escalations_resolved: integer
    escalations_unresolved: integer
```

## Delivery Statuses

### COMPLETE
All validity criteria were met. All work units completed. Assembly checks passed. No unresolved escalations.

### PARTIAL
The factory produced output, but some aspect is incomplete:
- Some work units failed and could not be resolved
- Some validity criteria were not met
- Assembly identified gaps

A partial delivery **always** includes an explicit account of what's missing and why.

### ESCALATED
The factory encountered something it cannot resolve and is returning the entire commission for human judgment:
- Fundamental contradiction in the commission
- Required capability not available in any agent
- Recursive returns exhausted at a critical gate

An escalated delivery may include partial work already completed.

## MB-OS Evaluation

When the MB-OS receives a delivery, it performs its own evaluation — distinct from the factory's internal validation:

1. **"Is this mine?"** — Does this serve the meaning that commissioned it?
2. **Alignment check** — Does the artifact align with the originating values?
3. **Narrative fit** — Does this fit into the person's current narrative arc?
4. **Tension resolution** — If this commission was born from a tension, does this delivery help resolve it?

The MB-OS may:
- **Accept** — integrate the artifact into the meaning layer
- **Return** — send back to the factory with additional context
- **Archive** — accept for record but not integrate (useful != mine)
- **Reject** — the artifact doesn't serve the originating meaning

## Audit Trail

Every delivery creates a permanent audit record. The audit trail answers:

1. What was commissioned? (commission snapshot)
2. How was it decomposed? (work graph)
3. Who did what? (agent assignments)
4. What did each gate find? (gate evaluations)
5. How was it assembled? (assembly report)
6. What was delivered? (artifact + attestation)
7. What remains unresolved? (tensions)

The audit trail is **never deleted**. It may be archived, but it is always retrievable. This is how the system maintains accountability over time.
