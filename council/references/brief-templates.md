# Council Brief Templates

Use these templates after the council skill has triggered. Adapt details to the user's actual mandate, but keep the structure so council work stays comparable and easy to synthesize.

## Council Proposal

```md
**Mandate** <one or two sentences describing the outcome the council is being convened to produce or improve>

**Mission Type** <create | decide | critique | plan | diagnose | research | rank/select | red-team | combined types>

**Proposed Council**

1. **<Councilor Name>:** <contribution or responsibility>. Blind spot: <likely blind spot>.
2. **<Councilor Name>:** <contribution or responsibility>. Blind spot: <likely blind spot>.
3. **<Councilor Name>:** <contribution or responsibility>. Blind spot: <likely blind spot>.

<If skill behavior matters:>
**Skill Behavior**
<briefly state whether guidance will be copied, a skill file will be loaded, or workflow responsibility stays with the parent>

Does this council look right, or would you like to change the mandate or membership before I convene them?
```

Proposal-phase responses stop after the approval or revision question. Do not include invented councilor stances, partial answers, or final recommendations.

## First-Pass Councilor Brief

```md
You are a councilor in a council convened by the steward.

Mandate: <mandate>

Mission Type: <mission type>

Your Role:

- Name: <role name>
- Expertise: <relevant expertise>
- Evaluation lens: <stakeholder, worldview, or quality lens>
- Success criteria: <what good looks like from this role>
- Likely blind spots: <what this role may underweight>
- Responsible for noticing: <specific risks or contributions>

Relevant Context: <user constraints, prior decisions, artifacts, or facts>

Relevant Skill Behavior: <Copied guidance | Skill file to load | Parent-owned workflow, if applicable>

Instructions:

- Reason independently before seeing other councilor outputs.
- Represent your role well; do not try to cover every viewpoint.
- Separate `Blocking Questions`, `Optional Questions`, and `Assumptions If Unanswered`.
- Do not talk directly to other councilors.
- Keep your response concise and schema-shaped.

Output Contract: <mission-specific contract from references/mission-types.md>

Question Fields:

- Blocking Questions: <questions that must be answered before useful progress is possible, or "None">
- Optional Questions: <questions that would improve quality but are not required, or "None">
- Assumptions If Unanswered: <assumptions you will use if no answer is available, or "None">
```

## Discovery Round Councilor Brief

Use this before synthesis when the mandate is underconstrained, preference-heavy, arbitration-heavy, or likely to improve through staged clarification. This is a lightweight discovery pass, not a full first pass.

```md
You are a councilor in a discovery round convened by the steward.

Mandate: <mandate>

Mission Type: <mission type>

Your Role: <role definition>

Relevant Context: <known facts, constraints, user preferences, prior answers, or "None">

Instructions:

- Propose only questions whose answers would materially change the resolution, arbitration frame, or confidence.
- Separate user-answerable uncertainty from questions the steward can likely answer from provided context.
- Include a default assumption for every question in case the steward does not ask it.
- Do not ask the user directly.
- Do not produce a full recommendation in this round.

Question Candidates:

- Question: Why It Matters: Priority: blocking | optional | arbitration Likely Answerable From Provided Context?: yes | no | uncertain Context Source If Answerable: Default Assumption If Unasked: Who Needs The Answer: all councilors | <named role(s)> | steward only
```

## Relevant Skill Behavior

Use one of these shapes inside the councilor brief when another skill matters:

```md
Relevant Skill Behavior: Copied guidance:

- <specific workflow rule, output format, or quality bar the councilor must apply>
```

```md
Relevant Skill Behavior: Skill file to load:

- Path: <exact skill path>
- Use only: <specific sections or behavior needed for this councilor>
```

```md
Relevant Skill Behavior: Parent-owned workflow:

- <the parent steward owns this workflow gate or artifact>
- Your job is to respect these constraints and improve the work from your lens, not execute the workflow yourself.
```

## Targeted Review Brief

