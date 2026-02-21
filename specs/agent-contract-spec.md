# Agent Capability Contract Specification

## Overview

Every agent in an Agentic Factory operates under an explicit capability contract. This contract declares what the agent can do, what it must not do, what it requires, and what it guarantees. No agent operates as a generalist. Authority is bounded and named.

## Contract Schema

```yaml
agent_contract:
  identity:
    id: string                   # Unique agent identifier
    name: string                 # Human-readable name
    version: string              # Semantic version
    type: enum                   # SPECIALIST | EVALUATOR | ASSEMBLER | ROUTER
    model: string?               # Underlying model/system (e.g., "claude-opus-4-6")
    description: string          # What this agent does, in plain language

  capabilities:
    domains: string[]            # What domains this agent covers
    operations: string[]         # What operations it can perform
    output_types: string[]       # What kinds of output it can produce
    languages: string[]?         # Natural languages supported
    max_complexity: enum         # LOW | MEDIUM | HIGH | EXPERT

  constraints:
    must_not: string[]           # Explicit prohibitions
    not_qualified_for: string[]  # Things this agent shouldn't attempt
    requires_escalation: string[]# Situations where this agent must escalate
    max_autonomy: enum           # EXECUTE | PROPOSE | ESCALATE_ALL

  input_contract:
    required_inputs:
      - name: string
        type: string
        description: string
    optional_inputs:
      - name: string
        type: string
        description: string
        default: any?
    input_validation: string[]   # Checks the agent performs on its inputs before processing

  output_contract:
    guaranteed_outputs:
      - name: string
        type: string
        description: string
        validity_check: string   # How to verify this output is valid
    possible_side_effects: string[] # Any effects beyond the direct output
    output_format: string        # Structured format specification

  trust:
    level: enum                  # FULL | SUPERVISED | RESTRICTED
    can_make_judgment_calls: boolean
    can_modify_work_graph: boolean
    can_commission_sub_agents: boolean
    escalation_target: string    # Where this agent escalates to

  performance:
    typical_latency: string      # Expected execution time range
    resource_requirements: string # Computational requirements
    failure_modes: string[]      # Known ways this agent can fail
    retry_policy: enum           # NONE | ONCE | EXPONENTIAL_BACKOFF
```

## Agent Types

### SPECIALIST
Performs domain-specific work. Most agents are specialists.
- Has deep capability in a narrow domain
- Produces work products
- Cannot evaluate its own output (that's the gate's job)

### EVALUATOR
Performs validation at gates. Does not produce work products.
- Has the ability to judge validity against criteria
- Produces Pass/Return/Escalate decisions
- Cannot modify the work product it evaluates

### ASSEMBLER
Composes outputs from multiple branches into coherent wholes.
- Has cross-domain awareness
- Checks consistency, completeness, coherence
- Cannot produce original content — only compose existing validated outputs

### ROUTER
Makes routing decisions in the orchestration layer.
- Matches capability requirements to agents
- Manages scheduling and parallelism
- Cannot execute work units or evaluate outputs

## Contract Enforcement

Contracts are enforced at two points:

1. **Assignment time** — when the Decomposition Engine matches work units to agents, it checks that the work unit's requirements fall within the agent's declared capabilities
2. **Runtime** — the Orchestration Layer monitors agent behavior against its contract. If an agent attempts to exceed its declared authority, the action is blocked and escalated

## Example: Research Analyst Agent

```yaml
agent_contract:
  identity:
    id: "agent-research-analyst-001"
    name: "Research Landscape Analyst"
    version: "2.1.0"
    type: SPECIALIST
    model: "claude-opus-4-6"
    description: "Analyzes research landscapes by mapping entities, relationships, and gaps"

  capabilities:
    domains: ["academic_research", "organizational_analysis", "field_mapping"]
    operations: ["entity_extraction", "relationship_mapping", "gap_analysis", "source_verification"]
    output_types: ["structured_brief", "entity_map", "gap_report"]
    languages: ["en"]
    max_complexity: HIGH

  constraints:
    must_not:
      - "Make recommendations or judgments about what the user should do"
      - "Include unverifiable claims"
      - "Extrapolate beyond available evidence"
    not_qualified_for:
      - "Financial analysis"
      - "Legal assessment"
      - "Personal fit evaluation"
    requires_escalation:
      - "When evidence is contradictory and cannot be resolved from sources"
      - "When scope ambiguity is encountered (in the commission's ambiguous zone)"
    max_autonomy: EXECUTE

  input_contract:
    required_inputs:
      - name: "commission"
        type: "Commission"
        description: "The full commission or derived sub-commission"
      - name: "entity_list"
        type: "Entity[]"
        description: "Named entities to analyze"
    optional_inputs:
      - name: "prior_research"
        type: "Document[]"
        description: "Previously gathered research to build on"
        default: null

  output_contract:
    guaranteed_outputs:
      - name: "entity_analysis"
        type: "EntityAnalysis[]"
        description: "Structured analysis of each entity"
        validity_check: "Each entity has at least one verifiable source"
      - name: "relationship_map"
        type: "RelationshipGraph"
        description: "How entities relate to each other"
        validity_check: "All relationships are bidirectionally consistent"
      - name: "gap_report"
        type: "GapReport"
        description: "Identified gaps in the landscape"
        validity_check: "Each gap references the entities that bound it"
    possible_side_effects: []
    output_format: "JSON + Markdown summary"

  trust:
    level: SUPERVISED
    can_make_judgment_calls: false
    can_modify_work_graph: false
    can_commission_sub_agents: false
    escalation_target: "orchestration_layer"

  performance:
    typical_latency: "30s-5min per entity"
    resource_requirements: "Standard LLM inference"
    failure_modes:
      - "Source unavailable — returns partial with explicit gaps"
      - "Scope ambiguity — escalates rather than guesses"
    retry_policy: ONCE
```
