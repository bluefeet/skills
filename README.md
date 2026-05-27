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
