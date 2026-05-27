---
name: git-conflict
description: Helps resolve or recover from Git conflicts or interrupted operations, including merge, rebase, cherry-pick, revert, stash apply/pop, autostash, marker-only cleanup, or safe branch rebase/update previews. Does not apply to generic Git advice, ordinary commits, commit-message drafting, or history organization.
---

# Git Conflict

## Activation Boundary

Use this skill for:

- resolving or recovering from active merge, rebase, cherry-pick, revert, stash apply/pop, or autostash conflicts
- diagnosing uncertain conflict or interrupted-operation state
- cleaning conflict markers when Git is not tracking unmerged paths
- safely previewing and running a branch rebase/update at the user's request

Do not use this skill for:

- generic Git explanation
- ordinary commit creation
- commit-message drafting
- branch history cleanup or commit organization
- broad release, PR, or repository maintenance tasks that do not involve conflict recovery or branch update safety

Non-trigger examples:

- "Clean up, squash, or reorder my commits for PR review" should route to commit organization.
- "Write a commit message for these staged changes" is out of scope.
- "Explain what `git rebase` does" is generic Git advice unless the user is actively updating or recovering this branch.

Always detect what Git is doing before explaining sides or recommending mutation. The meaning of `ours`, `theirs`, stage numbers, and continuation commands changes by operation.

## State Dispatch

| Detected state/request | Dispatch path |
| --- | --- |
| Active merge, rebase, cherry-pick, revert, stash apply/pop, or autostash context with unmerged paths | Use Conflict Resolution. Inspect operation metadata, then inspect per-path stages before recommending edits, staging, continue, skip, abort, or backout. |
| Active rebase/merge/cherry-pick/revert metadata with no unmerged paths | Inspect status, sequencer state, staged and unstaged changes, then propose the correct continue, commit, skip, abort, or explanation path without normal stage blob inspection. |
| No active operation, but the user requested branch rebase/update | Use Rebase preview. Resolve or ask for the exact target, preview commits and worktree risk, then ask before running any state-changing command. |
| Conflict markers exist, but Git has no unmerged paths and no active sequencer | Use Marker-Only Cleanup. Explain that marker editing is ordinary file cleanup, not Git conflict continuation. |
| No relevant conflict, interrupted operation, marker cleanup, or branch update request | Explain the detected state and ask what the user wants next. |

## Workflow Checklist

For complex work, track progress with this checklist. It does not need to be shown for tiny tasks.

- [ ] Detect Git state
- [ ] Choose the dispatch path
- [ ] Inspect sides, stages, and relevant context
- [ ] Explain the source of the conflict or rebase/update plan
- [ ] Ask for batched confirmation before mutation
- [ ] Apply the approved action
- [ ] Verify final Git state

## Confirmation Gate

Read-only inspection commands do not need confirmation. Ask before every action that mutates repository state, contacts remotes, rewrites history, or discards recoverable data:

- editing conflicted files
- staging resolutions
- choosing a side with `git restore`, `git checkout --ours`, or `git checkout --theirs`
- continuing, skipping, aborting, or completing an operation
- creating a merge commit or otherwise completing a merge
- creating, applying, or dropping a stash
- running rebase, reset, clean, or other state-changing Git commands
- fetching or refreshing remotes
- pushing

A confirmation gate is satisfied only by explicit user approval. Warnings, conditional phrasing, or "if you want" language are not enough when the next step would mutate repository state.

Before showing any command sequence that edits files, stages paths, continues, skips, aborts, rebases, fetches, pushes, stashes, drops a stash, resets, cleans, or otherwise changes state, make sure both conditions are true:

1. the necessary read-only evidence for that operation is present or has been gathered
2. the user has chosen or approved the action

If either condition is missing, give only read-only inspection commands, explain the likely options, and ask for the missing evidence or choice. If you show mutating commands early for illustration, label them as post-confirmation examples and do not present them as the next runnable sequence.

Before approval, separate:

1. detected state
2. recommended path
3. affected files or refs
4. direct permission or choice question

If showing mutating commands before approval, label them as post-confirmation examples. Do not present them as the next runnable sequence.

Examples:

- Rebase conflict: "I recommend keeping the replayed file, then staging and continuing. Do you want me to apply that resolution?"
- Untracked overlap: "These paths may collide. Which handling option should I use: move them aside, stash including untracked/ignored files, delete generated files, or stop?"
- Marker-only cleanup: "This is ordinary file cleanup. Do you want me to edit `README.md` to keep the intended text and remove the marker block?"
- Stash backout: "This reset would discard the conflicted `server.py` state and the `package.json` change. Do you want me to do that?"

