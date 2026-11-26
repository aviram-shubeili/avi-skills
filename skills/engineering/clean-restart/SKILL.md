---
name: "clean-restart"
description: "Throw away a sloppy implementation and rebuild from scratch: distill the spec and lessons learned into a clean brief, have a fresh agent re-implement it blind, then compare."
---

# Clean Restart

Use this skill when an implementation works, or almost works, but the code is slop: workarounds, tricks, patches stacked on patches, and nothing a reader can maintain. Patching it further keeps the bad design alive, and so does refactoring it in the same session, because the agent that wrote it stays anchored on its own choices.

The fix is to keep what the attempt **learned** and drop how it was **built**. A fresh agent then re-implements the work from a clean brief, without ever seeing the old code.

You are the **orchestrator**. You may read everything: the conversation, the old code, the diff, and the tests. The **builder** is a fresh-context sub-agent (Agent/Task tool, or a Copilot CLI custom agent). Never fork it or let it inherit this conversation. If you can't start a fresh-context sub-agent, write the brief and tell the user to run it in a new session.

---

## 1. Gather
- Read the original request and everything said in the conversation: requirements, corrections, and complaints.
- Read the failed attempt as a diff against its base commit. Note the base commit.
- Read the repo's own standards: CLAUDE.md / AGENTS.md, lint config, conventions, and the patterns in neighboring code.
- Run the existing tests and note which ones pass.

## 2. Extract
Sort what you found into the four groups below.

**Spec (keep)**
- Required behavior, inputs and outputs, and edge cases.
- Acceptance criteria. Where possible, write them as checks the builder can run.
- Interfaces that must not change: public APIs, DB schemas, contracts with other modules, and the names of anything that other code calls.
- Non-functional needs: performance, security, compatibility.

**Standards (keep)**
- Repo conventions, architecture boundaries, testing expectations, and the user's quality bar. When the user's complaint names a problem ("no magic timeouts", "no god class"), turn it into a positive standard.

**Lessons (keep, but rewritten as facts)**
Every workaround in the old code exists because something surprised the old builder. Find what that was, and keep only the *fact*:
- "Added a 500ms retry loop" becomes "Service X returns 409 for a short time after a write; reads may briefly be stale."
- "Special-cased null in three places" becomes "Field Y can be null for legacy records."

Keep only facts you verified, from code, docs, logs, or the old attempt's test results. Mark anything you inferred as *unverified*. Never write the old fix into the brief, not even as a hint.

**Discard**
The old architecture, file layout, class and function names (unless they're part of a contract), helper utilities, workaround mechanics, and any tests that only test those internals.

## 3. Decide what happens to the old tests
- Keep tests that check **behavior** through a stable interface. They become acceptance checks, and the builder may read them.
- Drop tests that lock in the old internals.
- If the behavior has no tests, write the missing acceptance cases as plain-language checks in the brief.

## 4. Write the restart brief

```
## Goal
## Spec
## Must not change
## Standards
## Known facts about the environment
(the lessons, phrased as facts; unverified ones marked)
## Done when
(runnable checks, plus: no workarounds without a comment explaining the underlying fact)
## How to work
Start from the base commit; do not look at branch <old-branch> or its history.
Design first: write a short design note (components, data flow, why this shape) before coding.
Prefer the simplest design that meets the spec and standards.
If you hit a problem that tempts a hack, stop and fix the cause, or report it.
When done, return: the design note, what you built, test results, and any new facts you discovered.
```

Write the brief so it never mentions the old attempt's design, and never says how it went wrong beyond the standards above.

## 5. Review gate
- Show the user the brief and the list of what you discarded. Ask: "Restart with this, edit it, or stop?"
- Ask specifically about any lesson marked *unverified*, and about anything you were unsure whether to keep.
- Skip this gate only if the user said "fully auto."

## 6. Rebuild blind
- Before starting the builder, create a worktree or branch from the **base commit**, so the old code isn't in its files. Keep the old branch for the comparison step; don't delete it.
- Start a fresh-context builder with the brief verbatim and nothing else.
- Don't steer the builder while it works. If it comes back blocked, give it the missing fact only, never old code.

## 7. Compare
Put the new implementation next to the old one:
- **Spec coverage**: run the acceptance checks against both versions.
- **Lessons**: did the new code handle each known fact? Say how, in one line each.
- **Simplicity**: lines changed, workaround count, new abstractions, and how much of the code is special-case handling.
- **Regressions**: anything the old version handled that the new one doesn't. Say whether it matters.
- **New facts**: things the builder discovered that the old attempt missed.

## 8. Deliver
1. The new branch.
2. A comparison table.
3. A recommendation, with one question: adopt the new version, ask the builder for specific fixes, or restart again with an updated brief?

If the new version also needs workarounds for the same fact, the problem is probably in the environment, not the design. Say so plainly.