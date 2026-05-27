---
name: commit-organizer
description: Organizes committed Git branch history into focused, reviewable linear commits. Use when the user asks to clean up, split, squash, reorder, reconstruct, or make committed branch or range history reviewable before a PR. Do not use for ordinary commit creation, commit-message drafting, generic Git advice, or standalone conflict recovery.
---

# Commit Organizer

Organize committed Git changes into a small set of focused, reviewable commits. Work from the final diff, not from messy development history. The default executable path is rewriting the current branch from one approved base to the original branch tip.

## Activation Boundary

Use this skill when the user wants committed branch history cleaned up for review:

- split, squash, reorder, reconstruct, or organize existing commits
- make a branch or committed range reviewable before opening a PR
- replace messy local history with a clean linear sequence

Do not use this skill for:

- creating an ordinary commit from current working-tree changes
- drafting commit messages only
- generic Git explanations or one-off Git advice
- standalone conflict, rebase, merge, cherry-pick, revert, bisect, or stash recovery

If conflicts or interrupted Git operations occur during an approved commit-organization rewrite, stop and use the repo's Git conflict-resolution workflow if available.

## Execution Contract

Use these terms consistently in executable rewrite plans:

- `BASE_SHA`: exact commit the current branch will reset to for an executable rewrite.
- `ORIGINAL_HEAD`: branch tip before rewrite execution begins.
- Default executable scope: current branch from `BASE_SHA` to `ORIGINAL_HEAD`.
- Diff equivalence: compare starting and final branch diffs against the same `BASE_SHA`.

An explicit user base can define executable `BASE_SHA..ORIGINAL_HEAD` scope after resolving the exact `BASE_SHA`. An explicit arbitrary or non-`HEAD` range defines analysis scope only. Do not mutate history for an arbitrary or non-`HEAD` range until the user approves a concrete execution strategy.

## Core Safety Invariants

- Require a clean working tree before execution.
- Do not mutate Git history before user approval.
- Treat the captured starting diff against `BASE_SHA` as the source of truth.
- Create and verify a local backup branch before reset or rewrite.
- Keep each commit green under relevant existing checks unless the user explicitly approves a non-green exception.
- Do not push or force-push without separate explicit approval.
- Use `references/rewrite-mechanics.md` for every approved executable rewrite.

Approved executable rewrites assume a modern Git with `git restore` and a POSIX-like shell with `mktemp` and `cmp`. If those prerequisites are missing or behave differently, stop before mutation and ask how to proceed.

## State And Scope Gate

Start with base-independent read-only inspection:

```sh
git rev-parse --show-toplevel
git status --short --branch
git status
git status --porcelain
git branch --show-current
git rev-parse HEAD
git rev-parse --abbrev-ref --symbolic-full-name @{u}
git diff --name-only --diff-filter=U
git ls-files -u
```

Use the user's explicit base or range when provided. Otherwise choose a base only when it is locally unambiguous. Ask instead of guessing when upstream, default branch, stacked branch parent, or range meaning is unclear.

Dirty state blocks execution. If `git status --porcelain` prints anything, do not create an executable plan or rewrite. If the user explicitly asks only for advice, you may give tentative planning guidance, but label it non-executable until the tree is clean.

Active Git operations or an unmerged index block execution and planning-for-execution. Use full `git status` plus the unmerged-index output from `git diff --name-only --diff-filter=U` and `git ls-files -u` before scoped planning. If a rebase, merge, cherry-pick, revert, bisect, or unmerged index is active, stop and route the user to conflict/recovery handling before organizing commits.

After resolving an analysis range or executable `BASE_SHA`, inspect the scoped diff:

```sh
git log --oneline --decorate <resolved-scope>
git diff --stat <resolved-scope>
git diff --name-status <resolved-scope>
```

Use `<base>..HEAD` for resolved current-branch scopes, or the user's explicit non-`HEAD` range when the analysis scope is arbitrary. Scoped diff inspection supports advice-only or planning-only analysis. It does not authorize mutation; only an approved executable plan may proceed to the rewrite gates.

## Base And Range Resolution Gate

For executable current-branch rewrites:

1. Resolve `BASE_SHA`.
2. Resolve `ORIGINAL_HEAD`.
3. Show both values in the plan.
4. State that the executable scope is current branch `BASE_SHA..ORIGINAL_HEAD`.

For arbitrary or non-`HEAD` ranges:

- Treat the range as analysis scope.
- Do not reset, rebase, cherry-pick, or create backup branches from the range alone.
- Ask for or propose a concrete execution strategy, such as rewriting the current branch from an approved `BASE_SHA` or creating a temporary branch at the range end.
- Require approval for that strategy before mutation.

## Diff-First Planning Gate

Build a diff inventory before proposing commits:

- changed files and meaningful change areas
- tests, docs, config, lockfiles, binary files, mode changes, and submodules
- dependency relationships between change areas
- broad formatting-only or mechanical churn
- likely existing verification commands for each area
- ambiguous or risky areas needing user attention

Plan by diff coverage, not by a fixed commit count. A complete plan accounts for every meaningful change area, assigns each area to a proposed commit, explains dependency order, and names checks that should keep each commit green.

Prefer fewer focused commits over maximum atomicity. Pair tests with the behavior they prove. Assign docs and config intentionally. Isolate unrelated formatting or mechanical churn unless there is a clear reason not to.

Use read-only subagents for large diff analysis when available, but keep all Git mutation in the main agent.

