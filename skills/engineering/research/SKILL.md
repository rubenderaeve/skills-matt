---
name: research
description: Use when the user wants a topic researched, docs or API facts gathered, or reading legwork delegated to a background agent.
---

Spin up a background agent to do the research, so you keep working while it reads.

Its job:

1. Investigate the question against primary sources (official docs, source code, specs, first-party APIs), not a secondary write-up of them. Follow every claim back to the source that owns it.
2. Write the findings to one Markdown file. Cite each claim's source.
3. Save it at `$ISSUES_DIR/<id>/<slug>.md`. Resolve the id from an id or URL in the task, then from the current branch matching `feature/<id>-`, `bugfix/<id>-`, or `hotfix/<id>-`. If neither resolves, ask. Do not guess. Slug is kebab-case from the question. The folder is `$ISSUES_DIR/<id>/` when `ISSUES_DIR` is set, otherwise `~/work/issues/<id>/`. Create it if missing. If that file already exists, ask before overwriting.
4. Report the path.

Redmine stays read-only. Do not create, edit, comment, assign, or close an issue.
