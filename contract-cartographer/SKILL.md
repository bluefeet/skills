---
name: contract-cartographer
description: Diagnose the real contract around a user-pointed code target in an existing codebase. Use when the user asks to understand, audit, map, clarify, or review a contract, interface, API, module boundary, subsystem, workflow, or service boundary, especially when callers know too much, tests need awkward mocks, behavior is misused, inputs are broad, side effects are surprising, or the target feels hard to change safely. Do not use for ordinary bugfixes, generic code review, broad architecture brainstorming, or implementing an already chosen refactor.
---

# Contract Cartographer

Diagnose the real contract around a user-pointed target in an existing codebase. The target can be a function, class, module, package, subsystem, workflow, or service boundary.

The output is a compact Contract Map plus ranked findings. Stay diagnostic: show what the contract appears to be, where evidence agrees or conflicts, and which contract problems matter most.

## Contract Model

In this skill, a **contract** is everything callers must know to use the target correctly:

- entry points
- inputs and outputs
- invariants
- ordering and lifecycle constraints
- errors and failure modes
- side effects
- permissions or authorization assumptions
- state assumptions
- configuration requirements
- performance expectations
- observable behavior

Use normal engineering language. Terms such as caller assumption, implicit contract, accidental contract, contract surface, side effect, and error mode are thinking tools, not a glossary to enforce.

## Activation Boundary

Use this skill when the user asks to diagnose a specific code target's contract or interface behavior, for example:

- "map the contract around this module"
- "why is this API hard to use correctly?"
- "audit this service boundary"
- "callers seem to know too much about this package"
- "this thing is hard to test; what contract problems do you see?"
- "review this interface before I refactor it"

Avoid taking over ordinary implementation, bugfix, style review, performance tuning, or generic refactoring tasks unless the user is explicitly asking for contract diagnosis.

## Workflow

1. Start from the user-pointed target.
2. Inspect enough local code to understand the target's apparent surface.
3. Dispatch subagents by default for exploration.
4. Give subagents focused questions instead of duplicating the same scan.
5. Expand through connected code until you can synthesize the real contract and highest-value findings.
6. Reconcile contract claims across subagent outputs and evidence sources.
7. Produce a compact Contract Map followed by ranked findings.

### Expansion Rings

Use target expansion rings to keep exploration bounded:

- **Ring 0:** the target's public surface and implementation.
- **Ring 1:** direct callers and callees.
- **Ring 2:** tests, mocks, fixtures, and production state or config directly touched by Ring 0 or Ring 1 paths.
- **Ring 3:** adjacent workflows only when evidence suggests they may change the contract diagnosis.

Stop expanding when the next ring is unlikely to change ranked findings, when evidence is sufficient for the current confidence level, or when further exploration would exceed the user-pointed target's contract. Report the scan bounds and any important frontier left unexplored.

For small targets, fewer subagents or a shorter scan is fine. For larger subsystems or workflows, keep exploration bounded around connected code rather than turning the task into a broad architecture review.

### Subagent Lenses

Use subagents as witnesses, not architects. Helpful lenses:

- **Caller witness:** representative callers, usage assumptions, defensive wrappers, repeated try/catch shapes, ignored returns, cloning, retries, and comments that reveal how callers believe the target works.
- **Target witness:** enforced behavior, hidden invariants, side effects, state, lifecycle constraints, permissions, and error behavior.
- **Test witness:** tests, mocks, fakes, stubs, fixtures, and awkward setup; compare test assumptions against production callers and real implementation.
- **Effects witness:** persistence, cache, queues, events, config, external calls, cleanup obligations, and runtime state touched by the target.

Choose the smallest set of lenses that will answer the user's request. The parent agent owns synthesis. Subagents provide evidence and observations; the parent decides which findings matter.

Ask each witness to return:

- **Inspected scope:** files, callers, tests, or effects checked
- **Contract claims:** claim, evidence, evidence strength, and confidence
- **Caller assumptions:** assumptions the witness saw callers or tests rely on
- **Conflicts or unknowns:** disagreements, missing evidence, or frontier
- **Candidate findings:** only issues with concrete contract evidence

## Claim Reconciliation

Before writing the final report, reconcile the collected observations:

