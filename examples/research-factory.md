# Example: Research Factory

## Scenario

Your MB-OS holds a meaning vector: *"I'm exploring a transition into independent AI safety research."* This connects to your values of intellectual growth, autonomy, and meaningful work — but exists in tension with your value of financial security.

The MB-OS commissions a Research Factory to map the landscape before any decision is made.

## Commission

```yaml
commission:
  id: "c-research-aisafety-001"
  origin:
    meaning_source: "career.transition.exploration"
    value_alignment: ["intellectual_growth", "autonomy", "meaningful_work"]
    tension_reference: "tension.security_vs_exploration"
  intent:
    description: "Map the independent AI safety research landscape"
    success_definition: "Clear picture of who, what, where, and where the gaps are"
    anti_goals:
      - "Do not make a recommendation"
      - "Do not assess my personal fit (that's the MB-OS's job)"
  constraints:
    hard:
      - "Only include publicly verifiable work"
      - "Do not include career advice"
    soft:
      - "Prefer depth over breadth"
  validity_criteria:
    - "Every entity has a verifiable source"
    - "The landscape map is internally consistent"
```

## Decomposition

The Decomposition Engine produces this work graph:

```
[Commission]
    ├── WU-1: Organization Mapping (parallel)
    │     └── Gate: Entity Integrity + Source Verification
    ├── WU-2: Key Researcher Identification (parallel)
    │     └── Gate: Entity Integrity + Source Verification
    ├── WU-3: Funding Model Analysis (parallel)
    │     └── Gate: Constraint Satisfaction
    ├── WU-4: Open Problems Survey (depends on WU-1, WU-2)
    │     └── Gate: Commission Alignment + Semantic Drift
    ├── WU-5: Skill Requirements Extraction (depends on WU-1, WU-4)
    │     └── Gate: Commission Alignment (anti-goal: no personal fit assessment)
    └── WU-6: Assembly (depends on all above)
          └── Gate: Consistency + Completeness + Coherence
```

## Agent Assignments

| Work Unit | Agent | Trust Level |
|-----------|-------|-------------|
| WU-1 | Research Analyst (org-specialist) | SUPERVISED |
| WU-2 | Research Analyst (person-specialist) | SUPERVISED |
| WU-3 | Research Analyst (funding-specialist) | SUPERVISED |
| WU-4 | Synthesis Agent | SUPERVISED |
| WU-5 | Synthesis Agent | RESTRICTED (anti-goal enforcement) |
| WU-6 | Assembly Agent | SUPERVISED |

## Execution Flow

### Phase 1: Parallel Research (WU-1, WU-2, WU-3)
Three specialist agents work simultaneously:
- WU-1 maps organizations (MIRI, ARC, Redwood, CAIS, independent groups)
- WU-2 identifies key researchers and their focus areas
- WU-3 analyzes funding models (grants, fellowships, self-funded, corporate partnerships)

### Phase 1 Gates
- **WU-1 Gate Result:** PASS — all organizations have verifiable sources
- **WU-2 Gate Result:** RETURN — one researcher included without public work → agent corrects and resubmits → PASS
- **WU-3 Gate Result:** PASS

### Phase 2: Synthesis (WU-4, WU-5)
- WU-4 surveys open problems using outputs from WU-1 and WU-2
- WU-5 extracts skill requirements from WU-1 and WU-4

### Phase 2 Gates
- **WU-4 Gate Result:** PASS
- **WU-5 Gate Result:** RETURN — agent included a line about "your engineering background would be relevant" → violates anti-goal → agent removes personal fit assessment → PASS

### Phase 3: Assembly (WU-6)
Assembly Agent composes all validated outputs into a structured brief.

**Assembly checks:**
- Consistency: PASS (no contradictions between branches)
- Completeness: PASS (all commission scope items covered)
- Coherence: PASS (reads as unified document)
- Traceability: PASS (every claim traces to a work unit)

## Delivery

```yaml
delivery:
  status: COMPLETE
  validity_attestation:
    all_criteria_met: true
    overall_validity: VALID
  unresolved_tensions:
    - description: "The funding landscape appears to be shifting — some models marked as 'active' may be in transition"
      impact: "Some funding information may have a short shelf life"
      recommendation: "Re-commission in 3 months for updated data"
  provenance:
    agents_involved: 5
    gate_evaluations: 7
    returns: 2
    escalations: 0
```

## MB-OS Evaluation

The MB-OS receives the landscape brief and evaluates:
- **"Is this mine?"** — Yes. This serves the exploration value and addresses the career tension.
- **Narrative fit** — This fits the current "exploration" chapter.
- **Next step** — The Coherence Engine notes that the meaning layer now has enough information to surface the security/exploration tension more precisely. The Decision Interface can now present a more informed choice.

The factory didn't decide anything. It provided the structured input the meaning layer needs to navigate the tension. The boundary was maintained.