Batched confirmation is allowed when the recommendation is clear and required evidence is already available, such as "edit this file, stage it, then run `git rebase --continue`." Reconfirm if state changes, a new conflict appears, required evidence is missing, or the next action differs from the approved batch.

Before `git rebase --continue`, `git merge --continue`, `git cherry-pick --continue`, `git revert --continue`, creating a merge commit, dropping a stash, or otherwise completing the operation, run pre-continuation verification:

```sh
git status --short --branch
git diff --name-only --diff-filter=U -- <intended-resolved-paths>
git ls-files -u -- <intended-resolved-paths>
rg -n '^(<<<<<<<|\|\|\|\|\|\|\||=======|>>>>>>>)( |$)' <intended-resolved-paths>
```

Proceed only if intended resolved paths have no unmerged index entries and no remaining conflict markers, or explain what still needs work.

Skip commands are a separate gate. Do not require the resolved-path clean check when a skip intentionally drops the conflicted patch. Instead, confirm the active operation, the exact commit or patch being skipped, the consequence of dropping that patch, and the supported skip command for the detected operation.

## State Detection

Start with labeled, read-only checks. Do not infer the operation from the user's wording, and do not present expected-negative metadata checks as failures.

```sh
echo "== status and branch =="
git status --short --branch
git status

echo "== current branch =="
git branch --show-current

echo "== repository root =="
git rev-parse --show-toplevel

echo "== remotes =="
git remote -v

echo "== unmerged files =="
git diff --name-only --diff-filter=U

echo "== unmerged index stages =="
git ls-files -u

echo "== active operation metadata =="
if test -d "$(git rev-parse --git-path rebase-merge)" || test -d "$(git rev-parse --git-path rebase-apply)"; then echo "active rebase"; else echo "no active rebase metadata"; fi
if test -f "$(git rev-parse --git-path MERGE_HEAD)"; then echo "active merge"; else echo "no active merge"; fi
if test -f "$(git rev-parse --git-path CHERRY_PICK_HEAD)"; then echo "active cherry-pick"; else echo "no active cherry-pick"; fi
if test -f "$(git rev-parse --git-path REVERT_HEAD)"; then echo "active revert"; else echo "no active revert"; fi

echo "== stash list =="
git stash list

echo "== recent reflog =="
git reflog -n 8 --date=relative
```

Run a marker scan when the user reports conflict markers or conflict-like text and Git does not show an active unmerged state, or when marker-only cleanup is explicitly requested. Treat this as read-only inspection and avoid assuming generated, vendored, or intentionally documented examples need edits.

```sh
echo "== conflict marker scan =="
rg -n '^(<<<<<<<|\|\|\|\|\|\|\||=======|>>>>>>>)( |$)'
```

## Side Meanings

Attach the detected meaning whenever using `ours`, `theirs`, stage numbers, or side-selection commands.

| Operation | `ours` / stage 2 | `theirs` / stage 3 | Continuation |
| --- | --- | --- | --- |
| Rebase | target branch state | replayed commit from this branch | `git rebase --continue` |
| Merge | current branch before merge | branch being merged in | `git merge --continue` or merge commit |
| Cherry-pick | current branch | picked commit | `git cherry-pick --continue` |
| Revert | current branch | inverse patch Git is applying | `git revert --continue` |
| Stash apply/pop | current working tree/index state | stashed change being applied | no stash continuation command |

For unmerged files, stage 1 is the common base, stage 2 is `ours`, and stage 3 is `theirs`. Inspect the stages that actually exist for the path.

Example: during rebase, stage 2/`ours` is the target branch state and stage 3/`theirs` is the replayed commit.

Example: during stash apply/pop, there is no `git stash --continue`; resolve files, verify status, then drop the stash only after confirmation if it remains and is no longer needed.

## Conflict Resolution

Use this flow for merge, rebase, cherry-pick, revert, stash apply/pop, autostash conflicts, and active operations with unmerged paths.

1. Classify the active operation and unmerged paths with State Detection.
2. Inspect operation-specific context with only the command that matches the detected operation:

   ```sh
   git rebase --show-current-patch
   git show --stat --oneline MERGE_HEAD
   git show --stat --oneline CHERRY_PICK_HEAD
   git show --stat --oneline REVERT_HEAD
   git stash list
   ```

3. For each conflicted path, inspect the unmerged index before reading stage blobs:

   ```sh
   git ls-files -u -- <path>
   ```

   Inspect only stages that exist:

   ```sh
   git show :1:<path>
   git show :2:<path>
   git show :3:<path>
   git diff --ours -- <path>
   git diff --theirs -- <path>
   git log --oneline --decorate -- <path>
   ```

   Add/add conflicts may lack a base stage. Delete/modify, rename/delete, binary, submodule, and other nonstandard conflicts may have absent or unusable stage blobs. Explain the actual shape from `git ls-files -u -- <path>` instead of assuming stages 1/2/3 exist.

