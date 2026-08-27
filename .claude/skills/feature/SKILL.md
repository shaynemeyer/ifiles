---
name: feature
description: Manage the current feature workflow — report status, load a spec, start implementation, review, test, explain, or complete a feature. Use when the user asks for feature status or progress, or to load/start/review/test/explain/complete a feature or fix, or refers to the feature lifecycle tracked in context/current-feature.md.
---

# Feature Workflow

Manages the full lifecycle of a feature from spec to merge. State is tracked in
`context/current-feature.md` — read that file at the start of every action.

## Working File

`context/current-feature.md` has these sections:

- `# Current Feature` — H1 heading, includes the feature name when a feature is active
- `## Status` — Not Started | In Progress | Complete. `complete` resets this to "Not Started" and
  records the feature under History, so "Complete" is only ever a transient value
- `## Goals` — bullet points of what success looks like
- `## Notes` — additional context, constraints, or details from the spec
- `## History` — the previously completed feature: the one finished immediately before whatever is
  currently loaded. Exactly one entry. `complete` moves any prior entry to
  `context/feature-history.md` (the archive, oldest to newest) before writing the new one, so this
  section never accumulates

## Work Log

`docs/work-log.md` is the durable record of completed work: one `##` section per feature, oldest to
newest, written when the work is finished and summarized. `complete` commits it (step 7) — but write
the entry as soon as the work is done and reported, rather than waiting, since the reasoning behind
a decision is cheapest to record while it is still in view.

It is deliberately not the same thing as the two files above. `## History` in
`context/current-feature.md` is one paragraph for orientation and holds exactly one entry;
`docs/work-log.md` is long-form and keeps everything. Neither is loaded automatically, so an entry
here is written for someone arriving cold.

## Spec Files

Feature specs live in `context/features/{nn-name}.md` (fixes in `context/fixes/`), numbered in
dependency order. Each carries a status line directly under its H1:

```markdown
# Bays CRUD

**Status:** Not Started
```

Three values are used: `Not Started`, `In Progress` which `start` writes, and
`Complete — merged YYYY-MM-DD (<sha>)` which `complete` writes.

**`In Progress` is a single-holder marker, and that is what keeps it honest.** An earlier version of
this file left it out, on the grounds that `context/current-feature.md` already tracks the active
feature and a second mutable copy would only drift. The drift is real — nothing about finishing a
*different* feature clears a marker left on an abandoned one — but the premise cuts the other way:
because only one feature is ever active, any spec marked `In Progress` that is not the loaded one is
by definition stale, so it can be found and reset rather than merely worried about. `load` does that,
and `status` reports one it finds. What the marker buys is that a spec read on its own no longer
contradicts reality, which is how a review of 15 came to report a finished feature as `Not Started`.

Outstanding work is therefore `grep -L '^\*\*Status:\*\*.*Complete' context/features/*.md`, unchanged
in meaning — `In Progress` counts as outstanding, which is correct. **The pattern is anchored to the
status line deliberately**: unanchored it is a regex over the whole file, so a spec that mentions the
words in prose reads as complete, which is exactly how `27-productionization.md` stayed invisible to
this command from the day it was written. Completed specs
stay in place rather than moving to an archive folder: they are still referenced constantly
(every CRUD feature builds on `02-domain-schema`, and 15 features extend `04-seed-harness`), and
each spec's `Depends on:` line names siblings by filename.

## Actions

The user names one action. Determine which from their request.

| Action     | Description                                               |
| ---------- | --------------------------------------------------------- |
| `status`   | Report where things stand — read-only, changes nothing    |
| `load`     | Load a feature spec or inline description                 |
| `start`    | Begin implementation, create branch                       |
| `review`   | Check goals met, code quality                             |
| `test`     | Write or verify tests for the feature's new logic          |
| `explain`  | Document what changed and why                             |
| `complete` | Commit, push, merge, reset, log in `docs/work-log.md`     |

Before executing, read the matching instruction file for the full steps:
`.kiro/skills/feature/actions/{action}.md`

If the user did not specify an action, list the options above and ask which they want.

## Constraints

- Read `context/ai-interaction.md` at the start of any action that runs commands or touches git.
  It is the source of truth for workflow, branch naming, commit format, and dev commands.
  `context/coding-standards.md` is the source of truth for code conventions. Do not hardcode
  stack-specific commands or assumptions here.
- Never commit, merge, push, or delete a branch without explicit permission, and never before the
  build passes. This overrides any looser wording in the action files.
