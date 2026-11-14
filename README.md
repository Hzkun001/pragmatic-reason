# claude-skills

Personal collection of Agent Skills for Claude Code.

## Skills

| Skill | Purpose |
| --- | --- |
| [pragmatic-reasoning](skills/pragmatic-reasoning/SKILL.md) | ROI-first decision protocol for tool, stack, lab and architecture choices. Calibrates between executing immediately and pushing back, and suspends shortcuts for deep low-level exploration. |

## Install

Copy a skill folder into your Claude Code skills directory:

```bash
git clone https://github.com/Hzkun001/claude-skills.git
cp -r claude-skills/skills/pragmatic-reasoning ~/.claude/skills/
```

Use `~/.claude/skills/` for all projects, or `<project>/.claude/skills/` for one project only.
