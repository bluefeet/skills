# Rewrite Mechanics

Use this reference only after the user has approved an executable current-branch rewrite plan. Keep the approved `BASE_SHA`, `ORIGINAL_HEAD`, backup branch, and final diff inventory visible while following these steps.

## Unique Artifacts

Create one unique artifact directory per rewrite run:

```sh
ARTIFACT_DIR="$(mktemp -d "${TMPDIR:-/tmp}/commit-organizer.XXXXXX")"
```

Capture starting evidence before reset or rewrite:

```sh
git diff --binary --full-index "$BASE_SHA" "$ORIGINAL_HEAD" > "$ARTIFACT_DIR/start.patch"
git diff --name-status "$BASE_SHA" "$ORIGINAL_HEAD" > "$ARTIFACT_DIR/start.name-status"
git diff --stat "$BASE_SHA" "$ORIGINAL_HEAD" > "$ARTIFACT_DIR/start.stat"
git diff --raw "$BASE_SHA" "$ORIGINAL_HEAD" > "$ARTIFACT_DIR/start.raw"
git diff --submodule=short "$BASE_SHA" "$ORIGINAL_HEAD" > "$ARTIFACT_DIR/start.submodule"
```

Report `ARTIFACT_DIR` in the final summary and in any stop-for-help message.

## Root-Anchored Reconstruction

Run reconstruction from the repository root. If `cd` fails, stop before mutation.

```sh
REPO_ROOT="$(git rev-parse --show-toplevel)"
cd "$REPO_ROOT"

git reset --hard "$BASE_SHA"
git restore --source "$ORIGINAL_HEAD" --worktree --staged -- :/
```

Before emptying the index for slicing, verify the materialized tree matches the captured starting diff:

```sh
git diff --cached --binary --full-index "$BASE_SHA" -- :/ > "$ARTIFACT_DIR/materialized.patch"
git diff --cached --name-status "$BASE_SHA" -- :/ > "$ARTIFACT_DIR/materialized.name-status"
git diff --cached --stat "$BASE_SHA" -- :/ > "$ARTIFACT_DIR/materialized.stat"
git diff --cached --raw "$BASE_SHA" -- :/ > "$ARTIFACT_DIR/materialized.raw"
git diff --cached --submodule=short "$BASE_SHA" -- :/ > "$ARTIFACT_DIR/materialized.submodule"

cmp "$ARTIFACT_DIR/start.name-status" "$ARTIFACT_DIR/materialized.name-status"
cmp "$ARTIFACT_DIR/start.raw" "$ARTIFACT_DIR/materialized.raw"
cmp "$ARTIFACT_DIR/start.submodule" "$ARTIFACT_DIR/materialized.submodule"
git diff --quiet "$ORIGINAL_HEAD" -- :/
```

If any command fails, stop before slicing commits. Report the backup branch, artifact directory, and failing comparison.

Prepare the worktree for deliberate staging:

```sh
git reset "$BASE_SHA"
git diff --cached --quiet
git diff --quiet "$ORIGINAL_HEAD" -- :/
```

The final diff is now available as unstaged worktree changes.

## Staging Planned Commits

- Prefer path-level staging when files map cleanly to one commit.
- Use `git add -p` only when one file contains separable changes.
- Stage deletions with `git rm <path>` or `git add -u <path>`, then verify with `git diff --cached --name-status`.
- Stage rename pairs together. If Git reports delete/add instead of a rename, report that representation instead of hiding it.
- Stage binary files by path; verify with `git diff --cached --name-status`, `git diff --cached --stat`, and `git diff --cached --summary`.
- Verify mode changes with `git diff --cached --summary` or `git diff --cached --raw`.
- Stage generated files and lockfiles with the source change they support unless the approved plan intentionally isolates them.
- For submodules, stage the gitlink intentionally and verify pointer movement with `git diff --cached --submodule=short`. Do not edit inside submodules unless that scope was explicitly approved.

Before each commit, audit staged content with:

```sh
git diff --cached --stat
git diff --cached --name-status
git diff --cached --raw
git diff --cached
```

If a planned commit cannot be reconstructed without mixing unrelated changes, stop and revise the plan instead of improvising.

## Final Equivalence

Capture final evidence with the same `BASE_SHA`:

```sh
git diff --binary --full-index "$BASE_SHA" HEAD > "$ARTIFACT_DIR/final.patch"
git diff --name-status "$BASE_SHA" HEAD > "$ARTIFACT_DIR/final.name-status"
git diff --stat "$BASE_SHA" HEAD > "$ARTIFACT_DIR/final.stat"
git diff --raw "$BASE_SHA" HEAD > "$ARTIFACT_DIR/final.raw"
git diff --submodule=short "$BASE_SHA" HEAD > "$ARTIFACT_DIR/final.submodule"
```

When no intentional verification fixes changed the final diff, verify tree equivalence and metadata:

```sh
git diff --quiet "$ORIGINAL_HEAD" HEAD
cmp "$ARTIFACT_DIR/start.name-status" "$ARTIFACT_DIR/final.name-status"
cmp "$ARTIFACT_DIR/start.raw" "$ARTIFACT_DIR/final.raw"
cmp "$ARTIFACT_DIR/start.submodule" "$ARTIFACT_DIR/final.submodule"
```

Patch comparison is useful when it passes, but do not rely on patch byte equality as the only equivalence gate.

If intentional verification fixes changed the final diff, list each changed file and rationale in the final summary. If unexplained drift remains, stop. Do not push or claim completion. Report the backup branch, artifact directory, and mismatch summary, then ask whether to fix reconstruction, accept an intentional delta, or restore from backup.
