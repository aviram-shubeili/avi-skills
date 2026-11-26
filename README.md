# avi-skills

My agent skills for Claude Code, GitHub Copilot CLI, and any other agent that reads `SKILL.md` files.

## Skills

Skills are grouped by category under `skills/<category>/<skill>/`.

### Agent workflow

Skills that shape how an agent takes on a task.

| Skill | What it does |
| --- | --- |
| [clean-context](skills/agent-workflow/clean-context/SKILL.md) | Takes a raw idea or prompt dump (dictation, notes, images), removes your ideas about *how* to solve it, gives the clean brief to a blind sub-agent, and then cross-checks the agent's result against your original ideas. |
| [review-feedback-loop](skills/agent-workflow/review-feedback-loop/SKILL.md) | Learns evidence-backed code-quality lessons from human PR feedback, keeps resumable state per repository, and independently reviews and repairs agent-authored draft PRs before human handoff. |

### Product

Skills for finding, validating, and sizing up product ideas.

| Skill | What it does |
| --- | --- |
| [problem-hunter](skills/product/problem-hunter/SKILL.md) | Problem-first research for solo micro-SaaS and browser extensions. Three modes: **discover** validated pains from reviews and forums, **validate** an idea you already have (go / narrow / kill), or mine **competitors'** negative reviews for gaps a single-feature product can win. Includes a query playbook, an anti-AI-dismissal rubric, scoring, and output templates. |

## Install

With the [skills CLI](https://github.com/vercel-labs/skills) (Node.js 22.20+), which finds skills in category folders:

```bash
npx skills add aviram-shubeili/avi-skills                         # all skills, this project
npx skills add aviram-shubeili/avi-skills -g                      # all skills, all projects
npx skills add aviram-shubeili/avi-skills --skill problem-hunter  # one skill
```

Or copy a skill folder into your agent's skills directory by hand. Agents expect a flat layout there, so copy the skill folder itself, not its category folder:

```bash
# Claude Code (all projects)
cp -r skills/product/problem-hunter ~/.claude/skills/

# GitHub Copilot CLI / per-repo (also read by Claude Code)
cp -r skills/agent-workflow/clean-context <your-repo>/.github/skills/
```

## Adding a skill

Put it in `skills/<category>/<skill-name>/SKILL.md`, with the folder name matching the skill's `name`. Reuse an existing category when one fits, and add a row to the matching table above.

## License

MIT
