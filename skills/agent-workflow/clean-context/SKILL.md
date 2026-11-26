---
name: "clean-context"
description: "Turn a raw idea or prompt dump into a clean brief, run a blind sub-agent on it, then cross-check its result against the user's original ideas."
---

# Clean Context

The user often dictates ideas and prompts with speech-to-text. These dumps mix the real problem with the user's own ideas about how to solve it. If an agent sees those ideas, it gets anchored on them. This skill keeps them apart:

- You are the **orchestrator**. You see everything the user gave you.
- A **blind worker** sub-agent sees only a clean brief and solves the problem on its own.
- When the worker is done, you compare its result with the user's original ideas.

The whole flow runs in this one session. You need a tool that starts a sub-agent with a **fresh context**, such as the Agent/Task tool in Claude Code or a custom agent in Copilot CLI. A forked sub-agent, or one that inherits this conversation, would see the raw dump, so don't use one. If you can't start a fresh-context sub-agent, say so. Then give the user the clean brief and the sealed notes so they can run the worker step in a new session themselves.

---

## 1. Read for intent
The input can be dictated text, notes, pasted context, or images. Dictation is messy: it repeats itself, corrects itself, and trails off. Work out what the user means. When a later statement contradicts an earlier one, the later one wins.

## 2. Sort every piece of information
- **Keep**:
  - the goal or problem, why it matters, and who it's for
  - observable facts: symptoms, errors, logs, current behavior, what already exists
  - hard constraints (must / must not): stack, platform, deadlines, compliance, APIs that can't change
  - success criteria and scope
  - pointers to where things live (repo, files, docs, tickets)
- **Seal** (anything that steers the solution):
  - proposed approaches, architectures, patterns, algorithms
  - preferred libraries or tools
  - suspected root causes and hypotheses
  - which file or function to change, and step-by-step plans
  - designs, sketches, and "do it like X does it" templates
  - alternatives the user already ruled out
- **Ask**: items that could go either way ("use Postgres": a constraint or a preference?).
  - Ask about all of them in one batch, at most 4 questions, multiple choice where possible.
  - If the user isn't there to answer, seal the item and mark it *Unsure*.

Images: keep what they show as fact (an error screenshot, the current UI, real data). Seal sketches or mockups of a proposed solution.

## 3. Write the brief and the sealed notes

The clean brief must not leak the sealed ideas:
- State symptoms, not diagnoses. "Add a retry so uploads stop failing" becomes "Uploads fail intermittently."
- Never name a sealed technology or approach, not even as "e.g."
- Add no hints like "consider caching."
- Keep the user's domain terms.

Use this template for the clean brief. Leave out empty sections, and never mention that anything was removed:

```
## Goal
## Context
## Constraints
## Done when
## How to work
Before committing, explore more than one approach. When you finish, return your result plus: the approach you chose, 2–3 alternatives you considered, why you rejected them, and any assumptions you made.
```

Use this template for the sealed notes. Number every item so the cross-check can refer to it:

```
# Sealed notes: <short title>, <date>
## Approaches / ideas
- S1 ...
## Hypotheses / suspected causes
- H1 ...
## Implementation details
- I1 ...
## Unsure
- U1 ...
```

## 4. Review gate
- Show the user both blocks, then ask: "Send as is, edit, or stop?"
- Skip this gate only if the user said "no review" or "fully auto."
- Before sending, reread the brief against every sealed item. If any item is named, hinted at, or implied, rewrite that line.

## 5. Run the blind worker
- Start a fresh-context sub-agent and give it the clean brief **verbatim**, with nothing else. Don't include the sealed notes, a path to them, or any summary of the user's ideas.
- Never write the sealed notes to a file that the worker can read.
- For plans, designs, or investigations, tell the worker not to change any files. If it should write code, isolate it in a worktree or branch when you can.
- Spawn 2–3 workers in parallel only if the user asks, to see how much their answers vary.
- Don't steer the worker while it runs, because anything you add could leak a sealed idea. If it comes back blocked on a missing fact, give it that fact and nothing more.

## 6. Cross-check
The worker's result is final at this point. Compare it with the sealed notes honestly, in both directions:
- Don't favor an idea because the user had it.
- Base every verdict on what the worker actually produced or reported. Don't guess at reasoning it didn't give.

Give each sealed item one verdict:
- **Matches**: the worker reached it independently. Say where.
- **Worker's is better**: explain concretely what the sealed idea would have missed or cost.
- **Sealed is better**: the user's idea beats the worker's or catches something it missed. Say what to change.
- **Different, both valid**: state the trade-off.
- **Disproved**: evidence shows the idea doesn't hold.
- **Not addressed**: the worker didn't touch it. Say whether it still matters.

## 7. Deliver
1. The worker's result, or a path to it.
2. A table with the columns: ID | sealed idea (short) | verdict | why (1–2 sentences, concrete).
3. **Discovered beyond the sealed notes**: what the worker found that the user didn't anticipate.
4. **Recommendation**, then one question: should the worker continue, merge specific sealed items (by ID), or start over?