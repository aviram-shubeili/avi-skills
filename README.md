# avi-skills

My agent skills for Claude Code, GitHub Copilot CLI, and any other agent that reads `SKILL.md` files.

## Skills

| Skill | What it does |
| --- | --- |
| [clean-context](skills/clean-context/SKILL.md) | Takes a raw idea or prompt dump (dictation, notes, images), removes your ideas about *how* to solve it, gives the clean brief to a blind sub-agent, and then cross-checks the agent's result against your original ideas. |

## Install

With the [skills CLI](https://github.com/vercel-labs/skills) (Node.js 22.20+):

```bash
npx skills add aviram-shubeili/avi-skills        # this project
npx skills add aviram-shubeili/avi-skills -g     # all projects
```

Or copy a skill folder into your agent's skills directory by hand:

```bash
# Claude Code (all projects)
cp -r skills/clean-context ~/.claude/skills/

# GitHub Copilot CLI / per-repo (also read by Claude Code)
cp -r skills/clean-context <your-repo>/.github/skills/
```

## License

MIT