```md
You are reviewing the steward's candidate resolution from your councilor role.

Mandate: <mandate>

Your Role: <role summary>

Candidate Resolution Summary: <brief steward synthesis or draft>

Review Contract:

- What works:
- What is weak, wrong, or missing:
- Most important change:
- Blocker:
- Endorsement state: endorse | endorse with conditions | do not endorse
```

Do not ask councilors to rewrite the whole result unless the candidate is structurally wrong. The review round is for focused improvement.

## Steward Question Triage

Use this when councilors return blocking or optional questions. The steward should remove duplicates, answer from known context when safe, and ask the user only for facts, preferences, permissions, constraints, or arbitration that the steward cannot infer.

```md
Councilor Questions: <deduplicated questions grouped by theme>

Answered From Context:

- Question: <question>
- Answer: <answer>
- Source: <prompt | prior context | mandate | proposal | skill guidance | steward procedure>
- Share with: <all councilors | named councilor>

Ask User:

- Question: <single highest-value user-facing question>
- Why it matters: <brief impact on the final result>
- Priority: blocking | arbitration
- Expected impact if answered: <what could change>

Proceed With Assumption:

- Question: <non-blocking or low-value question>
- Assumption: <explicit assumption>
- Reason not asked: <answerable from context | optional | low expected value | question budget reached>
- Share with: <all councilors | named role(s) | steward only>
```

## Discovery Answer Broadcast

Use this after the steward asks a discovery question or answers one from context.

```md
Canonical Answer:

- Question: <question>
- Answer: <answer>
- Source: <user | prompt | prior context | mandate | proposal | skill guidance | steward procedure>
- Share with: <all councilors | named role(s) | steward only>

Councilor Response Requested:

- What materially changes because of this answer:
- Any remaining material blocker:
- Assumption to use if discovery or questioning stops now:
```

## Continuation Brief

Use this when the user asks to refine, correct, question, pressure-test, or extend a prior council resolution and the mandate does not require proposing a materially new council.

```md
You are joining a targeted continuation round for an existing council.

Continuation Request: <the user's new request>

Council Packet:

- Mandate: <original or updated mandate>
- Mission type: <mission type>
- Roster and responsibilities: <compact list>
- Prior resolution: <short summary or link/paste>
- Important assumptions: <assumptions>
- Key tensions: <tensions>
- Material dissent or arbitration points: <points or "None">
- Councilor questions and answers: <canonical Q&A or "None">
- Council state: <state>
- Relevant skill behavior: <copied guidance, skill path, or parent-owned workflow>
- Round budget or notable round limits used: <limits or "Default council budget">

Your Role In This Continuation: <specific lens and what to check now>

Output Contract:

- What changes because of the user's request:
- What should stay stable from the prior resolution:
- Risks or regressions to avoid:
- Most important recommendation:
- Blocking Questions: <questions that must be answered, or "None">
- Optional Questions: <questions that would improve quality, or "None">
- Assumptions If Unanswered: <assumptions, or "None">
```

## Compact Progress Updates

Use short updates while the council works. The user should see the shape of the work without reading transcripts.

Good examples:

- "I have convened the four councilors: flavor, food science, nutrition, and home practicality."
- "First passes are in. The strongest tension is texture versus carb count; I am synthesizing around the food scientist's bake method and the nutrition reviewer's crust constraints."
- "I am sending the candidate recipe back for targeted review now, focused on blockers rather than rewrites."
- "Review is complete. I am folding in the crack-prevention fix and simplifying one ingredient choice before the final recipe."

Avoid:

- full councilor essays
- transcript dumps
- reporting every councilor detail
- exposing half-finished drafts as final

## Final Steward Quality Gate

Before delivering the resolution, check:

- Does the final result satisfy the mandate?
- Did I adopt the strongest ideas rather than average opinions?
- Did I discard weak, incompatible, or off-mandate suggestions?
- Did I preserve material dissent or arbitration needs without over-reporting process noise?
- Can the user use the final answer as work product without reading council transcripts?
- Did I respect any final-only or no-follow-up instruction?
