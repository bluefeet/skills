# AGENTS.md

## General Guidelines

- It is OK to work directly on `main` in this repository. Do not ask for additional permission just because the current branch is `main`.
- Do not create or require git worktrees unless the user explicitly asks for one.
- Do not commit spec and plan files.
- Before committing run `npm run format:check`.

## Skill Authoring

- Write skills and evals so they can run without the current conversation context.
- Keep skills and evals portable across engineers, tools, and coding agents. Do not assume Codex, Claude, unrelated installed skills, user preferences, or a specific runtime unless explicit and central.
- Run skill evals in isolation. The only intentional difference between compared runs should be the skill under test; if isolation is not possible, label the run contaminated and do not treat it as benchmark evidence.
- Prefer a few high-signal evals over broad coverage. Each eval should test a distinct behavior, boundary, or failure mode that matters.
- Design evals around the behavioral gap the skill is meant to close. If a no-skill or old-skill run would likely pass the eval, treat it as a regression guard, not evidence of improvement.
- Use minimal sufficient context. Include the facts and fixtures needed to make the eval fair, but do not include the skill's workflow, heuristics, or answer key in the prompt.
- Prefer fixture-based evals over live work. If a skill would normally run a command or inspect tool output, provide realistic fake output and ask what the agent should do from there.
- Fixtures should provide observed facts, not conclusions the skill is supposed to infer.
- Grade durable behavior: decisions, safety boundaries, required outputs, important omissions, and unacceptable actions. Avoid exact wording checks unless exact wording is the contract.
- Keep evals with the skill as it changes. Prune duplicate, flaky, always-passing, low-actionability, temporary, or implementation-detail evals instead of expanding the suite by default.

## README Maintenance

Treat `README.md` as the user-facing index of the skills in this repository. Keep skill sections in `README.md` sorted alphabetically by skill name.

When adding, removing, renaming, or materially changing a skill, check whether the README needs to change as part of the same work.

Material changes include changes to when the skill should be used, how it is triggered, what workflow it follows, or what kind of output users should expect.

Update `README.md` when the public name, existence, purpose, trigger boundary, usage guidance, or representative prompts for a skill change. If no README update is needed, mention that explicitly in the final response.

When editing multiple skill descriptions, check the set as a whole for repeated sentence patterns, duplicated openings, inconsistent detail level, and descriptions that read like activation rules instead of a browsable index.

Use this shape for skill sections unless there is a clear reason the skill needs a different structure:

```md
### skill-name

Start with a plain-language outcome: what problem the skill helps solve or what result it produces. Avoid opening every section with the same phrase, especially metadata-like starts such as "Use this when...".

Follow with the trigger boundary, workflow promise, and any important exclusion or dependency a real user needs to choose the right skill. Prefer reader value over implementation inventory; include internal mechanics only when they affect trust, safety, prerequisites, stored files, or expected output.

Example prompts should sound like requests a real user would write, not like test cases or descriptions of the skill's implementation.

Example prompts:

- "Example prompt one."
- "Example prompt two."
- "Example prompt three."

Read more at [skill-name/SKILL.md](skill-name/SKILL.md).
```

## Commit Messages

When creating or rewriting commits, use Conventional Commits.

- Format: `<type>: <description>`
- Keep the header 50 characters or less.
- Header gives the concise review headline: the main change a reviewer should recognize in `git log`.
- Body provides durable context that the header cannot fit or the diff cannot make obvious. Use it for meaningful what, why, boundaries, tradeoffs, dependencies, migration notes, safety constraints, or review guidance.
- Diff shows the exact implementation. Do not use the body to narrate obvious file edits, list touched files, repeat the ticket, or preserve temporary development trivia.
- Add a body when it materially improves future review, blame, archaeology, or maintenance. Omit it when the header and diff already carry the useful context.
- Write the body as paragraphs by default. Use bullets when there are several distinct points, and section those bullets only when they naturally group by topic.
- Wrap body paragraphs and bullets to 72 columns with shell commands.
- Base the message only on the contents of the commit.
- Use lowercase types such as `feat`, `fix`, `test`, `docs`, `refactor`, `chore`, `build`, or `ci`.

Examples:

- `test: confirm that apples are not oranges`
- `docs: described how orange trees grow`
- `fix: removed purple apples`
