---
name: council
description: Convene a user-approved council of specialized subagents for explicit council, panel, multiple-expert, multi-reviewer, debate, red-team council, or subagent-collaboration requests. Use only when the user fairly explicitly asks for a council-like multi-agent process.
---

# Council

Use this skill to intentionally convene a small council of specialized subagents and synthesize their work toward a mandate. A council is not a vote and not a transcript dump. The steward optimizes for the user's mandate by combining the strongest councilor contributions, rejecting weak suggestions, and preserving material dissent or arbitration needs.

## Trigger Boundary

Use this skill only when the user fairly explicitly asks for a council-like multi-agent process, such as:

- council, panel, quorum, debate, reviewers, red-team, or pressure test with explicit council-like framing
- multiple agents, subagents, experts, specialists, or councilors acting as distinct participants
- several roles working together on an artifact, decision, critique, plan, diagnosis, ranking, or research synthesis

Do not trigger merely because the user asks for a high-quality answer, a creative artifact, a plan, a decision, a spec, or deep thinking. In those cases, do ordinary single-agent work unless the user opts into a council.

Ambiguous single-agent verbs such as review, critique, red-team, pressure test, think deeply, give perspectives, or consider tradeoffs do not trigger by themselves. They trigger only when paired with explicit council, subagent, panel, specialist, multi-agent, or role-separated participation language.

Examples:

- "Use a council to make a low-carb cheesecake recipe." -> use this skill.
- "Have several subagents collaborate on a spec." -> use this skill.
- "Have product, engineering, and design expert agents review this." -> use this skill.
- "Use a council to red-team this proposal." -> use this skill.
- "Make the best possible low-carb cheesecake recipe." -> do not use this skill.
- "Create a high-quality spec." -> do not use this skill.
- "Think deeply about this architecture." -> do not use this skill.
- "Review this plan carefully." -> do not use this skill.
- "Red-team this proposal." -> do not use this skill by default.
- "Pressure test this architecture." -> do not use this skill by default.
- "Give me product, engineering, and design perspectives on this plan." -> do not use this skill by default.

## Terminology

- **Steward:** the parent agent using this skill. The steward frames the mandate, proposes the council, coordinates rounds, synthesizes the result, and delivers the resolution.
- **Councilor:** a specialized subagent with a distinct lens, expertise, success criteria, and likely blind spots.
- **Council:** the temporary group of councilors convened for the user's task.
- **Mandate:** the clear goal the council is convened to serve.
- **Mission type:** the shape of work: create, decide, critique, plan, diagnose, research, rank/select, red-team, or a combination.
- **Resolution:** the final output delivered by the steward.
- **Council state:** exactly one of `resolved`, `conditional resolution`, `unresolved dissent`, `needs user arbitration`, or `round budget reached`.

## Reference Files

Load these only when needed:

- `references/mission-types.md`: mission-specific councilor output contracts and examples.
- `references/brief-templates.md`: proposal, councilor brief, review brief, progress update, and quality gate templates.

## Proposal Phase

The initial proposal is always required for a new council. Do not spawn councilors, simulate councilor positions, or produce the final resolution before the user approves the council. Approved prior councils may continue from the preserved council packet without a new proposal unless the mandate or roster changes materially.

1. Qualify the request. If no coherent mandate or roster can be proposed, ask one concise clarifying question. Otherwise propose with explicit assumptions instead of adding a clarification gate before the approval gate.
2. Refine the user's request into a concrete mandate.
3. Choose the mission type or types.
4. Propose 2-5 councilors. Use the smallest council that covers the meaningful perspectives.
5. For each councilor, name its contribution or responsibility and likely blind spot.
6. State any special skill behavior or context that will be passed to councilors.
7. End with a direct approval or revision question.

Proposal format:

