# Example: Decision Factory

## Scenario

Your MB-OS surfaces a tension: you've received a job offer from a prestigious AI lab. It's high-status, well-compensated, and intellectually stimulating. But it conflicts with your stated commitment to autonomy and your narrative arc of building something independent.

The Coherence Engine flags this: *"This decision sits at the intersection of three active values: financial security, intellectual growth, and autonomy. These values are currently in tension."*

The MB-OS commissions a Decision Factory — not to decide, but to produce the **structured input you need to decide well**.

## Commission

```yaml
commission:
  id: "c-decision-joboffer-001"
  origin:
    meaning_source: "career.decision.offer_evaluation"
    value_alignment: ["financial_security", "intellectual_growth", "autonomy"]
    tension_reference: "tension.security_vs_autonomy"
    narrative_context: "6 months into exploration phase; first concrete offer that tests commitment"
  intent:
    description: "Produce a decision-ready artifact that maps this job offer against my meaning layer"
    success_definition: "I can see exactly what I'm choosing and what I'm giving up, through the lens of my values"
    anti_goals:
      - "Do not recommend a decision"
      - "Do not optimize for any single value — hold the tension"
      - "Do not frame either option as obviously correct"
  entities:
    - name: "The Offer"
      definition: "Specific job offer from [Lab X] for [Role Y]"
    - name: "The Independent Path"
      definition: "Continuing to build toward independent research/creation"
    - name: "Financial Security"
      definition: "As defined in meaning layer: ability to sustain current life without anxiety"
    - name: "Autonomy"
      definition: "As defined in meaning layer: freedom to choose what to work on and when"
    - name: "Intellectual Growth"
      definition: "As defined in meaning layer: continuous deepening of understanding in chosen domains"
  constraints:
    hard:
      - "Present both options with equal rigor — no bias toward either"
      - "Reference actual meaning layer definitions, not generic definitions"
      - "Do not introduce new values — only work with what's in the meaning layer"
    soft:
      - "Include temporal dimension — how does each option play out over 1, 3, 5 years?"
      - "Surface precedents — has the user faced similar tensions before? What happened?"
  validity_criteria:
    - "Both options are analyzed with equal depth"
    - "Every value referenced matches the meaning layer's definition"
    - "The output holds the tension rather than resolving it"
```

## Decomposition

```
[Commission]
    ├── WU-1: Offer Analysis (what exactly is being offered)
    │     └── Gate: Entity Integrity (is the offer described accurately?)
    │
    ├── WU-2: Independent Path Analysis (what exactly is being continued)
    │     └── Gate: Entity Integrity (is the path described accurately?)
    │
    ├── WU-3: Value Mapping — Financial Security
    │     └── How does each option serve/threaten this value?
    │     └── Gate: Uses meaning layer definition, not generic
    │
    ├── WU-4: Value Mapping — Autonomy
    │     └── How does each option serve/threaten this value?
    │     └── Gate: Uses meaning layer definition, not generic
    │
    ├── WU-5: Value Mapping — Intellectual Growth
    │     └── How does each option serve/threaten this value?
    │     └── Gate: Uses meaning layer definition, not generic
    │
    ├── WU-6: Temporal Projection
    │     └── How does each option likely unfold at 1, 3, 5 years?
    │     └── Gate: Projections are plausible, not optimistic/pessimistic
    │
    ├── WU-7: Precedent Search
    │     └── Has the user faced similar tensions? What was chosen? What was the outcome?
    │     └── Gate: Precedents are accurately retrieved from meaning layer history
    │
    ├── WU-8: Tension Map
    │     └── Synthesize: where exactly do the options diverge on values?
    │     └── Gate: Commission Alignment (holds tension, doesn't resolve it)
    │
    └── WU-9: Assembly
          └── Gate: Equal depth + Tension held + No recommendation
```

## Key Gate Moments

### The Bias Gate (WU-8)
The Tension Map agent's first output subtly framed the independent path more favorably — using words like "freedom" and "authenticity" while describing the offer in more neutral terms. The gate caught this as a constraint violation (present both options with equal rigor). Returned with specific instances flagged. Second pass: balanced.

### The Anti-Recommendation Gate (WU-9)
Assembly's first draft ended with "given your stated values, the independent path appears more aligned..." — flagged as an implicit recommendation. The anti-goal is clear: hold the tension, don't resolve it. Returned. Second draft ends with the tension map itself, no conclusion.

## Delivery

```yaml
delivery:
  status: COMPLETE
  artifact:
    type: "decision_brief"
    sections:
      - "Offer Analysis: what you'd be getting, in concrete terms"
      - "Independent Path Analysis: what you'd be continuing, in concrete terms"
      - "Value Impact Matrix: each value × each option"
      - "Temporal Projection: how each option unfolds over time"
      - "Precedent Report: similar past decisions and their outcomes"
      - "Tension Map: where exactly the values diverge"
  validity_attestation:
    overall_validity: VALID
  unresolved_tensions:
    - description: "The meaning layer's definition of 'autonomy' may be evolving — the user's recent reflections suggest a more nuanced view than what's currently stored"
      impact: "The value mapping for autonomy may be based on a slightly outdated definition"
      recommendation: "Consider updating the autonomy definition in the meaning layer before deciding"
```

## What Happens Next

The MB-OS presents you with the decision brief. You read it. You see — clearly, without bias, through the lens of your own values — what each option means.

The Decision Interface might note: *"Last time you chose security over exploration (3 years ago, the corporate role), your reflections at 6 months showed regret. At 18 months, you described it as 'necessary but constraining.' This is context, not a recommendation."*

Then **you** decide. The factory provided the map. You choose the territory.

This is the boundary: the factory doesn't decide. The MB-OS doesn't execute. You are the meaning-maker. The system serves that role — it doesn't replace it.
