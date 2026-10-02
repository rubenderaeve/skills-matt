---
name: implement-spec
description: "Implement the spec and slice files in $ISSUES_DIR/<id>/ on the current branch, frontier order."
disable-model-invocation: true
---

Implement the spec and its slice files on the current branch. The slices are a task graph. Work the frontier: a slice whose blockers are done.

## Issue folder

Resolve the Redmine id in this order: an id or URL in the argument, then the current branch matching `feature/<id>-`, `bugfix/<id>-`, or `hotfix/<id>-`, then ask. Do not guess.

The folder is `$ISSUES_DIR/<id>/` when `ISSUES_DIR` is set, otherwise `~/work/issues/<id>/`. Create the folder if it is missing.

If `ticket.md` is missing, fetch it with `python3 ~/.agents/skills/investigate-redmine-ticket/scripts/fetch_issue.py <id>`. That script writes `ticket.md`. Redmine stays read-only apart from that fetch.

## Steps

1. Read `spec.md` and every `<NN>-<slug>.md` slice in the issue folder. A slice is done when its **Status** is `done`. The frontier is every slice whose **Status** is `ready` and whose **Blocked by** entries are all done, or "None". Done when you can name the frontier.

2. Stay on the current branch. Implement the frontier slices in number order, one at a time. For each slice, call the Skill tool with `tdd` at the seams named in the spec. When the slice lands, set its **Status** to `done` and check its acceptance boxes. Done when no ready slice remains.

3. Call the Skill tool with `code-review` on the current branch. Fix the issues it raises. Done when you have reported the branch and the slice files. Do not open a pull request. Do not create, edit, comment, assign, or close a Redmine issue.