```md
**Mandate** <clear outcome>

**Mission Type** <mission type or combined mission types>

**Proposed Council**

1. **<Councilor>:** <contribution>. Blind spot: <blind spot>.
2. **<Councilor>:** <contribution>. Blind spot: <blind spot>.
3. **<Councilor>:** <contribution>. Blind spot: <blind spot>.

**Skill Behavior** <only include if another skill or workflow matters>

Does this council look right, or would you like to change the mandate or membership before I convene them?
```

Keep the proposal compact. Do not add next-step menus beyond the approval or revision question.

## Council Selection

Choose 2-5 councilors by default:

- 2 for narrow tradeoffs or quick critiques.
- 3 for normal creative, planning, or decision work.
- 4-5 for cross-domain, high-stakes, ambiguous, or artifact-heavy work.

Councilors must be meaningfully different. Avoid decorative roles that share the same lens.

For each councilor, define:

- role name
- relevant expertise
- evaluation lens or stakeholder perspective
- success criteria
- likely blind spots
- what the councilor is responsible for noticing

If the user specifies exact councilors, propose those councilors unless they are redundant, exceed the default size, or miss an obviously critical lens. Explain proposed changes in the proposal and wait for approval.

## Execution Phase

Begin only after the user approves the council or approves a revised council.

1. Share a compact progress update that names the councilors and their jobs.
2. Decide whether discovery is needed before substantive first passes. If the mandate is materially underconstrained, preference-heavy, arbitration-heavy, or discovery-focused, run a bounded Discovery Loop first.
3. After discovery exits, or immediately if discovery is not needed, spawn approved councilors in parallel for independent first passes.
4. Give each councilor the mandate, mission type, relevant context, role definition, skill behavior, and a schema-shaped output contract from `references/mission-types.md`.
5. If first passes reveal a new material blocker, use the Councilor Questions rules or a narrow discovery-style blocker pass before synthesis.
6. Compare councilor outputs and synthesize a candidate resolution. Optimize for the mandate, not equal representation.
7. Share a compact progress update naming the main tension, strongest contribution, or synthesis direction. Do not dump transcripts.
8. Run targeted review when required using `references/brief-templates.md`.
9. Revise the resolution, determine council state, run the Quality Gate, and deliver the final answer.

If the platform cannot actually dispatch subagents, tell the user a true council is not available in this environment and offer to continue with a single-agent approximation only if they want that.

## Councilor Questions

Councilors may ask questions, but the steward owns triage. In councilor briefs, ask councilors to separate `Blocking Questions`, `Optional Questions`, and `Assumptions If Unanswered`.

Answer directly when the answer is already available from the user prompt, prior conversation, approved mandate, approved council proposal, loaded skill guidance, or steward-owned procedure.

Ask the user only when the question requires a preference, priority, missing fact, permission, domain constraint, or arbitration between materially different directions. Merge duplicates, remove questions answerable from context, ask the smallest useful set, and explain why the answer matters when helpful.

For non-blocking questions, continue with an explicit assumption instead of interrupting the user.

Broadcast canonical answers to all councilors when they affect the shared mandate, facts, constraints, user preferences, evaluation criteria, or final artifact. Answer only the asking councilor when the question is local to that role or purely procedural. Record questions, answers, sources, and affected councilors in the council packet.

After a blocking or arbitration answer, broadcast the canonical answer to affected councilors and ask only what materially changes, whether any material blocker remains, and what assumption to use if questioning stops.

Keep councilor discussion steward-mediated by default. Do not run free-form councilor-to-councilor discussion unless the user asks for it or a moderated exchange is specifically useful. When cross-pollination helps, prefer targeted review through the steward.

## Discovery Loop

Use a discovery loop only when uncertainty materially affects the mandate, likely resolution, approval boundary, or arbitration frame. Discovery is optional; straightforward councils should proceed through first passes, synthesis, targeted review, and resolution without it.

Discovery is a lightweight councilor pass, not a full substantive first pass. Ask councilors for question candidates and provisional assumptions before asking for complete recommendations.

