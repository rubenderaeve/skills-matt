---
name: handoff
description: Compact the current conversation into a handoff file under $ISSUES_DIR/<id>/.
argument-hint: "What will the next session be used for?"
disable-model-invocation: true
---

Write a handoff document summarising the current conversation so a fresh agent can continue the work.

## Where to save

Resolve the Redmine id in this order: an id or URL in the argument, then the current branch matching `feature/<id>-`, `bugfix/<id>-`, or `hotfix/<id>-`, then ask. Do not guess.

The folder is `$ISSUES_DIR/<id>/` when `ISSUES_DIR` is set, otherwise `~/work/issues/<id>/`. Create the folder if it is missing.

Filename: `handoff-<slug>.md`, where the slug is kebab-case from the argument that says what the next session is for. If that argument is missing, the filename is `handoff.md`. If the file already exists, ask before overwriting.

The file lives in the issue folder, not in the product repo and not in the OS temp directory. Report the path when you are done. A colleague gets that path.

## What to include

Include a "suggested skills" section naming which skills the next agent should call the Skill tool for.

Do not duplicate content already captured in other artifacts (specs, plans, ADRs, issues, commits, diffs). Reference them by path or URL instead.

Redact any sensitive information, such as API keys, passwords, or personally identifiable information.

If the user passed arguments, treat the non-id part as what the next session will focus on and tailor the doc to that.
