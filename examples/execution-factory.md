# Example: Execution Factory

## Scenario

Your MB-OS holds an active commitment: *"Ship the first public version of the Agentic Factory Architect framework."* The Narrative Engine notes this has been building for weeks — conversations, architectural sessions, scattered documents. The Coherence Engine flags: *"This commitment has been in 'active' state for 3 weeks with no concrete deliverable. Drift risk: medium."*

The MB-OS commissions an Execution Factory to decompose the project, manage the work, and deliver a complete repository.

## Commission

```yaml
commission:
  id: "c-execution-afa-repo-001"
  origin:
    meaning_source: "intellectual_contribution.framework_publication"
    value_alignment: ["intellectual_contribution", "clarity_of_thought", "builder_identity"]
    narrative_context: "Transition from exploration to articulation — first public artifact of the framework"
  intent:
    description: "Produce a complete, publishable GitHub repository containing the Agentic Factory Architect framework"
    success_definition: "A repository that a technical reader can navigate, understand the full stack (PA → MB-OS → Factories), and see concrete specs and examples"
    anti_goals:
      - "Do not produce code implementations — this is an architecture framework, not a codebase"
      - "Do not oversimplify for broad audiences — the target reader is technical and thoughtful"
      - "Do not create marketing material disguised as documentation"
  entities:
    - name: "Repository"
      definition: "A GitHub repository at marxwood/agentic-factory-architect"
    - name: "Framework"
      definition: "The complete PA → MB-OS → Agentic Factory architectural stack"
    - name: "SWD Canon"
      definition: "The structural discipline skills that bind the framework, at marxwood/swd-canon-skills"
  constraints:
    hard:
      - "All content must be traceable to the original architectural conversations"
      - "The repository structure must be navigable without explanation"
      - "Every spec must include at least one concrete example"
      - "Cross-references to the SWD Canon must be accurate"
    soft:
      - "Include mermaid diagrams for visual learners"
      - "Each document should be self-contained (readable without reading all others)"
  validity_criteria:
    - "Every document referenced in README.md exists"
    - "All internal links resolve"
    - "The framework is internally consistent across all documents"
    - "A reader can trace from philosophy (MANIFESTO) → architecture (docs) → specs → examples"
```

## Decomposition

```
[Commission]
    │
    ├── WU-1: Structure Design
    │     └── Define repo layout, file names, navigation flow
    │     └── Gate: Does structure serve the navigability constraint?
    │
    ├── WU-2: Philosophy Layer (parallel after WU-1)
    │     ├── WU-2a: README.md (entry point)
    │     ├── WU-2b: MANIFESTO.md (philosophical foundation)
    │     └── Gate: Commission alignment (no marketing, no oversimplification)
    │
    ├── WU-3: Documentation Layer (parallel after WU-1)
    │     ├── WU-3a: Evolution doc (PA → MB-OS → Factories)
    │     ├── WU-3b: MB-OS design doc
    │     ├── WU-3c: Factory architecture doc
    │     ├── WU-3d: Governance doc
    │     ├── WU-3e: Canon integration doc
    │     └── Gate per doc: Self-containment + Canon accuracy + Internal consistency
    │
    ├── WU-4: Specification Layer (parallel after WU-3)
    │     ├── WU-4a: Commission spec
    │     ├── WU-4b: Agent contract spec
    │     ├── WU-4c: Validation gate spec
    │     ├── WU-4d: Delivery interface spec
    │     └── Gate per spec: Includes concrete example + Schema is complete
    │
    ├── WU-5: Diagram Layer (parallel after WU-3)
    │     ├── WU-5a: Full stack diagram
    │     ├── WU-5b: Factory flow diagram
    │     ├── WU-5c: MB-OS components diagram
    │     └── Gate: Diagrams match the architecture described in docs
    │
    ├── WU-6: Example Layer (parallel after WU-4)
    │     ├── WU-6a: Research factory example
    │     ├── WU-6b: Creation factory example
    │     ├── WU-6c: Decision factory example
    │     ├── WU-6d: Execution factory example (this one — meta-recursive)
    │     └── Gate: Examples use actual spec formats + Commission is realistic
    │
    └── WU-7: Assembly + Validation
          ├── Cross-reference check (all internal links resolve)
          ├── Consistency check (no contradictions across documents)
          ├── Completeness check (README references match actual files)
          └── Gate: Full validity suite
```