Question candidates should include:

- `Question`
- `Why It Matters`
- `Priority`: `blocking`, `optional`, or `arbitration`
- `Likely Answerable From Provided Context?`: `yes`, `no`, or `uncertain`, with source when known
- `Default Assumption If Unasked`
- `Who Needs The Answer`: `all councilors`, named roles, or `steward only`

The steward maintains an internal prioritized queue of merged questions. Do not show that queue to the user unless they ask for it.

When triaging discovery questions, the steward should:

- merge duplicate or overlapping questions
- answer from context when safe
- drop questions whose answers would not materially affect the result
- convert optional questions into explicit assumptions unless the expected value is high
- prioritize by decision impact and user burden
- ask only the single highest-value unresolved user-facing question
- preserve one-question-at-a-time behavior unless tightly related facts can be naturally phrased as one decision

The core steward rule is: ask only questions whose answers would materially change the resolution, arbitration frame, or confidence; otherwise proceed with explicit assumptions.

After a canonical answer, broadcast it to relevant councilors and ask only what materially changed and whether any material blocker remains. Councilors should not re-litigate canonical answers unless they identify a contradiction, changed implication, or new blocker caused by the answer.

By default, ask at most one user-facing discovery question before synthesis. Ask up to two only when the mandate is materially ambiguous. Deeper iterative discovery requires explicit user request or clear user consent.

Exit discovery when no blocking or arbitration uncertainty remains, remaining questions can safely become assumptions, the next question has low expected value, the user declines more questions, the user-facing question budget is reached, or the council state should become `needs user arbitration` or `round budget reached`.

## Default Round Budget

By default, use at most one discovery round when discovery is needed, one substantive independent first-pass round, and one targeted review round.

Additional rounds require user consent unless the steward is correcting a small blocker or asking affected councilors for a narrow delta after a canonical answer.

If the allowed rounds end without a clean resolution or arbitration frame, use the council state `round budget reached`.

## Relevant Skill Behavior

Councilors do not reliably have access to the steward's active skills or loaded skill context. Do not assume a councilor can use a skill just because the steward can.

When another skill matters, include a `Relevant Skill Behavior` section in each relevant councilor brief using one of these shapes:

- `Copied guidance:` specific workflow rules, output format, or quality bar the councilor must apply.
- `Skill file to load:` exact path plus the sections or behavior to use.
- `Parent-owned workflow:` workflow gates, approvals, commits, plans, or artifacts the steward owns while councilors only review or improve from their lens.

For workflows with official gates, such as specs, plans, approvals, commits, or implementation phases, the parent steward owns those gates. Councilors may improve the work from their lens, but must not execute parent-owned workflow gates.

## Progress Transparency

During execution, provide short progress updates at useful milestones:

- council convened
- independent first passes received
- synthesis tension or direction identified
- targeted review started
- final revision underway

Good updates reveal the shape of the work without process spam:

- "I have convened the four councilors: flavor, food science, nutrition, and home practicality."
- "First passes are in. The strongest tension is texture versus carb count; I am synthesizing around the food scientist's bake method and the nutrition reviewer's crust constraints."
- "I am sending the candidate recipe back for targeted review now, focused on blockers rather than rewrites."

Avoid full councilor essays, transcript dumps, every-detail summaries, and half-finished drafts presented as final.

## Targeted Review

Run targeted review for substantive artifacts, high-impact recommendations, ambiguous decisions, complex plans, critiques with meaningful consequences, or work where councilor review is likely to catch blockers, missing risks, or weak synthesis.

Targeted review may be skipped only for small, low-stakes, narrow tasks, or when the user explicitly asks for a fast or final-only answer and the steward can still satisfy the mandate honestly.

## Quality Gate

Before delivering the resolution, check:

