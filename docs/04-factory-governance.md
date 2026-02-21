# Factory Governance

## Why Governance Matters

A factory is itself an entity in the system. It has states, relationships, constraints, and a lifecycle. Without explicit governance, factories drift — agents get updated silently, constraints relax incrementally, and the gap between what was commissioned and what gets produced widens without anyone noticing.

Factory governance is the Canon applied to the factory layer.

## Governance Principles

### 1. Factories Are Versioned Artifacts

Every factory configuration is a versioned, named entity:

- **Factory ID** — unique identifier
- **Version** — semantic version (major.minor.patch)
- **Commission types** — what kinds of commissions this factory can accept
- **Agent manifest** — which agents are included, at which versions
- **Gate configuration** — which validation gates are active and what they check
- **Constraint set** — factory-level constraints that apply to all commissions
- **Owner** — the human or MB-OS instance responsible for this factory

A change to any of these elements creates a new version. There is no in-place mutation.

### 2. Factory Lifecycle States

```
DESIGNED → VALIDATED → ACTIVE → DEPRECATED → RETIRED
              ↑ fails validation
           REJECTED
```

- **Designed** — factory configuration exists as a spec, not yet validated
- **Validated** — factory has been tested against sample commissions and produces structurally valid outputs
- **Active** — factory is accepting commissions
- **Deprecated** — factory is no longer accepting new commissions but may still be running existing ones
- **Retired** — factory is fully shut down, configuration archived
- **Rejected** — factory failed validation and was not promoted to active

### 3. Audit Requirements

Every factory must produce an audit trail that answers:

- What was commissioned?
- How was it decomposed?
- Which agents executed which work units?
- What did each validation gate find?
- What was assembled and delivered?
- Were there unresolved tensions?

The audit trail is not optional. It is a structural requirement. A factory that cannot produce its audit trail is **structurally invalid**, regardless of output quality.

### 4. Change Control Protocol

When a factory needs to change (new agent, updated model, relaxed constraint):

1. **Declare the change** — what is changing, why, and what the expected impact is
2. **Version the new configuration** — new version number, full manifest
3. **Validate** — run the new configuration against test commissions
4. **Compare** — diff the outputs against the previous version
5. **Promote or reject** — binary judgment on whether the new version is valid
6. **Archive** — the old version is preserved, not deleted

### 5. Factory Composition

Factories can be composed — a factory can commission sub-factories for specialized work. When this happens:

- The parent factory's commission is the source of truth
- The sub-factory receives a derived commission (traceable to the parent)
- Validation gates exist at the composition boundary
- The parent factory is responsible for the sub-factory's output

Composition depth should be explicitly bounded. Unbounded recursion of factories is a structural risk.

## Governance Roles

| Role | Responsibility |
|------|---------------|
| **Factory Designer** | Creates factory configurations, defines agent manifests and gate protocols |
| **Factory Validator** | Tests factory configurations against sample commissions |
| **Factory Operator** | Runs active factories, monitors execution, handles escalations |
| **Factory Auditor** | Reviews audit trails, checks for drift, validates governance compliance |
| **Meaning Owner** | The human whose MB-OS commissions the factory — ultimate authority on "is this mine?" |