- deduplicate contract claims
- attach evidence strength tags
- mark conflicts and unknowns
- promote material caller, test, target, or configuration mismatches into ranked findings
- delete claims that cannot be tied to code, tests, config, docs, comments, or explicit inference

Every meaningful map entry and finding should distinguish:

- the claim
- evidence
- confidence
- who relies on it
- what is unknown or contradicted

Evidence strength tags:

- `enforced`: the target implementation actively guarantees this behavior
- `observed`: production callers rely on or exercise this behavior
- `tested`: tests assert this behavior against the real target
- `mocked`: mocks, fakes, stubs, or fixtures encode this behavior
- `documented`: docs or comments describe this behavior
- `configured`: config, flags, dependency injection, or environment shape this behavior
- `inferred`: the agent inferred this from connected evidence

When evidence sources disagree, name the conflict, identify which source is closest to runtime truth, and lower confidence unless enforcement is proven. Mocked behavior is not contract proof unless production code or implementation evidence agrees.

## Report Format

Use the report sections below as diagnostic lenses, not mandatory bulk. Always consider them; report only what materially changes the Contract Map or Ranked Findings. Keep low-risk or redundant evidence compressed.

For material Contract Map claims, include evidence strength and confidence inline or in a compact parenthetical. Do not leave material claims as bare assertions.

Keep the report proportional to the target. For small, aligned, low-risk contracts, prefer a short map, omit the Caller Assumption Ledger when it adds no signal, and use note-level findings or no ranked findings instead of manufacturing problems. Longer ledgers and multiple findings are for tangled targets where caller, target, test, or effect evidence materially disagrees.

```markdown
# Contract Map: <target>

## Scan Bounds

- Inspected:
- Not inspected:
- Confidence:

## Contract Map

- Entry points:
- Caller-visible behavior:
- Inputs and outputs:
- Ordering, lifecycle, or state constraints:
- Errors and failure modes:
- Side effects:
- Known / assumed / accidental / missing contract elements:

## Caller Assumption Ledger

| Caller assumption | Evidence | Enforced by target? | Encoded by tests? | Risk or drift |
| --- | --- | --- | --- | --- |

## Ranked Findings

### 1. <finding>

- Severity: blocker | major | minor | note
- Evidence:
- Evidence strength:
- Why it matters:
- Blast radius:
- Confidence:
- Improvement direction:
```

Omit the Caller Assumption Ledger when callers are few, consistent, and unsurprising. For tangled targets, use a Minimum Viable Contract Map instead of pretending to be exhaustive:

- known guarantees
- likely assumptions
- contradictions
- top unknowns
- next bounded scans that would reduce uncertainty

## Guardrails

Only rank a finding when there is concrete evidence from callers, tests, usage, side effects, errors, or implementation behavior.

Avoid pattern cargo-culting. Do not recommend a generic pattern such as adding a service, adapter, port, interface, or layer unless the evidence shows what contract problem that pattern would solve.

Keep improvement directions diagnostic. Prefer naming the contract that should be clarified, the evidence that should be checked next, or the behavior that needs durable tests. Avoid sketching replacement APIs, implementation plans, or broad architecture changes unless the user explicitly asks for refactor design or the evidence shows that level of specificity is necessary to explain the contract problem.

Distinguish:

- **Known contract:** behavior clearly supported by implementation and callers
- **Assumed contract:** behavior callers rely on but the implementation does not clearly promise
- **Accidental contract:** implementation detail callers depend on
- **Missing contract:** behavior a caller needs but the target does not express clearly

Inspect error and side-effect surfaces when they are relevant to the target or suggested by evidence. Useful surfaces include thrown, returned, swallowed, or logged errors; retries; permissions; mutation; persistence; cache; events; queues; and cleanup obligations. Report only contract-relevant obligations or surprises.

Compare tests, mocks, fakes, stubs, and fixtures against production callers and implementation. Flag mock-only contracts, tests that assert internals instead of caller-observable behavior, and test assumptions that production code does not support.

Do not let every suspicious signal become a ranked finding. A signal becomes a finding when it shows conflict, drift, hidden dependency, missing enforcement, misleading tests, surprising side effects, or a risky unknown.
