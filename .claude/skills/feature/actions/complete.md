# Complete Action

Every git step below requires explicit user permission before running, and the build must pass
first. Confirm before starting.

1. Verify the project's build and tests pass, using the commands in `context/ai-interaction.md`.
2. Stage the changes and commit, following the commit message convention in
   `context/ai-interaction.md`.
3. Switch to main and merge the feature branch (no push yet).
4. Ask before deleting the local feature branch, then delete it.
5. Mark the spec complete: set the `**Status:**` line under the H1 of the feature's file in
   `context/features/` (or `context/fixes/`) to
   `Complete — merged YYYY-MM-DD (<merge commit sha>)`. It will read `In Progress` at this point,
   because `start` wrote that; replace the whole line rather than appending to it.
6. Roll the history forward, in this order — the move must happen before the new entry is written,
   or the archive ends up out of order:
   - If `## History` in `context/current-feature.md` already holds an entry, append it to the END
     of `context/feature-history.md` (the archive, oldest to newest)
   - Then reset `context/current-feature.md`:
     - Change the H1 back to `# Current Feature`
     - Set Status back to "Not Started"
     - Clear the Goals and Notes sections (keep placeholder comments)
     - Write this feature's summary as the **only** entry under `## History`
7. Append this feature's summary to `docs/work-log.md` — the durable record of what was built and
   why, kept in the repo rather than only in the chat that produced it. Newest entries go at the
   END, so the file reads oldest to newest like `context/feature-history.md`.

   One `## nn-name — Title` section per feature, with a `**Branch:** … · **Merged:** YYYY-MM-DD
   (<sha>)` line under it. Cover, in whatever subheadings the work warrants:
   - what was built, and the decisions that were judgement calls rather than the obvious choice
   - anything that deviates from the spec, and why
   - bugs found during the work that the build could not see, and what now pins them
   - what was verified, against what (test counts, and specifically anything checked against real
     Postgres rather than SQLite)
   - anything left open for the user's judgement

   **Longer and more specific than the `## History` entry**, which is a paragraph for orientation.
   This is the entry someone reads in six months to find out why a field is shaped the way it is.
   Do not restate the spec — that file is still in `context/features/` and is linked by name.

   If `docs/work-log.md` does not exist, create it with a `# Work Log` H1.

8. Commit the reset and the work log:
   `chore: reset current-feature.md after completing [feature]`
9. Push main to origin ONCE (a single push with all changes).
10. If the feature branch was previously pushed, delete it from origin.
