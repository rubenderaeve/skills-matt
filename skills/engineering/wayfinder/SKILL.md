---
name: wayfinder
description: "Plan a huge chunk of work as decision files under $ISSUES_DIR/<id>/, and resolve them one at a time until the way to the destination is clear."
disable-model-invocation: true
---

A loose idea has arrived, too big for one agent session. The way from here to the **destination** is not visible yet. Wayfinding charts that way as files in one Redmine issue folder, then resolves **decision tickets** one at a time. A decision ticket's resolution is a decision, not a slice of a build.

Name the destination first. It might be a spec to hand off, a decision to lock, or a change made in place. The map produces decisions, not deliverables. When the way is clear, stop and hand off. Call the Skill tool with `to-spec` only if the user asks to continue. This skill does not build the destination.

## Issue folder

Resolve the Redmine id in this order: an id or URL in the argument, then the current branch matching `feature/<id>-`, `bugfix/<id>-`, or `hotfix/<id>-`, then ask. Do not guess. There is no second folder for an effort that has no id yet.

The folder is `$ISSUES_DIR/<id>/` when `ISSUES_DIR` is set, otherwise `~/work/issues/<id>/`. Create the folder if it is missing.

If `ticket.md` is missing, fetch it with `python3 ~/.agents/skills/investigate-redmine-ticket/scripts/fetch_issue.py <id>`. That script writes `ticket.md`. Redmine stays read-only apart from that fetch.

Refer to each decision by its title. The number rides inside the title, never instead of it.

## Files

The map is `$ISSUES_DIR/<id>/wayfinder.md`. If it already exists, continue it. Decisions are `$ISSUES_DIR/<id>/decisions/<NN>-<slug>.md`. If a decision file already exists, ask before overwriting.

<map-template>

## Destination

<what reaching the end of this map looks like. One or two lines.>

## Notes

<domain, skills every session should consult, standing preferences>

## Decisions so far

- [<closed ticket title>](decisions/<NN>-<slug>.md): <one-line gist>

## Not yet specified

<in-scope fog you cannot ticket yet>

## Out of scope

<work ruled beyond the destination>

</map-template>

<decision-template>

# <NN>: <title>

**Type:** research, prototype, grilling, or task

**Status:** open

**Blocked by:** numbers and titles, or "None (can start immediately)"

## Question

<the decision this ticket resolves>

## Answer

<filled in when Status becomes closed>

</decision-template>

A decision is unblocked when every ticket blocking it has **Status** `closed`. The frontier is the open, unblocked, unclaimed decisions (`Status: open`). Claiming sets **Status** to `claimed` before any work.

The map is an index. A decision lives in its file. The map gists it and links it.

## Ticket types

- **research**: an external fact. Call the Skill tool with `research`. Pass the Redmine id so the findings land in this issue folder. Link that file from **Answer**.
- **prototype**: a cheap artifact to react to. Call the Skill tool with `prototype`. Link the artifact from **Answer**.
- **grilling**: conversation with the user. Call the Skill tool twice, for `grilling` and `domain-modeling`. The agent does not answer the user's side.
- **task**: manual work that unblocks a decision. Drive it when you can. Otherwise hand the user a checklist. **Answer** records what was done.

## Fog and scope

Ticket a question when you can state it precisely now, even if it is blocked. Leave it under **Not yet specified** when you cannot phrase it yet. **Out of scope** is work beyond the destination. It does not graduate. If a live decision turns out to sit past the destination, set **Status** to `closed`, and add one line under **Out of scope** with the gist and why. Leave it out of **Decisions so far**.

## Chart the map

The user invokes with a loose idea.

1. Name the destination. Call the Skill tool twice, for `grilling` and `domain-modeling`. Done when the destination is one or two lines the user has accepted.
2. Grill again, breadth-first, for the open decisions and the steps takeable now. If there is no fog and the journey fits one session, stop and ask how to proceed. Do not create a map.
3. Write `wayfinder.md` with Destination and Notes filled in, Decisions so far empty, and the fog in **Not yet specified**.
4. Write the decisions you can specify now, then fill **Blocked by** in a second pass so numbers exist.
5. For each `research` decision, spin up a subagent that calls the Skill tool with `research`, passing this Redmine id. Do not resolve any other decision in this session.
6. Stop. Charting resolves nothing by hand.

## Work through the map

The user invokes with an id. A decision title is optional.

1. Read `wayfinder.md`, not every decision body.
2. If the user named a decision, use it. Otherwise take the first frontier decision in number order. Set its **Status** to `claimed` before any work.
3. Resolve it. Fetch a related decision file only when you need it. Call the Skill tool for whichever skills **Notes** names. If in doubt, call the Skill tool twice, for `grilling` and `domain-modeling`.
4. Write **Answer**, set **Status** to `closed`, and append one gist line under **Decisions so far**.
5. Add newly surfaced decisions. Move any fog you can now phrase out of **Not yet specified** and into a new file. If a decision sits beyond the destination, rule it out of scope as above.

Resolve at most one decision per session, except `research` decisions fired while charting.
