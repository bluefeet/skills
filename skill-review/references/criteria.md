# Skill Review Criteria

Use this reference when reviewing a skill directory, a `SKILL.md` file, or pasted skill text. Apply criteria by evidence, not by guesswork. Reserve Behavioral Checks Needed for criteria that require observed usage, eval output, or transcripts. When directory files, adjacent-skill context, deployment context, or user intent are unavailable, label the limitation as contextual evidence in the finding or scorecard notes.

## Severity

- `blocker`: invalid, unsafe, or likely to prevent the skill from working.
- `major`: likely to degrade triggering, task quality, correctness, or repeatability.
- `minor`: worthwhile quality issue, but not urgent.
- `advisory`: improvement opportunity or situational best practice.

## Applicability

- `universal`: applies to every skill.
- `conditional`: applies only when the skill has the relevant feature.
- `scaling`: important when the skill is being shared, reused broadly, or productionized.

## Evidence

- `static`: can be checked from the skill files alone.
- `behavioral`: requires observing real skill usage, eval output, or transcripts.
- `contextual`: depends on intended users, project conventions, or deployment context.

## Category Scorecard Guidance

- `strong`: criteria in this category are substantially satisfied and the evidence is clear.
- `adequate`: no major concern is visible, but there is room to improve.
- `needs work`: at least one blocker or major issue exists in this category.
- `not applicable`: the category does not apply to the reviewed skill.
- `needs behavioral evidence`: file review is insufficient to judge important criteria in this category.

## Categories

- **Metadata Validity:** structurally valid name and description.
- **Discovery And Activation:** whether the skill can be selected for the right tasks.
- **Scope And Applicability:** boundaries, preconditions, exclusions, and overuse prevention.
- **Context Efficiency:** token cost, concision, and load discipline.
- **Information Architecture:** navigability after content is split across files.
- **Instruction Design:** whether instructions match task fragility and workflow complexity.
- **Output And Example Design:** templates and examples that make desired results concrete.
- **Validation And Feedback Loops:** ways the skill helps the agent catch mistakes.
- **Portability, Freshness, And Consistency:** stale guidance, path style, terminology, and platform assumptions.
- **Executable Assets And Dependencies:** bundled scripts, tools, runtime assumptions, and dependency clarity.
- **Evaluation Quality And Efficiency:** whether evals are representative, high-signal, cost-aware, non-redundant, and designed to test skill behavior without unnecessary live tool calls.

## Criteria Table