4. Read nearby implementation, tests, and relevant recent commits when they affect the recommendation.
5. Explain the operation that caused the conflict, what each side was preserving, whether the conflict is semantic or mechanical, and what tests or call sites matter.
6. Recommend a concrete resolution and ask at the Confirmation Gate before editing, staging, continuing, skipping, aborting, or completing.

After approved resolution for merge, rebase, cherry-pick, or revert, stage only resolved paths and use the continuation command for the detected operation. For stash apply/pop conflicts, verify status and drop the stash only after explicit confirmation if the entry remains and is no longer needed. If more conflicts appear, repeat state detection and conflict assessment.

Supported skip/abort commands include:

```sh
git rebase --skip
git rebase --abort
git merge --abort
git cherry-pick --skip
git cherry-pick --abort
git revert --skip
git revert --abort
```

## Stash Apply/Pop Backout

Stash apply/pop conflicts are not merge or rebase sequencer operations. There is no single safe abort command, and `git stash --continue` is not a real recovery path. Do not suggest `git stash apply --continue`, a universal `git stash --abort`, or any broad reset/clean/restore recipe without evidence and confirmation.

Preserve evidence first:

```sh
git status --short --branch
git diff --name-only --diff-filter=U
git ls-files -u
git diff
git diff --staged
git stash list
```

For any undo, backout, or "this is messy" recovery request, this evidence is required before recommending a backout path. If the prompt or tool output does not include the unstaged diff, staged diff, unmerged paths or stages, and stash list, ask for the missing read-only checks first. Do not provide commands that restore, stage, reset, clean, drop a stash, or otherwise mutate state until the evidence is present and the user has chosen the path.

Distinguish the user's goal:

- Resolve by taking the current working tree/index side for specific conflicted paths: inspect the path stages, explain that this only resolves those paths, ask before editing/restoring/staging, then verify status.
- Resolve by taking stashed changes for specific conflicted paths: inspect the path stages, explain the stashed side, ask before editing/restoring/staging, then verify status.
- Broader backout of the applied stash, or a narrow backout when unrelated local work is present: inspect status, unmerged stages, unstaged diffs, staged diffs, and the exact stash ref first. Applied stash changes can touch tracked, untracked, staged, unconflicted, and conflicted paths differently, so avoid universal restore recipes.

Avoid `reset`, `clean`, or broad `restore` commands unless the user explicitly confirms and you have recoverability evidence: current status, stash ref still available or another recovery point, and a clear list of paths that will be discarded.

After successful resolution, verify whether the stash entry remains. `git stash apply` keeps entries, and conflicting `git stash pop` normally keeps entries, but always check:

```sh
git status --short --branch
git stash list
```

Drop a stash entry only after explicit confirmation that it is no longer needed.

## Rebase

Use this flow when no active operation exists and the user asks to update or rebase the current branch, or when completing a rebase after conflicts.

### Target Resolution And Preview

Resolve casual requests into concrete refs without guessing:

- Distinguish same-branch upstream sync from base-branch rebase. If the user says "update this branch" and metadata only proves same-branch upstream tracking, ask whether they mean syncing with the upstream of this branch or rebasing onto a base branch such as `main`.
- For `rebase main`, inspect plausible local and remote refs, including `main`, `origin/main`, and `upstream/main`.
- If the user explicitly asks to update with `main` and only one plausible remote main is shown, proposing `origin/main` with an explicit assumption is acceptable.
- If local `main`, `origin/main`, `upstream/main`, configured upstream, or PR-base information conflict or are uninspected, inspect read-only refs or ask before choosing. This preferred remote-tracking behavior is a conversational safety convention, not raw Git semantics.
- Treat clues such as "I usually open PRs against the company repo" as evidence for a likely recommendation, not as target approval. You may say which target looks likely and why, but keep ambiguous bases as choices until the user approves the exact ref. Do not present a runnable rebase sequence while the target is still ambiguous.
- `git fetch` or any remote refresh contacts a remote and needs confirmation unless the user already approved it.
- A preview based on existing local remote-tracking refs should say those refs may be stale.
- If the current branch has no configured upstream or merge remote target, or HEAD is detached, ask before proceeding.

Useful read-only checks:

```sh
git branch --show-current
git rev-parse --abbrev-ref --symbolic-full-name @{u}
git config --get branch.<current-branch>.remote
git config --get branch.<current-branch>.merge
git show-ref --verify refs/remotes/origin/main
git show-ref --verify refs/remotes/upstream/main
git show-ref --verify refs/heads/main
```

