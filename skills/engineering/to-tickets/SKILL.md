---
name: to-tickets
description: "Break a plan, spec, or the current conversation into tracer-bullet tickets in $ISSUES_DIR/<id>/, each declaring its blocking edges as text."
disable-model-invocation: true
---

# To Tickets

Break a plan, spec, or conversation into **tickets**: tracer-bullet vertical slices, each declaring the tickets that **block** it.

## Issue folder

Resolve the Redmine id in this order: an id or URL in the argument, then the current branch matching `feature/<id>-`, `bugfix/<id>-`, or `hotfix/<id>-`, then ask. Do not guess.

The folder is `$ISSUES_DIR/<id>/` when `ISSUES_DIR` is set, otherwise `~/work/issues/<id>/`. Create the folder if it is missing.

If `ticket.md` is missing, fetch it with `python3 ~/.agents/skills/investigate-redmine-ticket/scripts/fetch_issue.py <id>`. That script writes `ticket.md`. Redmine stays read-only apart from that fetch.

## Process

### 1. Gather context

Work from whatever is already in the conversation. If the user passes a spec path, an id, or a URL, read that and `spec.md` in the issue folder when it exists.

### 2. Explore the codebase

If you have not already explored the codebase, do so. Ticket titles use the project's domain glossary, and respect ADRs in the area you're touching.

Look for prefactoring that makes the implementation easier. Make the change easy, then make the easy change.

### 3. Draft vertical slices

<vertical-slice-rules>

- Each slice cuts a narrow but complete path through every layer it needs: vertical, not one layer.
- A completed slice is demoable or verifiable on its own.
- Each slice fits in a single fresh context window.
- Prefactoring comes first.

</vertical-slice-rules>

Give each ticket its **blocking edges**: the other tickets that must complete before it can start. A ticket with no blockers can start immediately.

**Wide refactors are the exception.** A wide refactor is one mechanical change whose blast radius fans across the codebase, so no vertical slice can land green. Sequence it as expand, then migrate, then contract. First add the new form beside the old. Then migrate call sites in batches sized by blast radius, each batch its own ticket blocked by the expand. Then delete the old form in a ticket blocked by every migrate batch. When a batch cannot stay green alone, the batches share an integration branch that blocks a final integrate-and-verify ticket. Green is promised only there.

### 4. Quiz the user

Present the breakdown as a numbered list. For each ticket show:

- **Title**
- **Blocked by**
- **What it delivers**: the end-to-end behaviour this ticket makes work

Ask whether the granularity is right, whether the blocking edges are real, and whether any tickets should merge or split. Iterate until the user approves. Done when the user approves the breakdown.

### 5. Write the tickets

Write one file per approved ticket at `$ISSUES_DIR/<id>/<NN>-<slug>.md`, numbered from `01` in dependency order (blockers first). Slug is kebab-case from the title. If that file already exists, ask before overwriting.

Do not create, edit, comment, assign, or close a Redmine issue. Do not modify a parent issue.

Done when every approved ticket is a file and you have reported the paths.

<ticket-template>

# <NN>: <Ticket title>

**What to build:** the end-to-end behaviour this ticket makes work, from the user's perspective, not a layer-by-layer implementation list.

**Blocked by:** the numbers and titles of the tickets that gate this one, or "None (can start immediately)".

**Status:** ready

- [ ] Acceptance criterion 1
- [ ] Acceptance criterion 2

</ticket-template>

Avoid specific file paths or code snippets. They go stale fast. Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can, inline it and note that it came from a prototype. Trim to the decision-rich parts.