## Execution Characteristics

### Parallelism
WU-2, WU-3, and WU-5 can run in parallel once WU-1 (structure) is complete. WU-4 depends on WU-3 (specs reference architecture docs). WU-6 depends on WU-4 (examples use spec formats).

### Self-Reference
This example is itself a product of the execution factory it describes. This is intentional — it demonstrates that the framework can describe its own production process. The meta-recursive nature is a feature, not a bug: if the framework can't account for how it was built, it's incomplete.

### Agent Specialization
Different work units require different capabilities:
- WU-2 needs a philosophical writer
- WU-3 needs a technical architect
- WU-4 needs a specification designer
- WU-5 needs a visual/structural thinker
- WU-6 needs someone who can hold the whole system and produce realistic scenarios
- WU-7 needs a meticulous validator

In a real factory, these would be different agents (or the same model with different system prompts and constraints). The key: each has a bounded role and explicit output contract.

## Key Gate Moments

### The Marketing Gate (WU-2)
README.md's first draft opened with enthusiasm about "revolutionary" and "paradigm-shifting" technology. Gate caught this as a constraint violation (no marketing material). Returned with note: "The framework should speak for itself. Replace promotional language with precise description."

### The Self-Containment Gate (WU-3)
WU-3c (factory architecture) assumed the reader had read WU-3b (MB-OS design). Gate returned it: "This document references 'the meaning layer' without defining it. Each doc should be readable independently. Add a brief definition or cross-reference."

### The Consistency Gate (WU-7)
Assembly found that WU-3c described validation gates with two outcomes (Pass/Return) but WU-4c's spec defined three outcomes (Pass/Return/Escalate). Inconsistency flagged. WU-3c was updated to match the spec.

## Delivery

```yaml
delivery:
  status: COMPLETE
  artifact:
    type: "github_repository"
    location: "marxwood/agentic-factory-architect"
    files: 19
    total_content: "~1800 lines across 19 files"
  validity_attestation:
    all_criteria_met: true
    overall_validity: VALID
    criteria_results:
      - criterion: "Every document referenced in README exists"
        result: PASS
      - criterion: "All internal links resolve"
        result: PASS
      - criterion: "Framework is internally consistent"
        result: PASS
      - criterion: "Reader can trace philosophy → architecture → specs → examples"
        result: PASS
  unresolved_tensions:
    - description: "The framework describes factories but doesn't implement them — there's a gap between architecture and running code"
      impact: "Readers may ask 'where's the code?'"
      recommendation: "Consider a future commission for a reference implementation"
    - description: "The execution factory example is self-referential — it describes its own production. This is philosophically interesting but may confuse readers who expect a 'normal' example"
      recommendation: "Add a note acknowledging the meta-recursive nature"
  provenance:
    agents_involved: 7
    gate_evaluations: 15
    returns: 3
    escalations: 0
    total_duration: "~45 minutes"
```

## The Meta-Lesson

This execution factory demonstrates the deepest principle of the framework: **the system can account for its own creation**.

The repository you're reading was produced by a process that the repository itself describes. The commission format used to initiate this work is specified in `specs/commission-spec.md`. The validation gates that checked this work are defined in `specs/validation-gate-spec.md`. The agent contracts that bounded each contributor's role are formalized in `specs/agent-contract-spec.md`.

This isn't cleverness — it's a validity test. If the Agentic Factory Architect framework can't describe how an agentic factory built the Agentic Factory Architect framework, then the framework has a gap.

It can. It doesn't.