Before asking to run a rebase, report:

- exact target ref
- commits from this branch that would be replayed: `git log --oneline <target>..HEAD`
- incoming commits the branch will incorporate: `git log --oneline HEAD..<target>`
- changed paths from the target and replayed ranges
- working-tree and untracked/ignored overlap risk
- exact proposed command, using `git -c merge.conflictstyle=diff3 rebase ...`
- whether local remote-tracking refs were refreshed or the preview is based on existing local refs

Interpret reachability explicitly rather than treating every empty-looking preview as a no-op:

```sh
git merge-base --is-ancestor HEAD <target>
git merge-base --is-ancestor <target> HEAD
```

If `<target>` is an ancestor of `HEAD`, this branch already contains the target. If `HEAD` is an ancestor of `<target>`, the update may be a fast-forward or no-replay branch move, not a conflict-producing replay; still preview and ask before moving the branch. If both are true, `HEAD` and `<target>` point at the same commit.

### Worktree And Overlap Safety

Classify working-tree state before rebase:

```sh
git status --porcelain=v1
git ls-files --others --exclude-standard
```

If tracked changes exist, `--autostash` may be appropriate, but `--autostash` does not save untracked files. Overlapping untracked paths may block the rebase or remain unmanaged.

After resolving the target, compare untracked paths and visible or likely ignored paths against paths changed by incoming target commits and replayed branch commits:

```sh
git diff --name-only HEAD...<target>
git diff --name-only <target>...HEAD
git status --ignored --short
```

Treat exact path collisions and parent/child directory collisions conservatively. If overlap is possible, list handling choices and stop before any stash, move, delete, or rebase recipe. Ask which handling strategy the user wants. Useful options include stashing with `--include-untracked`, moving files aside, committing intentional work, deleting generated files, or proceeding only if the user accepts the risk. Only provide or run the exact commands after the user chooses, or label them clearly as post-confirmation examples.

### Running And Completion

At the Confirmation Gate, summarize the target ref, exact command, expected replay count, notable incoming changes, tracked-change autostash behavior, untracked/ignored safety result, and whether refs were refreshed.

Approved commands should use diff3 conflict markers without changing the user's config:

```sh
git -c merge.conflictstyle=diff3 rebase <target>
git -c merge.conflictstyle=diff3 rebase --autostash <target>
```

After the command finishes or stops, inspect:

```sh
git status --short --branch
git status
git branch --show-current
git rev-parse --short HEAD
git rev-parse --abbrev-ref --symbolic-full-name @{u}
git stash list
```

If conflicts appear, return to Conflict Resolution. After rebase and any stash or autostash application complete, verify current branch, `HEAD`, upstream status, working tree status, and remaining stash entries before discussing push or summarizing completion.

Treat push as a separate confirmation gate after rebase completion. Before any approved push, report current branch, upstream remote/ref, and exact push command. Ask if upstream is absent or ambiguous.

Use only `git push --force-with-lease <remote> <current-branch>:<upstream-ref>` for an approved rewritten-branch push, preferably with explicit remote/ref when ambiguity is possible. If the lease fails, explain that the remote moved; offer to refresh, re-preview, and rebase again or ask how to proceed. Never suggest plain `git push --force`.

## Marker-Only Cleanup

Use this path when a marker scan finds conflict markers but Git has no unmerged paths and no active sequencer metadata, or when marker-only cleanup is explicitly requested.

1. Scan for markers when triggered:

   ```sh
   rg -n '^(<<<<<<<|\|\|\|\|\|\|\||=======|>>>>>>>)( |$)' <paths-or-repo>
   ```

2. Verify no unmerged paths and no active sequencer operation:

   ```sh
   git status --short --branch
   git diff --name-only --diff-filter=U
   git ls-files -u
   ```

   Also check active operation metadata from State Detection.

3. Explain that no continuation command applies. This is ordinary file editing because Git is not tracking unmerged paths for these files.
4. Ask before editing marker-only files.
5. After editing, verify intended files are marker-free:

   ```sh
   rg -n '^(<<<<<<<|\|\|\|\|\|\|\||=======|>>>>>>>)( |$)' <intended-files>
   ```

## Gotchas

- `ours`/`theirs` flips in rebase: `ours` is the target branch state and `theirs` is the replayed commit.
- Autostash does not protect untracked files; check risky untracked and ignored overlap before rebase.
- `--force-with-lease` is not a guarantee against every remote race; if it fails, the remote moved and needs a fresh preview.
- Stash apply/pop conflict behavior differs from merge/rebase continuation; there is no stash continue command.
- Marker-only cleanup is not a Git sequencer operation, so no continuation command applies.
