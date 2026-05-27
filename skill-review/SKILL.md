---
name: skill-review
description: Review an existing skill directory, SKILL.md file, or pasted skill text for skill-authoring quality. Use when the user asks to review, audit, critique, evaluate, improve, harden, or quality-check a skill's metadata, trigger behavior, progressive disclosure, instruction design, examples, bundled assets, dependencies, validation loops, or evaluation quality.
---

# Skill Review

Use this skill to produce a structured best-practices review of one skill directory, one `SKILL.md` file, or pasted skill text. The review is a hybrid audit and coaching report: objective issues get severity and evidence, while judgment-based improvements get tradeoffs and concrete revision advice.

Do not run a full eval loop as part of this skill. Identify behavioral checks that would need observed usage, eval outputs, or transcripts, and distinguish those from findings proven by static review.

## Reference

Read `references/criteria.md` before producing a review. Use it as the source of categories, severity values, applicability values, evidence types, scorecard statuses, and criterion guidance.

## Review Workflow

1. **Inventory**
   - Read the target `SKILL.md`, frontmatter, directory layout, bundled references, scripts, assets, and eval files when present.
   - When passed a `SKILL.md` path, inspect its containing directory when available.
   - When only standalone skill text is available, mark directory-level checks as `not applicable` or note the missing context as contextual evidence instead of failing them.
   - Identify which criteria are universal, conditional, or scaling for this skill.

2. **Static Review**
   - Check metadata validity, discovery and activation, scope boundaries, body length, reference structure, terminology, path style, dependency notes, scripts, output templates, examples, and eval quality.
   - For evals, check whether the suite is representative, high-signal, non-redundant, cost-aware, and uses fixture outputs when testing reasoning over tool results.
   - Base static findings on concrete evidence from files or pasted text.

3. **Behavioral Readiness Review**
   - Identify what cannot be proven statically: trigger reliability, whether workflows are followed, whether examples improve output quality, whether evals are discriminating or flaky, and whether bundled files are discovered correctly.
   - Put these under Behavioral Checks Needed or mark relevant scorecard rows as `needs behavioral evidence`.
   - Use `needs behavioral evidence` only for gaps that require observed usage, transcripts, eval outputs, trigger behavior, or repeated runs. If files, frontmatter, directory contents, or deployment context are unavailable, describe that as missing static or contextual evidence instead of behavioral uncertainty; use `not applicable` when the category truly does not apply.

4. **Synthesis**
   - Lead with the highest-impact findings.
   - Do not manufacture findings to fill every severity level.
   - Include strengths, category status, behavioral checks, and an improvement order.

## Report Format

Use this structure:

```text
# Skill Review: [skill name]

## Overall Assessment
Status: [ready | needs targeted revision | not ready | needs behavioral evidence]

[Brief qualitative assessment and main risk or opportunity.]

## Strengths
- [Specific strength with evidence or rationale.]

## Findings
### Blocker
- [Category] [Finding]
  Evidence: [file, section, quote, or paraphrase]
  Why it matters: [impact]
  Recommendation: [specific fix]

### Major
- [Category] [Finding]
  Evidence: [file, section, quote, or paraphrase]
  Why it matters: [impact]
  Recommendation: [specific fix]

### Minor
- [Category] [Finding]
  Evidence: [file, section, quote, or paraphrase]
  Why it matters: [impact]
  Recommendation: [specific fix]

### Advisory
- [Category] [Finding]
  Evidence: [file, section, quote, or paraphrase]
  Why it matters: [impact]
  Recommendation: [specific fix]

## Category Scorecard
| Category | Status | Notes |
| --- | --- | --- |

## Behavioral Checks Needed
- [What should be tested by running the skill or reviewing transcripts/evals.]

## Suggested Improvement Order
1. [Highest leverage fix.]
2. [Next fix.]
3. [Optional polish.]
```

Example finding:

```text
### Major
- [Evaluation Quality And Efficiency] Eval status expectations are too loose.
  Evidence: static; `evals/evals.json` accepts either `ready` or `needs behavioral evidence` for the same static-good scenario.
  Why it matters: Reviewers can disagree on readiness without the eval catching the drift.
  Recommendation: Split the cases so routine follow-up checks expect `ready`, while material unproven behavior expects `needs behavioral evidence`.
```

Omit empty finding severity sections. Sort findings within each severity by likely impact. Include every category in the scorecard, using `not applicable` or `needs behavioral evidence` where appropriate.

## Status Rules

Choose the overall status by precedence:

1. `not ready`: blockers, invalid loading or metadata, unsafe guidance, or structural problems make the skill unreliable.
2. `needs targeted revision`: unresolved major static or contextual issues remain, and no blocker is present.
3. `needs behavioral evidence`: static and contextual review are acceptable, but material confidence depends on observed usage, eval output, or transcripts.
4. `ready`: no blocker or major issues remain, and any behavioral checks are routine or non-blocking.

Behavioral Checks Needed can appear under any overall status. Listing a behavioral check does not automatically make the overall status `needs behavioral evidence`; the missing evidence must materially affect reliance on the skill.

Treat missing context as major only when it materially affects selection, correctness, repeatability, or reliance.

Do not use numeric scores unless the user explicitly asks for them.

## Final Self-Check

Before finalizing, verify:

- Findings are supported by static, behavioral, or contextual evidence as labeled.
- Behavioral uncertainty is not presented as proven fact.
- The review uses source-neutral best-practices language.
- The report does not include provenance notes or hidden-source disclaimers.
- The report does not encode repository-specific workflow rules as skill-quality criteria.
- The highest-impact findings appear before polish.
- The improvement order is concrete enough for another agent to act on.
