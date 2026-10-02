## What it does

`triage` writes an agent-ready brief for one Redmine ticket. The brief is `$ISSUES_DIR/<id>/brief.md`: a category, what to build, and acceptance criteria.

It reads the ticket and writes the file. It does not list a queue, apply labels, comment, or close anything. Redmine stays read-only apart from fetching `ticket.md` when that file is missing.

It is only for a ticket you did not write. [Tickets](https://www.aihero.dev/ai-coding-dictionary/ticket) from [to-tickets](https://aihero.dev/skills-to-tickets) are already agent-ready, and running `triage` over them adds a second brief you do not need.

## When to reach for it

You invoke this by typing `/triage` with a Redmine id or URL. The [agent](https://www.aihero.dev/ai-coding-dictionary/agent) won't reach for it on its own. With no id, it takes one from a `feature/<id>-`, `bugfix/<id>-`, or `hotfix/<id>-` branch, and asks if that is missing too.

| What you have | Where to go |
| --- | --- |
| One Redmine ticket someone else filed | `/triage` |
| A rough idea of your own, nothing written down | [grill-with-docs](https://aihero.dev/skills-grill-with-docs) |
| A settled conversation to turn into a [spec](https://www.aihero.dev/ai-coding-dictionary/spec) | [to-spec](https://aihero.dev/skills-to-spec) |
| A spec to split into agent-ready tickets | [to-tickets](https://aihero.dev/skills-to-tickets) |
| A confirmed bug that needs a root cause | [diagnosing-bugs](https://aihero.dev/skills-diagnosing-bugs) |

## Prerequisites

`ISSUES_DIR` when set, otherwise `~/work/issues`. The brief lands in `$ISSUES_DIR/<id>/brief.md`. Fetching `ticket.md` uses the `investigate-redmine-ticket` fetch script and the `redmine-bricsys` key.

## The brief

The artifact is the **brief**. Once it is written, that file is what a later [session](https://www.aihero.dev/ai-coding-dictionary/session) implements from. The Redmine description stays context. The brief names the behaviour to build, not file paths or line numbers.

## Common questions

**Where did the triage state machine go?**
This fork does not label, comment, or close Redmine issues. `/triage` writes `brief.md` for one id and stops. A queue of unlabeled issues is not something these skills list.

**Do I triage tickets I just created with `/to-tickets`?**
No. Those files are already agent-ready. Triage is for a Redmine ticket that arrived from someone else.

## It's working if

- One `brief.md` exists next to `ticket.md`, and the agent reported that path.
- The category is `bug` or `enhancement`.
- You can tell what "done" means from the acceptance lines alone.
- Redmine itself is unchanged.

## Where it fits

`triage` is an **on-ramp**, not a step in the main chain. The main flow runs from an idea you had (grill, spec, tickets, implement, review). `triage` is the lane for one ticket that arrived instead. It meets that flow at the brief, which [implement](https://aihero.dev/skills-implement) can pick up the same way it picks up a ticket from [to-tickets](https://aihero.dev/skills-to-tickets). When you're not sure which lane you are in, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