| ID | Category | Criterion | Severity | Applicability | Evidence | Review Guidance | Common Recommendation |
| --- | --- | --- | --- | --- | --- | --- | --- |
| metadata.valid-name | Metadata Validity | Skill has a valid lowercase slug `name` using lowercase letters, digits, and hyphens when needed. | blocker | universal | static | Check frontmatter and directory name when available. Single-word names are valid when they otherwise satisfy naming constraints. | Rename to a lowercase slug identifier and keep frontmatter aligned. |
| metadata.valid-description | Metadata Validity | Skill has a non-empty `description` in frontmatter. | blocker | universal | static | Check frontmatter presence and basic parseability. | Add a concise description that explains what the skill does and when to use it. |
| metadata.description-format | Metadata Validity | Description is within metadata constraints and contains no markup that could break loading. | major | universal | static | Check for excessive length, XML-like tags, or malformed frontmatter. | Shorten and simplify the description. |
| metadata.name-clarity | Metadata Validity | Name is clear, memorable, and aligned with the task. | minor | universal | contextual | Compare name against the skill purpose and likely user phrasing. | Rename only if the current name would confuse selection or maintenance. |
| activation.what-it-does | Discovery And Activation | Description states what the skill does. | major | universal | static | Read the description without the body; it should communicate purpose. | Rewrite the description to name the capability directly. |
| activation.when-to-use | Discovery And Activation | Description states when the skill should be used. | major | universal | static | Look for trigger contexts, user intents, or task situations. | Add trigger conditions and realistic task contexts. |
| activation.specificity | Discovery And Activation | Description is specific enough to compete with other skills. | major | universal | contextual | Consider whether adjacent skills would be confused. | Add distinguishing task boundaries and concrete domains. |
| activation.not-overbroad | Discovery And Activation | Description avoids triggering for unrelated tasks. | major | universal | contextual | Look for broad verbs with no domain boundary. | Narrow the trigger language and define excluded cases when useful. |
| activation.realistic-phrasing | Discovery And Activation | Trigger language covers realistic user phrasing. | minor | universal | contextual | Compare against likely user requests, not only formal labels. | Add natural trigger examples or phrasing in the description. |
| scope.inputs | Scope And Applicability | Expected inputs or starting context are clear when needed. | major | conditional | static | Check whether the workflow assumes files, URLs, pasted text, tools, or prior state. | State required inputs or how to proceed when they are absent. |
| scope.boundaries | Scope And Applicability | Skill defines boundaries or deferral cases when overuse is likely. | major | conditional | contextual | Look for adjacent tasks where the skill should not apply. | Add boundary guidance or examples of non-use. |
| scope.required-vs-optional | Scope And Applicability | Required behavior is distinguished from optional or maturity-dependent guidance. | minor | universal | static | Check whether all advice sounds equally mandatory. | Label optional, conditional, or scaling guidance clearly. |
| context.concise-body | Context Efficiency | `SKILL.md` is concise enough to load comfortably. | major | universal | static | Count lines and scan for repeated or tutorial-like material. | Move dense detail to references and remove low-value prose. |
| context.no-obvious-background | Context Efficiency | Skill avoids teaching background the agent already knows. | minor | universal | static | Look for generic explanations that do not affect task behavior. | Replace background with task-specific decision guidance. |
| context.details-deferred | Context Efficiency | Large detail is moved into directly linked references. | major | conditional | static | Check whether specialized details are inline despite being needed only sometimes. | Move conditional material to a focused reference file. |
| context.examples-earn-cost | Context Efficiency | Examples and prose earn their token cost. | minor | universal | static | Ask whether each example changes likely behavior. | Remove repetitive examples or replace them with one high-signal example. |
| architecture.progressive-disclosure | Information Architecture | Skill uses progressive disclosure for substantial detail. | major | conditional | static | Check whether `SKILL.md` points to references instead of embedding everything. | Add focused reference files and load rules. |
| architecture.direct-links | Information Architecture | Important references are linked directly from `SKILL.md`. | major | conditional | static | Check for reference chains that require multiple hops. | Link important files directly from the main skill. |
| architecture.one-level | Information Architecture | References avoid unnecessary nesting. | minor | conditional | static | Inspect reference depth and navigation effort. | Flatten reference structure unless hierarchy is clearly useful. |
| architecture.descriptive-files | Information Architecture | Bundled files use descriptive names. | minor | conditional | static | Check whether names explain purpose without opening files. | Rename vague files to describe their role. |
| architecture.toc-long-files | Information Architecture | Long reference files have a table of contents. | minor | conditional | static | Apply to reference files long enough to be hard to scan. | Add a short table of contents near the top. |
| architecture.domain-split | Information Architecture | Domain-specific material is separated into focused files. | minor | conditional | static | Check whether unrelated variants share one long reference. | Split by domain, framework, provider, or workflow path. |
| instruction.specificity-fit | Instruction Design | Instruction specificity matches task fragility. | major | universal | contextual | Fragile tasks need exact steps; judgment tasks need principles. | Make fragile paths explicit or loosen over-prescriptive judgment guidance. |
| instruction.workflow | Instruction Design | Complex tasks have a clear sequential workflow. | major | conditional | static | Look for multi-step tasks without order or gates. | Add a numbered workflow with decision points. |
| instruction.conditionals | Instruction Design | Divergent paths have conditional guidance. | major | conditional | static | Check for "if this, then that" needs. | Add branch guidance for important variants. |
| instruction.no-option-overload | Instruction Design | Skill avoids presenting too many equivalent options. | minor | universal | static | Look for menus where a default would help. | Provide a recommended default and explain when to choose alternatives. |
| instruction.tool-clarity | Instruction Design | Tool or MCP references are clear and fully qualified when relevant. | major | conditional | static | Check whether tool calls are named precisely enough to execute. | Use exact tool names and conditions for using them. |
| output.template-needed | Output And Example Design | Strict output templates are provided when structure matters. | major | conditional | static | Check whether downstream use depends on exact sections or fields. | Add the required report, file, or response template. |
| output.template-not-overstrict | Output And Example Design | Strict templates are used only when strictness helps. | minor | universal | contextual | Look for rigid format where judgment or conversation would be better. | Relax rigid sections that do not serve the task. |
| output.examples-concrete | Output And Example Design | Concrete input/output examples exist when style or format matters. | minor | conditional | static | Check whether examples teach the desired output. | Add realistic examples with input and output. |
| output.examples-not-abstract | Output And Example Design | Examples are specific rather than abstract. | minor | conditional | static | Look for examples too generic to guide behavior. | Replace abstract examples with realistic cases. |
| validation.loop | Validation And Feedback Loops | Quality-critical work includes a validation loop. | major | conditional | static | Check for review, test, feedback, or confirmation steps. | Add a verification or review pass before final output. |
| validation.intermediate-artifacts | Validation And Feedback Loops | High-risk or batch work creates verifiable intermediate outputs. | major | conditional | static | Look for workflows where final-only output hides errors. | Add checkpoints or intermediate artifacts. |
| validation.error-recovery | Validation And Feedback Loops | Error recovery is actionable. | major | conditional | static | Check whether failure cases say what to inspect or do next. | Add concrete recovery steps and escalation points. |
| portability.time-sensitive | Portability, Freshness, And Consistency | Time-sensitive guidance is isolated or marked. | major | conditional | static | Look for date-sensitive facts in the main path. | Move volatile guidance to references or require verification. |
| portability.legacy-separated | Portability, Freshness, And Consistency | Legacy or outdated patterns are separated from current guidance. | minor | conditional | static | Check whether older approaches compete with current instructions. | Label legacy guidance and keep current path prominent. |
| portability.terminology | Portability, Freshness, And Consistency | Terminology is consistent. | minor | universal | static | Scan for multiple names for the same concept. | Pick one term and use it consistently. |
| portability.paths | Portability, Freshness, And Consistency | Paths use forward slashes. | minor | universal | static | Scan file paths in instructions. | Convert path examples to forward slashes. |
| portability.platform-assumptions | Portability, Freshness, And Consistency | Platform assumptions are documented when relevant. | major | conditional | contextual | Check for commands, tools, or environments assumed silently. | State required environment or fallback behavior. |
| executable.utility-scripts | Executable Assets And Dependencies | Deterministic or fragile work uses bundled scripts when useful. | advisory | conditional | contextual | Look for repeated code generation or error-prone manual steps. | Add a script only when it materially reduces repeated fragile work. |
| executable.dependencies | Executable Assets And Dependencies | Dependencies and runtime assumptions are listed. | major | conditional | static | Inspect scripts and instructions for required tools or libraries. | Document dependencies, versions when needed, and runtime assumptions. |
| executable.run-vs-read | Executable Assets And Dependencies | Instructions clarify whether scripts should be run or read. | major | conditional | static | Check how bundled scripts are referenced. | Say when to execute scripts and what outputs to expect. |
| executable.errors | Executable Assets And Dependencies | Scripts handle errors with useful messages. | major | conditional | behavioral | Static review can inspect intent; execution confirms behavior. | Add clear failures, validation, and next-step messages. |
| executable.constants | Executable Assets And Dependencies | Constants and configuration are justified. | minor | conditional | static | Look for unexplained magic values. | Name or document important constants and configuration. |
| evaluation.scenarios | Evaluation Quality And Efficiency | Representative eval scenarios exist for shared or productionized skills. | major | scaling | static | Check whether evals cover expected real usage. | Add representative eval prompts and expected outputs. |
| evaluation.realistic | Evaluation Quality And Efficiency | Evals use real usage scenarios rather than toy cases. | minor | scaling | static | Read prompts for realistic context and edge cases. | Rewrite toy prompts into realistic user tasks. |
| evaluation.skill-gap | Evaluation Quality And Efficiency | Evals test the gap the skill is meant to solve. | major | scaling | contextual | Compare evals to the skill's purpose. | Add cases where the skill should improve behavior versus baseline. |
| evaluation.non-redundant | Evaluation Quality And Efficiency | Evals cover distinct behaviors, risks, or user scenarios rather than duplicating each other. | major | scaling | contextual | Compare eval prompts and expectations for overlap in the behavior they test. | Prune or merge duplicate evals and keep the strongest representative case. |
| evaluation.high-signal | Evaluation Quality And Efficiency | Each eval is likely to reveal meaningful skill behavior or a concrete improvement opportunity. | major | scaling | contextual | Ask what would change in the skill if this eval failed. | Rewrite or remove evals whose failure would not guide an improvement. |
| evaluation.discriminating | Evaluation Quality And Efficiency | Evals test behavior the skill should improve, not behavior a baseline would already handle well. | major | scaling | behavioral | Compare with baseline or reason from the skill's intended gap when baseline results are unavailable. | Add cases where the skill should materially outperform no-skill or prior behavior. |
| evaluation.priority-coverage | Evaluation Quality And Efficiency | Evals prioritize important workflows and failure modes over narrow polish. | major | scaling | contextual | Check whether eval cost is spent on core risks rather than small formatting details. | Replace low-value checks with cases covering important decisions or failure modes. |
| evaluation.cost-aware | Evaluation Quality And Efficiency | The core eval suite is small and cheap enough to rerun often. | minor | scaling | contextual | Look for unnecessary live tool calls, large fixtures, or many overlapping cases. | Keep a minimal core suite and move broad or expensive cases to an expanded suite. |
| evaluation.prune-low-value | Evaluation Quality And Efficiency | Duplicate, flaky, always-passing, low-actionability, or over-narrow evals are pruned or rewritten. | major | scaling | behavioral | Requires eval outcomes, transcripts, or repeated runs to identify pass/fail patterns and flakiness. | Remove or rewrite evals that do not provide reliable decision value. |
| evaluation.core-vs-expanded | Evaluation Quality And Efficiency | Expensive or broad evals are separated from the fast core suite. | advisory | scaling | static | Check whether eval organization distinguishes routine checks from occasional broad validation. | Split evals into fast core coverage and slower expanded coverage when cost warrants it. |
| evaluation.tool-output-fixtures | Evaluation Quality And Efficiency | Tool-result interpretation evals provide sample tool output instead of requiring live tool calls. | major | conditional | static | Check whether evals meant to test response to tool results include representative outputs inline or as files. | Replace live tool dependency with fixture output unless the eval is explicitly testing integration. |
| evaluation.fixture-realism | Evaluation Quality And Efficiency | Tool-output fixtures are realistic enough to exercise success, failure, ambiguity, and edge cases. | major | conditional | contextual | Inspect whether fixtures resemble real command, API, or tool responses and cover meaningful branches. | Add realistic fixture examples for success, failure, ambiguous, and edge-case tool outputs. |
| evaluation.reasoning-vs-integration | Evaluation Quality And Efficiency | Evals distinguish tool-result reasoning from live tool integration. | major | conditional | contextual | Identify whether the eval purpose is reasoning over output or end-to-end tool invocation. | Use fixtures for reasoning evals and reserve live calls for integration evals. |
| evaluation.baseline | Evaluation Quality And Efficiency | Baseline or prior-version comparison exists when useful. | advisory | scaling | behavioral | Requires test execution or review of outputs. | Compare with no-skill or previous-skill behavior when evaluating maturity. |
| evaluation.navigation-observed | Evaluation Quality And Efficiency | Agent navigation observations inform revisions. | advisory | scaling | behavioral | Requires transcripts, eval runs, or usage notes. | Review how agents actually load and use the skill before broad rollout. |

## Evidence And Recommendation Examples

Strong evidence is specific: file path, section name, line number when available, or a precise paraphrase of the relevant text. Weak evidence is vague, such as "the skill seems long" without showing where the length or repetition appears.

Good recommendations name the next edit. For example: "Move the provider-specific deployment details from `SKILL.md` into `references/aws.md` and link that file from the workflow branch that handles AWS." Avoid generic advice such as "improve organization."

When evidence is behavioral, do not present it as proven by static review. Put it under Behavioral Checks Needed or label the scorecard row `needs behavioral evidence`.
