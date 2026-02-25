# Contributing to ai-dev-env

Contributions are welcome — bug fixes, skill improvements, new skills, and documentation.

## Getting Started

1. Fork the repository
2. Create a branch for your work (`git checkout -b my-skill-name`)
3. Make your changes
4. Submit a pull request

## Adding or Modifying Skills

Each skill lives in `skills/<skill-name>/SKILL.md`. The frontmatter must include a `name` and `description`:

```yaml
---
name: my-skill
description: Use when <specific trigger condition>
---
```

**Guidelines for skill descriptions:**
- Start with "Use when" followed by the trigger condition
- Be specific — vague descriptions prevent automatic activation
- Keep it to one sentence

**Guidelines for skill content:**
- Be prescriptive — skills should tell the agent exactly what to do, not suggest
- Include concrete steps, not principles
- Test your skill by starting a fresh Claude Code session and verifying it activates

## Pull Request Expectations

- One skill (or related set of changes) per PR
- Include a brief description of what the skill does and when it should activate
- If modifying an existing skill, explain what changed and why

## Reporting Issues

Use [GitHub Issues](https://github.com/giladresisi/ai-dev-env/issues) to report bugs or suggest improvements. Include:
- What skill is affected
- What you expected to happen
- What actually happened
