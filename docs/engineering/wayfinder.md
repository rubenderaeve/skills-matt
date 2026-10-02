## What it does

`wayfinder` takes an effort too big for one agent [session](https://www.aihero.dev/ai-coding-dictionary/session): an idea whose **destination** you can name but whose route you cannot yet see. It charts that route as a **map** of **decision tickets** in one Redmine issue folder, then resolves them one at a time until the way is clear.

It plans, it does not do. Every ticket holds a question, not a slice of a build, and the map is finished when nothing is left to decide before someone goes and builds the thing. When the map clears, wayfinder hands off. It does not carry on into code.

## When to reach for it

You invoke this by typing `/wayfinder` with a Redmine id. The [agent](https://www.aihero.dev/ai-coding-dictionary/agent) won't reach for it on its own.

The trigger is narrow: the effort has to be larger than one session can hold, and the route has to be foggy. `/grill-with-docs` is for single-session planning. `/wayfinder` is for multi-session planning.

| What you have in front of you | What to run |
| --- | --- |
| A well-scoped feature you can settle in one sitting | [grill-me](https://aihero.dev/skills-grill-me), or [grill-with-docs](https://aihero.dev/skills-grill-with-docs) when there is a codebase |
| A build spanning many sessions, with the route still unclear, and a Redmine id | `/wayfinder` |
| A thread where the deciding is already done | [to-spec](https://aihero.dev/skills-to-spec) |
| A cleared wayfinder map | [to-spec](https://aihero.dev/skills-to-spec), then [to-tickets](https://aihero.dev/skills-to-tickets) and [implement](https://aihero.dev/skills-implement) |

## Prerequisites

A Redmine id. The map is `$ISSUES_DIR/<id>/wayfinder.md`. Decisions are `$ISSUES_DIR/<id>/decisions/<NN>-<slug>.md`. There is no second folder for an effort that has no ticket yet. Pass the parent id, or the skill asks.

## The map, the fog, and the frontier

The map is an index. A decision lives in its own file. The map gists closed decisions and links them.

**Fog** is in-scope work you cannot phrase as a question yet. It sits under **Not yet specified** until you can. **Out of scope** is work beyond the destination. It does not graduate.

The **frontier** is the open, unblocked, unclaimed decision files. Blocking is a **Blocked by** line in the file. A session claims a decision by setting its status to `claimed` before it works, then `closed` when the answer is written. One decision per session, except research tickets fired while charting.

## Common questions

**Where did `decision-mapping` go?**
It is this skill, renamed to `wayfinder` in v1.1 and invoked as `/wayfinder`. A **decision ticket** is a question, not an implementation [ticket](https://www.aihero.dev/ai-coding-dictionary/ticket).

**Where do I see the frontier if there is no tracker board?**
In the decision files. A file with status `open` and blockers that are `closed` is takeable. The map does not list open tickets. It only gists the ones already closed.

## It's working if

- The destination is written down and agreed before a single decision file exists.
- Every open decision reads as a question. A file that says "build the X" belongs downstream of the map.
- A session claims one decision, writes the answer into that file, marks it closed, and leaves one gist line on the map. Then it stops.
- **Not yet specified** shrinks as fog becomes files.
- When the opening grill turns up no fog, the skill stops and tells you the effort is small enough to skip the map.
- The session that finishes the map points you at a spec, not a pull request.

## Where it fits

`wayfinder` is a **situational on-ramp**, not the default front door. Most work starts on the grill-led chain. Wayfinder is what you climb onto when the idea is too big for one session, and it merges back at [to-spec](https://aihero.dev/skills-to-spec), because a cleared map hands off rather than builds.

Underneath, it is other skills wearing wayfinder's scheduling: [grilling](https://aihero.dev/skills-grilling) and [domain-modeling](https://aihero.dev/skills-domain-modeling) resolve the default ticket, [prototype](https://aihero.dev/skills-prototype) resolves the tickets that talking cannot, and [research](https://aihero.dev/skills-research) writes its file into the same issue folder. [handoff](https://aihero.dev/skills-handoff) is the bridge in and out. For anything else, [ask-matt](https://aihero.dev/skills-ask-matt) routes over the whole set.
