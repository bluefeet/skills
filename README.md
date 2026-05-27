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
