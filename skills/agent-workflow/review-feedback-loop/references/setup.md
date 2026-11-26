# Setup

## 1. Discover the boundary

Identify the repository's canonical forge identity, existing instructions,
standards, review agents, checks, and draft-to-ready mechanism. Read applicable
instructions before proposing edits.

Use the existing review process rather than installing a competing set of rules.
Determine which agent environments the team expects to use. If not specified,
ask which environments to cover and who approves learned lessons.

Completion: the authority, supported environments, approval owner, and available
enforcement mechanism are identified; unknowns are explicitly listed.

## 2. Configure repository-local state

Use an existing appropriate knowledge directory if there is one. Otherwise propose
`.review-feedback/` using the contract in [State](state.md).

For an explicit setup request, initialize non-destructive configuration and empty
records within the requested scope. Confirm storage and visibility before saving
review evidence. Prefer shared, versioned accepted lessons and access-controlled
local evidence/progress; record both locations in configuration.

Defaults:

- Bootstrap sample: 50 PRs, all states, newest by creation time.
- Human-authored discussions supply learning evidence.
- New lessons begin as candidates; maintainers approve promotion.
- Review findings are internal unless posting is explicitly requested.
- At most three review-and-repair rounds per handoff attempt.
- Draft-to-ready transition requires a current independent review result.

Completion: configuration records explicit repository identity, storage locations,
lesson approval mode, review round limit, and covered environments.

## 3. Add a tight context pointer

Patch the authoritative editable instruction source, respecting generated files.
Use its normal discovery mechanism; avoid duplicating instructions across files.
Adapt this pointer to the actual installed skill and state location:

> Before preparing an agent-authored draft PR for human review, run
> `review-feedback-loop` review using `.review-feedback/config.json`.
> Read accepted lessons applicable to the changed code. Keep blocked PRs in draft.

Installation paths and invocation syntax vary by agent environment. Verify that
each covered environment can actually discover the skill and the pointer.

Completion: supported environments have a working pointer; uncovered ones are
named rather than implied to be supported.

## 4. Distinguish guidance from enforcement

Instruction-only setup relies on an agent following the pointer. Call it
**instruction-only**, not enforced.

If enforcement is requested, use the team's existing PR integration where possible.
A workflow or extension may run an independent reviewer and record a result tied
to the head revision. Required checks can block merge; by themselves they do not
prevent marking a PR ready. A controlled transition service can guard transitions
it owns; it cannot claim to prevent manual bypasses unless the platform supports
that restriction.

Ask for approval before adding privileged integrations, schedules, secrets,
branch rules, or background services. Apply least privilege; untrusted fork code
must not execute with write credentials. The integration validates the revision
again immediately before any transition and invalidates clearance on changes.

Completion: describe precisely what is guarded (agent handoff, ready transition,
or merge), what can bypass it, and what remains unavailable.

## 5. Verify setup

Use a synthetic blocked review and a stale-revision result. Both must fail the
configured gate. A clean result may pass only for its reviewed revision.
For instruction-only setup, verify discovery and describe enforcement as absent.

Deliver the configuration and changed instruction locations. Installing the loop
does not initiate historical mining, create PRs, or mark existing PRs ready.