## Plan Format

Offer 2-3 options only when there is real ambiguity. For straightforward diffs, give one recommended plan.

Include:

| Order | Proposed commit | Purpose | Included changes | Depends on | Checks |
| --- | --- | --- | --- | --- | --- |
| 1 | `feat(api): add retry policy` | Add retry behavior | API client, retry config, tests | none | API tests |
| 2 | `docs(api): document retry behavior` | Document public behavior | API docs | 1 | docs/lint if available |
| 3 | `chore(legacy): isolate formatting churn` | Keep formatting reviewable | unrelated formatting-only files | none | formatter/lint |

For multi-area diffs, include coverage:

| Change area | Files | Proposed commit | Reason |
| --- | --- | --- | --- |
| Retry behavior | `src/api/client.py`, tests, config | 1 | Behavior, config, and tests belong together |
| API docs | `docs/api.md` | 2 | Documents the new behavior |
| Formatting churn | `src/legacy_helper.py` | 3 | Broad unrelated mechanical change |

End the plan with:

- recommended option and why
- `BASE_SHA` and `ORIGINAL_HEAD` when executable
- backup branch name to create
- rewrite mechanism to use after approval
- reconstruction mode: path-based, hunk-based, mixed, or stop-and-ask
- verification strategy
- final diff equivalence check
- explicit approval request before rewrite

## Rewrite Safety Gate

After approval, run pre-mutation revalidation before any Git mutation:

1. Re-run clean status, active-operation checks, current branch, `HEAD`, and base resolution.
2. Stop and re-plan if any value differs from the approved plan.
3. Record `ORIGINAL_HEAD`.
4. Create a unique artifact directory and capture starting diff evidence using `references/rewrite-mechanics.md`.
5. Create a unique local backup branch at `ORIGINAL_HEAD`.
6. Verify the backup branch resolves to `ORIGINAL_HEAD`.
7. If backup creation or verification fails, stop before reset or rewrite.
8. Follow `references/rewrite-mechanics.md` for root-anchored reconstruction, pre-slicing sanity checks, staged-content audits, and final equivalence.

Do not improvise reconstruction for non-text or metadata changes. If the approved reconstruction mode no longer fits the actual diff, stop and re-plan.

Before each commit, audit staged content:

```sh
git diff --cached --stat
git diff --cached --name-status
git diff --cached --raw
git diff --cached
```

For binary, mode, rename, submodule, or large generated changes, verify staged metadata with name-status, stat, or raw diff output rather than relying only on text hunks.

## Verification And Exception Gate

Use existing repo tooling only. Infer relevant commands from local guidance, package scripts, build/test/lint/typecheck commands, and CI configuration.

For each commit:

- run the narrowest meaningful existing checks for the touched scope
- broaden checks for shared contracts, build config, dependency metadata, or cross-cutting code
- if a small obvious fix is needed because of the split or order, make it in the relevant commit and rerun checks
- stop on non-obvious failures

On non-obvious verification failure:

1. Stop.
2. Summarize the failing command and affected commit or scope.
3. Explain that the branch cannot honestly be reported as fully green.
4. Ask whether to fix, reorder, squash, split, or explicitly accept a non-green exception.
5. Do not continue producing a partially green history without approval.

If the user approves a non-green exception because a later commit makes it green, report that exception clearly in the final summary.

## Conflict And Push Boundaries

If conflicts or interrupted Git operations appear during the rewrite, stop and use the repo's conflict-resolution workflow if available. If no such workflow exists, explain the current state, name the backup branch, and ask how to proceed.

Keep backup branches local unless the user explicitly asks otherwise.

Never push automatically. If the user later asks to update a remote branch, require separate approval and prefer `git push --force-with-lease`. Report whether the branch is published or upstream-tracked, and do not imply the lease is a complete remote-safety guarantee.

## Final Equivalence And Summary

Before calling the work complete, compare the final branch diff against the captured starting diff using the same `BASE_SHA` and the final artifacts in `ARTIFACT_DIR`. When no intentional final-diff deltas exist, verify tree-level equivalence against `ORIGINAL_HEAD`. Do not rely only on `--stat` or patch text.

Unexplained final-equivalence mismatch is a hard stop: do not push or claim completion. Report the backup branch, artifact directory, and mismatch summary, then ask whether to fix reconstruction, accept an intentional delta, or restore from backup.

Report every intentional final-diff delta caused by verification fixes.

Use this summary shape:

```markdown
Organized the branch into 3 linear commits from `BASE_SHA` `<sha>`.

Backup branch: `backup/my-branch-before-organize-20260525-143210`

Final commits:

1. `<sha>` `feat(api): add retry policy`
   - Covers retry behavior, config, and tests.
   - Verification: `pytest src/api/client_test.py`
2. `<sha>` `docs(api): document retry behavior`
   - Covers API documentation.
   - Verification: docs/lint if available
3. `<sha>` `chore(legacy): isolate formatting churn`
   - Covers unrelated formatting-only churn.
   - Verification: formatter/lint

Final verification:

- `<command>`

Final diff equivalence:

- Compared against the starting diff using `BASE_SHA` `<sha>`.
- Artifact directory: `<path>`
- Verification fixes: none.

Push:

- No remote push was performed.
- If you want to update the remote branch, use a separately approved `git push --force-with-lease`.
```

If verification fixes changed the final diff, replace `Verification fixes: none` with the changed files and rationale.
