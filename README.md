# Aran's Agent Skills

Install with:

```
npx skills@latest add git@github.com:bluefeet/skills.git
```

## The Skills

### commit-organizer

Turns messy committed Git branch history into a clean, reviewable sequence of focused linear commits. It fits branch-history cleanup before PR review, including splitting, squashing, reordering, reconstructing, or organizing existing commits.

It starts with read-only Git inspection, resolves the exact base and original branch tip, builds a diff-first commit plan, and requires approval before rewriting history. Safety checks include a clean working tree, a local backup branch, per-commit verification, and final diff equivalence. It is not for ordinary commits, commit-message drafting, generic Git advice, or standalone conflict recovery.

Example prompts:

- "Organize this branch into reviewable commits before I open a PR."
- "Split my messy local history into a clean set of logical commits."
- "Squash and reorder the committed changes on this branch so the story is easier to review."

Read more at [commit-organizer/SKILL.md](commit-organizer/SKILL.md).

### council

Convenes a small, explicit group of specialized subagents for decisions, critiques, plans, diagnoses, research synthesis, or creative artifacts. It is strongest when distinct expert lenses would catch different risks or produce a better synthesis than one linear answer.

The steward first proposes a mandate, mission type, and councilors for approval. After approval, it gathers independent councilor passes, synthesizes the strongest contributions, preserves meaningful dissent, and returns a resolution. Ordinary requests for careful review, deep thinking, or perspectives remain single-agent unless the user explicitly asks for a council, panel, specialists, subagents, or similar multi-agent process.

Example prompts:

- "Use a council to pressure-test this launch plan before I commit to it."
- "Have a panel of experts help me choose between these three job offers."
- "Use several specialist subagents to review this feature spec before implementation."

Read more at [council/SKILL.md](council/SKILL.md).

### git-conflict

Resolves and recovers from Git conflicts or interrupted Git operations, including merge, rebase, cherry-pick, revert, stash apply or pop, autostash, marker-only cleanup, and safe branch rebase or update previews.

Before interpreting sides or recommending changes, it detects the actual Git state. It separates read-only inspection from mutating actions, asks for confirmation before editing, staging, continuing, skipping, aborting, or rebasing, and verifies that intended resolved paths have no remaining unmerged entries or conflict markers. It is not for generic Git advice, ordinary commits, commit-message drafting, or branch-history cleanup.

Example prompts:

- "Help me resolve this rebase conflict."
- "I have conflict markers in a file but Git says nothing is unmerged. Clean this up."
- "Preview updating this branch onto main and tell me the risks before rebasing."

Read more at [git-conflict/SKILL.md](git-conflict/SKILL.md).

### home-buyer-profile

Creates and maintains a durable markdown profile for someone buying a residential home to live in. The profile captures search criteria, budget readiness signals, locations, dealbreakers, preferences, tradeoffs, uncertainty, rejected signals, and approved updates discovered from listing feedback.

It writes and updates `~/home-search/buyer-profile.md`, asks adaptive one-question-at-a-time interview prompts, and avoids storing sensitive financial details. It is scoped to residential home purchases and should come before buyer-centered listing scoring or comparison when no profile exists.

Example prompts:

- "Create a home buyer profile for my search."
- "Update my buyer profile with these new dealbreakers."
- "Help me clarify my budget, location, and tradeoff criteria before I compare listings."

Read more at [home-buyer-profile/SKILL.md](home-buyer-profile/SKILL.md).

### home-listing-analyzer

Manages durable markdown records for residential home-for-purchase listings in a buyer's search. It imports, updates, scores, compares, and reports on listings under `~/home-search/listings/`, using the buyer profile as the foundation for fit analysis.

During import or update, it gathers source content, confirms the listing is in scope, requires a full street address, checks for duplicates, creates or updates a main listing file plus a companion source file, and preserves facts, feedback, unknowns, fit analysis, profile signals, and activity history. If no buyer profile exists, the buyer profile workflow comes first.

Example prompts:

- "Import this home listing into my search."
- "Score this listing against my buyer profile."
- "Compare the active listings and tell me which ones deserve a tour."

Read more at [home-listing-analyzer/SKILL.md](home-listing-analyzer/SKILL.md).

### skill-review

Reviews an existing skill directory, `SKILL.md` file, or pasted skill text for skill-authoring quality. It audits metadata, trigger behavior, scope boundaries, progressive disclosure, instruction design, examples, bundled assets, dependencies, validation loops, and evaluation quality.

The output is a best-practices review with an overall status, strengths, severity-ranked findings, a category scorecard, behavioral checks needed, and suggested improvement order. It distinguishes static evidence from behavior that would require observed usage, eval outputs, or transcripts; it does not run a full eval loop itself.

Example prompts:

- "Review this skill for trigger reliability and authoring quality."
- "Audit `skill-review/SKILL.md` and tell me what needs improvement."
- "Quality-check this pasted skill before I install it."

This skill was greatly inspired by the [Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) from the Claude API Docs.

Read more at [skill-review/SKILL.md](skill-review/SKILL.md).
