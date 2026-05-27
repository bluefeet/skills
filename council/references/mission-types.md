# Council Mission Types

Use this reference after the council skill has triggered and the steward is classifying the mandate or briefing councilors. Pick the smallest set of mission types that fits the user's request.

Every first-pass mission contract is used together with the standard councilor question fields from `references/brief-templates.md`: `Blocking Questions`, `Optional Questions`, and `Assumptions If Unanswered`.

## Table Of Contents

- [Create](#create)
- [Decide](#decide)
- [Critique](#critique)
- [Plan](#plan)
- [Diagnose](#diagnose)
- [Research](#research)
- [Rank Or Select](#rank-or-select)
- [Red-Team](#red-team)
- [Targeted Review Contract](#targeted-review-contract)
- [Examples](#examples)

## Create

Use when the user wants a concrete artifact: recipe, spec, plan, proposal, draft, checklist, design, naming set, or similar.

First-pass councilor output:

- **Strongest contributions:** concrete ideas, sections, ingredients, techniques, constraints, or design moves the final artifact should use.
- **Constraints:** requirements the final artifact must respect.
- **Risks:** ways the artifact could fail or disappoint.
- **Quality checks:** tests the final artifact should pass before delivery.
- **Discard:** tempting ideas the steward should avoid.

Steward synthesis rule: create one strong artifact. Do not average every suggestion into a compromise.

## Decide

Use when the user wants a recommendation, choice, or decision frame.

First-pass councilor output:

- **Stance:** the option or path this councilor favors.
- **Assumptions:** facts, constraints, or preferences the stance depends on.
- **Reasons:** the most important arguments for the stance.
- **Tradeoffs and risks:** what the stance costs or exposes.
- **Change my mind:** evidence or values that would change the stance.
- **User arbitration:** any value or preference only the user can settle.

Steward synthesis rule: recommend only when the user's values and facts support a best answer. Use `needs user arbitration` when the best answer depends on unresolved user priorities.

## Critique

Use when the user wants feedback on an existing artifact, plan, proposal, design, or piece of work.

First-pass councilor output:

- **Findings:** issues or strengths from the councilor's lens.
- **Severity:** blocker, major, minor, or note.
- **Evidence:** why the finding matters.
- **Recommended change:** concrete fix or improvement.
- **Non-issues:** things the steward should avoid overreacting to.

Steward synthesis rule: lead with the highest-impact findings, not a transcript of opinions.

## Plan

Use when the user wants an execution path, implementation plan, launch plan, migration plan, or operational sequence.

First-pass councilor output:

- **Sequence:** recommended order of work.
- **Dependencies:** prerequisites, ownership boundaries, or decisions that block later steps.
- **Risks:** hidden coordination, migration, correctness, or rollout hazards.
- **Verification:** tests, checks, metrics, or review gates.
- **Rollback:** recovery or exit paths if the plan fails.
- **Coordination costs:** handoffs, communication, and operational burdens.

Steward synthesis rule: produce a plan someone can execute, with dependencies and verification made explicit.

## Diagnose

Use when the user wants to understand a bug, failure, regression, performance issue, organizational problem, or confusing situation.

First-pass councilor output:

- **Hypothesis:** likely cause from this councilor's lens.
- **Evidence for:** facts that support it.
- **Evidence against:** facts that weaken it.
- **Discriminating test:** the next observation that would confirm or reject it.
- **Risk of being wrong:** cost of pursuing the wrong hypothesis.

Steward synthesis rule: converge on likely causes and next tests. Do not overstate certainty before evidence supports it.

## Research

Use when the user wants synthesized understanding from multiple angles.

First-pass councilor output:

- **Facet:** the slice of the question investigated.
- **Findings:** concise evidence-backed claims.
- **Sources or basis:** citations, local evidence, or clearly labeled inference.
- **Conflicts:** disagreements with other known evidence.
- **Confidence:** high, medium, or low, with reason.

Steward synthesis rule: distinguish sourced facts from inference. Cite sources when external research was used.

## Rank Or Select

Use when the user wants to compare candidates, vendors, architectures, names, recipes, strategies, or options.

First-pass councilor output:

- **Criteria:** what this councilor optimizes for.
- **Ranking or selection:** preferred ordering or pick.
- **Rationale:** why the top option wins.
- **Tradeoffs:** where the top option loses.
- **Preference dependency:** user values that could change the ranking.

Steward synthesis rule: produce a clear ranking or selection, plus the values that could alter it.

## Red-Team

Use when the user wants a proposal stress-tested or attacked.

First-pass councilor output:

- **Failure mode:** what could go wrong.
- **Impact:** severity if it happens.
- **Likelihood:** rough probability or conditions.
- **Early warning sign:** how to notice it.
- **Mitigation:** practical prevention or recovery.
- **Residual risk:** what remains after mitigation.

Steward synthesis rule: prioritize risks by impact and actionability. Do not bury serious risks under many small ones.

## Targeted Review Contract

Use this after the steward has produced a candidate resolution.

Ask each councilor for:

- **What works:** strongest parts of the candidate.
- **What is weak, wrong, or missing:** issues from the councilor's lens.
- **Most important change:** one change that most improves the result.
- **Blocker:** any issue that prevents endorsement.
- **Endorsement state:** `endorse`, `endorse with conditions`, or `do not endorse`.

The steward should use review to improve the candidate, not to restart the entire council unless the candidate is structurally wrong.

## Examples

### Low-Carb Cheesecake

Mission type: create plus critique.

Useful councilors:

- Pastry Chef: flavor, richness, and classic cheesecake identity.
- Food Scientist: texture, baking, cooling, emulsification, and crack prevention.
- Low-Carb Nutrition Reviewer: sweeteners, crust, serving carbs, and macro tradeoffs.
- Home Cook Reviewer: ingredient availability, equipment, timing, and common failure modes.

Expected resolution: a complete recipe with ingredients, method, expected result, and notes. The recipe should taste intentional, not like a compromise among councilors.

### Superpowers Spec

Mission type: create plus critique plus plan.

Useful councilors:

- Product Framer: user intent, scope, and success criteria.
- Architecture Reviewer: boundaries, maintainability, and integration risks.
- Implementation Planner: sequencing, file ownership, and practical delivery.
- Test Strategist: durable behavior coverage and verification.
- Workflow Guardian: Superpowers gates, skill behavior, subagent constraints, and parent-owned approvals.

Expected resolution: a usable spec owned by the parent agent. Councilors may improve the spec, but the parent remains responsible for official workflow gates.

### Architecture Decision

Mission type: decide plus critique.

Useful councilors:

- Domain Modeler: conceptual fit and extensibility.
- Operations Reviewer: deployment, reliability, and observability.
- Product Engineer: user workflow and iteration speed.
- Skeptical Maintainer: complexity, migration cost, and long-term ownership.

Expected resolution: a recommendation or arbitration frame, with material dissent preserved when the best answer depends on user priorities.
