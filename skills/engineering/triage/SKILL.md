---
name: triage
description: "Write $ISSUES_DIR/<id>/brief.md for one Redmine ticket: category, what to build, acceptance."
disable-model-invocation: true
---

# Triage

Write an agent-ready brief for one Redmine ticket. The brief is a local file. Redmine stays read-only.

## Issue folder

Resolve the Redmine id in this order: an id or URL in the argument, then the current branch matching `feature/<id>-`, `bugfix/<id>-`, or `hotfix/<id>-`, then ask. Do not guess.

The folder is `$ISSUES_DIR/<id>/` when `ISSUES_DIR` is set, otherwise `~/work/issues/<id>/`. Create the folder if it is missing.

If `ticket.md` is missing, fetch it with `python3 ~/.agents/skills/investigate-redmine-ticket/scripts/fetch_issue.py <id>`. That script writes `ticket.md`. Redmine stays read-only apart from that fetch.

## Process

1. Read `ticket.md` in full: description, journals, relations, parent. Done when you can name the request in one sentence.

2. Write `$ISSUES_DIR/<id>/brief.md` with the template below. Category is `bug` or `enhancement`. If `brief.md` already exists, ask before overwriting. Done when the file is written and you have reported its path.

<brief-template>

# Brief

**Category:** bug or enhancement

## What to build

The end-to-end behaviour, from the user's perspective.

## Acceptance

- [ ] Criterion 1
- [ ] Criterion 2

</brief-template>

Tickets you wrote with `to-tickets` are already agent-ready. This skill is for a Redmine ticket that arrived from someone else.
