# Start Action

1. Read `context/current-feature.md` and verify Goals are populated.
2. If Goals are empty, stop and report that the `load` action must run first.
3. Set Status to "In Progress".
4. Set the spec's own `**Status:**` line to `In Progress` as well — the line directly under the H1
   of the file in `context/features/` (or `context/fixes/`) named under `## Notes`. Both files then
   say the same thing, so a spec read on its own is not misleading; see the Spec Files section of
   `SKILL.md` for why this duplication is safe, and `load` for what clears a stale marker. Skip this
   step when the feature was loaded from an inline description and has no spec file.
5. Create and check out the branch, deriving the name from the H1 heading and following the
   naming convention in `context/ai-interaction.md`.
6. List the goals, then implement them one by one.
