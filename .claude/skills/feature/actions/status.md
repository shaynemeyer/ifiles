# Status Action

Read-only. Never modifies a file, never touches git state, never asks for permission.

1. Read `context/current-feature.md` — the H1 and `## Status`, plus the `## History` entry (the
   previously completed feature).
2. Count progress across specs. **Every pattern below is anchored to the start of the status line,
   and must stay that way** — unanchored, they are regexes matched against the whole file, so a spec
   that merely *mentions* "Complete" or "In Progress" in prose is miscounted. That is not
   hypothetical: it hid `27-productionization.md` from the outstanding list from the day it was
   written. See `AGENTS.md`.
   - complete: `grep -l '^\*\*Status:\*\*.*Complete' context/features/*.md`
   - outstanding: `grep -L '^\*\*Status:\*\*.*Complete' context/features/*.md`
   - in progress: `grep -rl '^\*\*Status:\*\*.*In Progress' context/features context/fixes 2>/dev/null` — at most one, and it should
     be the spec named in `context/current-feature.md`. More than one, or one that disagrees with
     `current-feature.md`, is a stale marker from an abandoned attempt: report it as an anomaly
     rather than quietly fixing it, since it usually means a branch was left behind too
   - the next feature is the lowest-numbered outstanding spec whose dependencies are all complete
3. Report git state: current branch, uncommitted change count, commits not pushed to origin.
4. Report gate state using the commands in `context/ai-interaction.md`. Run them only if they are
   fast; if a gate is slow or needs a service that is not running, say it was not checked rather
   than reporting a guess.
5. Note anything blocked or awaiting a decision — a dirty tree, an unmerged branch, a spec loaded
   but not started, gates failing.

## Output Format

Lead with the answer, then the detail. Keep it to a handful of lines — this action is used to
orient quickly, so a long report defeats it.

```
Current:   <feature name and status, or "nothing loaded">
Progress:  <n> of <total> complete — next up <nn-name>
Git:       <branch>, <clean | n uncommitted>, <n unpushed>
Gates:     <lint / tests / migrations, or "not checked (reason)">
Previous:  <feature from History>
```

Follow with a line on what to do next, and anything waiting on the user. If everything is clean
and nothing is loaded, say so plainly instead of padding the report.
