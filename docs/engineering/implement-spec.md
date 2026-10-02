## What it does

`implement-spec` takes a [spec](https://www.aihero.dev/ai-coding-dictionary/spec) and its slice files and lands them on the branch you are already on. It reads the slices as a **task graph**. Blocking edges decide what can start, so there is always a **frontier** of slices whose blockers have landed. It implements that frontier in number order, one slice at a time.

It does not open a pull request, and it does not create or close a Redmine issue. When a slice lands, that file's status becomes `done`.

## When to reach for it

You invoke this by typing `/implement-spec`, and the [agent](https://www.aihero.dev/ai-coding-dictionary/agent) won't reach for it on its own.

| Your situation | Reach for |
| --- | --- |
| A spec, split into slice files with blocking edges, that you want landed in one run on the current branch | `/implement-spec` |
| One ticket at a time, in your own [context window](https://www.aihero.dev/ai-coding-dictionary/context-window), [clearing](https://www.aihero.dev/ai-coding-dictionary/clearing) between tickets | [implement](https://aihero.dev/skills-implement) |
| A spec that isn't split into tickets yet | [to-tickets](https://aihero.dev/skills-to-tickets) first |
| A small piece of work with no real graph | [implement](https://aihero.dev/skills-implement) directly |

## Prerequisites

Slice files in `$ISSUES_DIR/<id>/`, as [to-tickets](https://aihero.dev/skills-to-tickets) writes them, plus `spec.md` from [to-spec](https://aihero.dev/skills-to-spec). The id comes from an argument or URL, otherwise from the current `feature/<id>-`, `bugfix/<id>-`, or `hotfix/<id>-` branch.

You are already on the branch the work should land on. The skill does not create one.

## Frontier order

A slice is ready when its status is `ready` and every ticket named in **Blocked by** is `done`, or **Blocked by** says none. The run takes those in number order. Each slice is built by calling [tdd](https://aihero.dev/skills-tdd) at the seams named in the spec. After the last slice, it calls [code-review](https://aihero.dev/skills-code-review) on the current branch and fixes what that review raises.

## Common questions

**How is this different from running `/implement` on each ticket myself?**
With `implement` you are the dispatcher: one [session](https://www.aihero.dev/ai-coding-dictionary/session) per ticket, clearing in between. `implement-spec` walks the graph in one session, on the branch you already have. The price is that you review the branch at the end rather than each slice as it lands. For a small change with no real graph, skip it and use `implement`.

## It's working if

- The branch you started on is the branch the work landed on.
- A slice starts only after the slices it names as blockers are `done`.
- Each slice's trace shows `tdd` running, with a failing test before the code.
- Finished slice files say `done`.
- The run ends with a `code-review` of that branch, and no new pull request.

## Where it fits

`implement-spec` is the build step of the main chain, as the one-session alternative to running [implement](https://aihero.dev/skills-implement) once per ticket:

```txt
grill-with-docs → to-spec → to-tickets → implement-spec → retro
```

Its neighbours are [to-tickets](https://aihero.dev/skills-to-tickets), which declares the blocking edges, and [code-review](https://aihero.dev/skills-code-review), which it runs at the end. [ask-matt](https://aihero.dev/skills-ask-matt) is the router over the whole set when you are not sure which flow you are in.
