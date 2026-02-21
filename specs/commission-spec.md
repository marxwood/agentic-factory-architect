# Commission Specification Format

## Overview

A commission is the formal interface between the MB-OS and an Agentic Factory. It replaces the concept of a "prompt" or "task description" with a structured, immutable specification that carries meaning, context, constraints, and validity criteria.

## Commission Schema

```yaml
commission:
  id: string                    # Unique identifier (UUID v4)
  version: string               # "1.0.0" — commissions are versioned
  timestamp: ISO-8601           # When this commission was created
  status: enum                  # DRAFT | ISSUED | ACCEPTED | COMPLETED | ARCHIVED

  origin:
    meaning_source: string      # Which part of the meaning layer this comes from
    value_alignment: string[]   # Which values this commission serves
    narrative_context: string   # Where this fits in the person's narrative arc
    tension_reference: string?  # If this addresses an active tension, reference it

  intent:
    description: string         # Semantic description of what's being commissioned
    success_definition: string  # What "done" looks like, in meaning terms
    anti_goals: string[]        # What this commission is explicitly NOT trying to achieve

  entities:
    - name: string              # Entity name
      type: string              # Entity type (person, concept, organization, artifact, etc.)
      definition: string        # What this entity is, within this commission's context
      relationships: string[]   # How this entity relates to other entities in the commission

  constraints:
    hard:                       # Non-negotiable — violation = invalid
      - description: string
        rationale: string       # Why this constraint exists
    soft:                       # Preferred but negotiable — violation = escalate
      - description: string
        rationale: string
        escalation_path: string # Who decides if this can be relaxed

  validity_criteria:
    - criterion: string         # A binary test
      evaluator: string         # Who/what evaluates this (gate type, human, etc.)
      scope: string             # What part of the output this applies to

  scope_boundary:
    in_scope: string[]          # Explicitly included
    out_of_scope: string[]      # Explicitly excluded
    ambiguous: string[]         # Acknowledged gray areas — escalate if encountered

  delivery:
    format: string              # Expected output format
    audience: string            # Who will consume this output
    urgency: enum               # LOW | MEDIUM | HIGH | CRITICAL
    deadline: ISO-8601?         # Optional hard deadline

  metadata:
    parent_commission: string?  # If this is a sub-commission, reference the parent
    factory_type: string?       # Preferred factory type, if any
    notes: string?              # Freeform context for the factory
```

## Commission Lifecycle

```
DRAFT → ISSUED → ACCEPTED → COMPLETED
                      ↓
                   ARCHIVED (if commission needs to change, archive and re-issue)
```

## Immutability Rule

Once a commission's status moves to `ISSUED`, the commission body is **immutable**. If changes are needed:

1. The current commission is moved to `ARCHIVED`
2. A new commission is issued with a reference to the archived one
3. The new commission explicitly states what changed and why

This prevents scope drift at the root.

## Example Commission

```yaml
commission:
  id: "c-2026-0221-research-factory-001"
  version: "1.0.0"
  timestamp: "2026-02-21T10:00:00Z"
  status: ISSUED

  origin:
    meaning_source: "career.transition.exploration"
    value_alignment: ["intellectual_growth", "autonomy", "meaningful_work"]
    narrative_context: "Exploring transition from engineering management to independent research"
    tension_reference: "tension.security_vs_exploration"

  intent:
    description: "Produce a structured landscape analysis of the independent AI safety research space"
    success_definition: "A clear map of who is doing what, where the gaps are, and where my skills align"
    anti_goals:
      - "Do not produce a recommendation or decision — this is information gathering"
      - "Do not optimize for the most prestigious path"

  entities:
    - name: "AI Safety Research"
      type: "field"
      definition: "Academic and independent research focused on ensuring AI systems are beneficial"
      relationships: ["contains: organizations", "requires: skills", "produces: artifacts"]
    - name: "Independent Researcher"
      type: "role"
      definition: "A researcher operating outside traditional academic or corporate structures"
      relationships: ["requires: funding_model", "requires: skills"]

  constraints:
    hard:
      - description: "Only include organizations and researchers with publicly verifiable work"
        rationale: "The output must be fact-checkable"
      - description: "Do not include the user's own assessment of their skills — only map the landscape"
        rationale: "Self-assessment is the MB-OS's job, not the factory's"
    soft:
      - description: "Prefer depth over breadth — 10 well-analyzed entities over 50 shallow ones"
        rationale: "Decision-quality information requires depth"
        escalation_path: "meaning_owner"

  validity_criteria:
    - criterion: "Every named entity has a verifiable source"
      evaluator: "source_verification_gate"
      scope: "all_entities"
    - criterion: "The landscape map is internally consistent (no contradictions)"
      evaluator: "consistency_gate"
      scope: "assembled_output"

  scope_boundary:
    in_scope: ["organizations", "key_researchers", "funding_models", "skill_requirements", "open_problems"]
    out_of_scope: ["the user's personal fit", "salary comparisons", "career advice"]
    ambiguous: ["adjacent fields like AI governance — include if directly relevant"]

  delivery:
    format: "structured_brief"
    audience: "the meaning owner (human)"
    urgency: MEDIUM
    deadline: null
```
