# Load Action

1. Interpret what the user supplied after "load":
   - Looks like a filename (single word, no spaces): look for `context/features/{name}.md` OR
     `context/fixes/{name}.md`
   - Multiple words: treat as an inline feature description and generate goals from it
   - Nothing supplied: stop and report that `load` requires a spec filename or feature description

2. Clear any stale `In Progress` marker:
   `grep -rl "Status:.*In Progress" context/features context/fixes 2>/dev/null`. Recursive rather
   than a `*.md` glob because `context/fixes/` may not exist yet, and zsh aborts on a glob that
   matches nothing. Every hit other than
   the spec being loaded belongs to a feature that was started and then abandoned, because only one
   feature is ever active. Set each back to `Not Started`, and say which ones were reset — an
   abandoned attempt may also have left a branch and uncommitted work behind that the user has
   forgotten about.

3. Update `context/current-feature.md`:
   - Update the H1 heading to include the feature name (e.g. `# Current Feature: Add Navbar`)
   - Write goals as bullet points under `## Goals`
   - Write any additional notes/context under `## Notes`
   - Set Status to "Not Started"

   Leave the loaded spec's own `**Status:**` line alone. `load` only prepares the work and `start` is
   what marks it underway, so loading a spec in order to read it must not claim someone is working
   on it.

4. Confirm the spec loaded and show the feature summary.

If the spec's `**Status:**` line already reads `Complete`, say so and confirm before proceeding —
loading a finished feature is usually a mistake. If it already reads `In Progress`, this feature was
started earlier and never completed: say so, and check for a leftover branch before continuing.