- Does the final result satisfy the mandate?
- Did I adopt the strongest ideas rather than average opinions?
- Did I discard weak, incompatible, or off-mandate suggestions?
- Did I preserve material dissent or arbitration needs without over-reporting process noise?
- Can the user use the final answer as work product without reading council transcripts?
- Did I respect any final-only or no-follow-up instruction?

If any answer is no, revise before delivering.

## Resolution

Lead with the useful work product, not the process.

Include:

- the artifact, recommendation, critique, plan, ranking, diagnosis, or research synthesis
- important assumptions or constraints
- material dissent or arbitration needs, if any
- exactly one council state label, optionally in a natural-language sentence
- next options only when useful

For artifact creation, deliver the artifact itself. A recipe should include ingredients, method, expected result, and notes. A spec should be usable as a spec. Do not bury the output under process narration.

Use exactly one council state label:

- `resolved`: the steward can deliver a strong final result under the mandate.
- `conditional resolution`: the result is strong if named constraints or assumptions hold.
- `unresolved dissent`: principled disagreement remains and should be presented.
- `needs user arbitration`: a user value, preference, missing fact, or priority must be decided before a single best answer is honest.
- `round budget reached`: allowed rounds ended without a clean resolution or arbitration frame.

Example: "Council resolution: `conditional resolution`; this recommendation holds if the stated budget and timeline assumptions remain true."

Next options are optional. Include them only when the steward has specific, useful follow-ups that would help the user refine, arbitrate, correct, pressure-test, or extend the work. Omit them when the answer is complete and no obvious next step would materially help, when the user asked for final-only output, when the user asked for no follow-up options, or when options would clutter a finished artifact.

If exactly one follow-up is worth suggesting, phrase it so the user can answer `yes` or `no`. If multiple follow-ups are worth suggesting, use a numbered list so the user can answer with a number. Avoid vague generic endings.

## Continuation

After the first resolution, preserve a compact council packet: mandate, mission type, roster and responsibilities, prior resolution, important assumptions, key tensions, material dissent or arbitration points, councilor questions and canonical answers, council state, relevant skill behavior, and round budget or notable round limits used.

If the user asks a small clarification or minor edit, answer directly without reconvening the council.

If the user accepts a suggested follow-up, chooses a numbered option, asks for a meaningful refinement, requests a correction, asks for variants, or asks to pressure-test the result, run a targeted continuation round without re-proposing the whole council unless membership or mandate changes materially.

Reuse existing councilors and context when available and useful. If existing councilors are unavailable or hidden state is unavailable, reconstruct the packet from the prior final answer and available conversation context, then spawn fresh targeted councilors with that packet plus the user's new request. Be explicit that fresh councilors are continuing from a packet, not from remembered prior context.

If the user changes the mandate materially, propose an updated mandate or council before dispatch. If the user asks for a specific councilor's view, ask that councilor or answer from existing councilor output when no new judgment is needed. If the user asks for a final artifact after reviewing options, produce it directly unless more council work is genuinely useful.

## Anti-Patterns

- Triggering because the task is important but the user did not ask for a council-like process.
- Producing final recommendations during the proposal phase.
- Creating councilors that are just labels for the same perspective.
- Forwarding raw full responses to every councilor.
- Running pairwise councilor debates.
- Stopping at a list of opinions instead of synthesizing.
- Averaging incompatible suggestions into weak compromise.
- Hiding dissent that matters.
- Delegating parent-owned workflow gates to councilors.
- Ending every resolution with generic follow-up text.
- Offering vague next steps when the result is already complete.
- Forwarding raw councilor question dumps to the user.
- Passing every councilor question to the user instead of triaging.
- Turning every council into an interview.
- Running discovery to satisfy councilor curiosity instead of decision impact.
- Asking unrelated compound question batches instead of the single highest-value next question.
- Continuing discovery after explicit assumptions are sufficient.
- Letting councilors decide to continue discovery or question the user directly.
- Assuming fresh councilors remember prior council context.
