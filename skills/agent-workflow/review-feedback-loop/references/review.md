# Review

## 1. Freeze the review input

Read configuration, applicable repository standards, and accepted lessons scoped
to the changed code. Identify PR/base/head revisions and task acceptance criteria.
For local changes without a PR, capture an immutable diff identity; this can
produce a review assessment, not a forge transition.

Read existing human review feedback when it is part of the current task. Historical
automation feedback is excluded from learning, but the independent reviewer may
raise new findings: the two purposes are distinct.

Completion: the current diff, intended behavior, applicable rules, and verification
requirements are available. Missing acceptance criteria are surfaced.

## 2. Dispatch an independent reviewer

Use a fresh subagent or separate session, preferably the repository's existing
review agent. Its role is read-only. Provide the immutable diff/revisions,
acceptance criteria, applicable standards, accepted lessons, and code access.
Do not prime it with the author's desired verdict or self-review conclusions.

Ask it to inspect relevant code and return evidence-backed actionable findings,
coverage of applicable requirements/rules, uncertainties, and a ready/blocked
recommendation. Blocking findings need a concrete defect, unmet requirement, or
applicable mandatory standard; stylistic tastes and unapproved lessons cannot
block handoff. Check tests for the behavior they prove, not merely their presence.

If independent execution is unavailable, report blocked: a self-review cannot
substitute for this gate. Wait for the review result before consuming it.

Completion: an independent result for the captured revision is persisted;
unreviewed areas and unverifiable claims are explicit.

## 3. Repair justified findings

Evaluate each finding against code, requirements, and standards. Fix justified
issues within task scope. Preserve unrelated user work. Run the smallest relevant
checks and test the actual behavior, including focused regression cases.

Record each disposition and its evidence. Send contested findings back to the
reviewer with concrete counterevidence. A blocker stays unresolved unless the
reviewer independently dismisses it or an authorized maintainer explicitly
accepts the exception. Record exceptions separately from fixes.

Permission to review is not permission to edit, commit, push, post comments,
change policy, mark ready, or merge. Perform only actions authorized by the user's
task and environment. If edits are not authorized, return findings and stay blocked.

Completion: each finding is fixed, independently dismissed, or unresolved;
checks and failures are recorded with no success-shaped substitutes.

## 4. Re-review the latest changes

After edits, capture the new revision/diff. Have the independent reviewer examine
the repair diff, affected behavior, and any new implications. Findings marked fixed
require independent confirmation. Reuse its context for follow-up, or dispatch a
fresh reviewer if the environment cannot resume it.

Repeat review-and-repair up to the configured limit (default three rounds total).
At the limit, report remaining blockers and keep draft status; do not broaden the
scope or repeat an unchanged argument indefinitely. A user may authorize another
bounded attempt after seeing the blocker.

Completion: the latest revision has an independent result, including verification
of repairs and any remaining blockers.

## 5. Handoff

Ready requires all of the following:

- Applicable requirements and mandatory rules are reviewed.
- No unresolved blocking findings remain.
- Required checks pass for the latest revision; unavailable required checks block.
- No relevant code, base revision, or policy/lesson change invalidates the result.

Re-read live forge state immediately before a requested ready transition. If it
changed, re-review; if closed or merged, report its state without modifying it.
Mark ready only when the user or installed integration has explicitly authorized
that transition. Otherwise report "ready for human review; PR remains draft."

Persist the review identity, dispositions, checks, and outcome. Report blockers
and verification gaps prominently. Optional check gaps remain visible even when
the team policy permits readiness. Human review remains the next authority;
this gate neither approves nor merges the PR.
