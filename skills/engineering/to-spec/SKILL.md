---
name: to-spec
description: "Turn the current conversation into a spec at $ISSUES_DIR/<id>/spec.md. No interview, just synthesis of what you've already discussed."
disable-model-invocation: true
---

This skill takes the current conversation context and codebase understanding and produces a spec. Do not interview the user. Synthesize what you already know.

## Issue folder

Resolve the Redmine id in this order: an id or URL in the argument, then the current branch matching `feature/<id>-`, `bugfix/<id>-`, or `hotfix/<id>-`, then ask. Do not guess.

The folder is `$ISSUES_DIR/<id>/` when `ISSUES_DIR` is set, otherwise `~/work/issues/<id>/`. Create the folder if it is missing.

If `ticket.md` is missing, fetch it with `python3 ~/.agents/skills/investigate-redmine-ticket/scripts/fetch_issue.py <id>`. That script writes `ticket.md`. Redmine stays read-only apart from that fetch.

## Process

1. Explore the repo to understand the current state of the codebase, if you haven't already. Use the project's domain glossary vocabulary, and respect any ADRs in the area you're touching.

2. Sketch the seams you will test the feature at. Prefer an existing seam to a new one, and take the highest seam you can. The ideal number is one. Check with the user that these seams match their expectations. Done when the user has confirmed the seams, or corrected them.

3. Write the spec with the template below to `$ISSUES_DIR/<id>/spec.md`. If that file already exists, ask before overwriting. Done when the file is written and you have reported its path.

<spec-template>

## Problem Statement

The problem that the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## User Stories

A LONG, numbered list of user stories. Each user story should be in the format of:

1. As an <actor>, I want a <feature>, so that <benefit>

## Implementation Decisions

A list of implementation decisions that were made. This can include the modules that will be built or modified, the interfaces of those modules, technical clarifications, architectural decisions, schema changes, API contracts, and specific interactions.

Do not include specific file paths or code snippets. They go stale fast.

Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it within the relevant decision and note that it came from a prototype. Trim to the decision-rich parts.

## Testing Decisions

A list of testing decisions that were made. Include what makes a good test (external behavior, not implementation details), which modules will be tested, and prior art for the tests.

## Out of Scope

A description of the things that are out of scope for this spec.

## Further Notes

Any further notes about the feature.

</spec-template>
