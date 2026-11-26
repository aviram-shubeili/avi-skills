# Repository-local state

The following is a logical contract, not a mandate to replace an existing store.
Use equivalent records in the team's current tooling when they preserve these
invariants. For a new file-based store, use these names.

| Record | Contents |
|---|---|
| `config.json` | Schema version, canonical repository identity, storage locations, sample policy, approval owner/mode, round limit, covered environments, gate capabilities |
| `progress.json` | Fixed bootstrap PR identities, per-PR checkpoints, refresh cursor and incomplete reads |
| `evidence.jsonl` | Deduplicated evidence records and revisions |
| `lessons.json` | Candidate, accepted, and retired lessons with their provenance |
| `reviews/<pr>-<head>.json` | Independent review, findings, dispositions, checks, and handoff status |

Use schema version `1`. Check repository identity and schema before reuse.
On mismatch or corrupt state, stop and report it; preserve the existing records.
Initialize only missing files during setup, never replace an existing store with
empty state. Store JSON using the project's serialization and formatting tools.

## Progress and evidence

Freeze the bootstrap sample before analysis so interruption does not change the
meaning of "50 most recent." If fewer PRs exist, record the actual count.

Each PR checkpoint includes its identity, observed head, retrieval time,
discussion fingerprint, pagination completion by endpoint, analysis status,
and errors. A PR is complete only after all relevant discussion pages and needed
code/outcome evidence are read and its lessons or no-lesson disposition recorded.

Evidence uses stable source IDs: repository + PR + comment/thread ID. Preserve
edit fingerprints and dispositions without creating duplicate observations.
Include source URLs, relevant code revision, concise paraphrase, human/automation
classification basis, discussion outcome, uncertainty, and linked lesson IDs.
Use authorship only to classify evidence; avoid copying personal profiles.

An empty response is not proof that a discussion is absent if retrieval failed.
Retain partial progress and retry only the incomplete reads.

## Lessons

Each lesson contains:

- Stable ID, status, concise actionable rule, rationale, and category.
- Applicability: languages, paths/components, and relevant behavioral conditions.
- Evidence IDs, counterevidence, confidence, and known exceptions.
- Approval provenance and timestamps for promotion, revision, or retirement.

Candidates can come from one strong example. Repetition increases confidence,
not authority. Accepted lessons require explicit maintainer approval through the
configured mechanism; setup authorization is not approval of future lessons.
Existing documented rules are referenced rather than copied into a second rule.

Keep one canonical lesson per meaning. Updates link additional evidence to that
lesson. Contradictions reopen a lesson for review; they cannot silently rewrite
accepted guidance. Until resolved, mark the affected lesson disputed and surface
the conflict instead of treating it as a blocking rule.

## Review and persistence

A review records PR identity, base and head revisions, diff identity, applicable
standards/lesson versions, reviewer identity or run handle, round number, findings,
author dispositions, independent verification, executed checks, missing checks,
and `ready` or `blocked` with reasons.

Findings have stable IDs, locations, severity, evidence, suggested correction,
and status. Distinguish fixed, independently dismissed, and unresolved.
An author-only dismissal does not clear a blocking finding.

Write atomically using an available native mechanism. Serialize updates to a
single repository store; use locking or revision checks where concurrent agents
can write. Detect a concurrent change and reconcile it before retrying. Advance
checkpoints only after corresponding evidence and analysis have been persisted.
Keep backups or use version control for recoverability.

PR-ready authorization remains separate from a persisted `ready` assessment.
Recheck head and base revisions, relevant policy/lesson versions, and required
checks immediately before handoff. A changed identity invalidates clearance.
