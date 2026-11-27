# Learn

## 1. Select or resume

Read configuration and existing progress. On the first bootstrap, select 50 PRs
across all states, ordered by creation time descending with a stable ID tie-break.
Persist this fixed sample. Resume an interrupted bootstrap from its checkpoints.

After bootstrap, include new PRs and previously analyzed PRs with changed
discussions or outcomes. An old PR can acquire new comments. Use the forge's
updated-time index with an overlap window and stable-ID deduplication; where that
index cannot expose edits/deletions reliably, periodically reconcile tracked
discussion fingerprints. Record the limits of the available API.

Completion: every selected PR has a pending or reusable checkpoint and the
selection rule, count, and refresh coverage are recorded.

## 2. Retrieve discussions and context

Read each selected PR's description, review summaries, inline threads/replies,
and general conversation comments with complete pagination. Include resolved and
outdated threads: they can contain the strongest learning evidence.

Classify authorship using platform account type and known review integrations.
Exclude automated reviewer comments from lesson evidence, including their
nitpicks. Preserve substantive human replies to automated threads. Flag uncertain
classification rather than guessing from display names. Human style guidance is
eligible when accepted and generalizable; a preference is not automatically a rule.

For substantive human feedback, inspect the relevant diff at the comment's
revision, subsequent discussion, and resulting code when available. Distinguish
requested, implemented, defended, retracted, superseded, and unknown outcomes.
Thread resolution or PR merge alone does not prove a suggestion was implemented.

Completion: all relevant discussion endpoints are exhausted; needed outcome
evidence is read or marked unavailable. Failures leave the PR partial.

## 3. Derive scoped lessons

Separate observations from actionable lessons. Capture the failure mode,
why it matters, a corrective practice, applicable context, and source evidence.

Examples of categories: behavior and regression risk; abstractions and ownership;
test determinism and meaningful assertions; fixtures and isolation; error handling;
performance; API compatibility; security and deployment boundaries.
These are search lenses, not requirements to invent a lesson in each category.

Compare candidates with existing standards and lessons. Link evidence for an
existing rule instead of duplicating it. Record counterexamples and conflicting
reviews. A rejected suggestion can teach a boundary or exception; it cannot be
reported as an accepted improvement.

Completion: each substantive feedback item has a disposition: linked lesson,
new candidate, context-specific observation, duplicate, or excluded with reason.
PRs without relevant human feedback have an explicit no-lesson disposition.

## 4. Persist and propose

Persist evidence and candidate changes before marking each PR complete.
Process in small batches so interruption preserves useful progress. Propose
promotion or retirement through the configured maintainer mechanism.

Write accepted guidance only after explicit approval. Edit the authoritative
editable source, or maintain accepted lessons behind its short context pointer.
Keep raw evidence separate from the guidance loaded during coding sessions.

Completion: every sampled PR is complete or explicitly partial; counts reconcile,
candidates are deduplicated, and accepted guidance has not changed without approval.

Report actual coverage, recurring lessons with source links, contradictory
evidence, and approval decisions needed. Claim partial completion when access,
pagination, or code evidence is missing.
