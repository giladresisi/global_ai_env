# ai-dev-env

A collection of Claude Code skills for software development workflows — planning, execution, code review, API research, git, and validation.

## What it is

`ai-dev-env` gives your Claude Code agent a structured, opinionated development workflow through composable skills. Rather than leaving your agent to improvise how it plans, reviews, commits, and validates work, these skills encode proven patterns that trigger automatically when relevant.

The result: an agent that consistently produces implementation plans before touching code, runs structured code reviews before committing, investigates bugs systematically before proposing fixes, and validates its own work before declaring it done.

## Installation

### Claude Code (via Plugin Marketplace)

Register the marketplace, then install:

```bash
/plugin marketplace add giladresisi/ai-dev-env-marketplace
/plugin install ai-dev-env@ai-dev-env-marketplace
```

### Verify Installation

Start a new session and ask your agent to plan a feature or commit changes. It should automatically invoke the relevant skill rather than improvising.

## Skills

### Planning

| Skill | When it activates |
|-------|-------------------|
| `create-prd` | Creating a Product Requirements Document (PRD.md) from a conversation or feature request |
| `plan-feature` | Creating a comprehensive implementation plan including codebase analysis and parallel execution strategy, used best for implementing a single feature / phase from a PRD |

### Execution

| Skill | When it activates |
|-------|-------------------|
| `execute` | Implementing a feature plan file, choosing between sequential and team-based parallel execution with mandatory validation gates; automatically calls `execution-report` on completion to surface coverage gaps |
| `init-project` | Setting up or reinitializing a project locally for the first time |

### Code Review & Quality

| Skill | When it activates |
|-------|-------------------|
| `code-review` | Performing a technical pre-commit code review on recently changed files for bugs, security issues, and standards compliance |
| `code-review-fix` | Fixing bugs found during a code review, processing each issue with explanation and test verification |

### API Research

| Skill | When it activates |
|-------|-------------------|
| `explore-api` | Researching an unfamiliar external API before integration to verify feasibility and limitations |
| `prime` | Loading project context and architecture overview before starting implementation work |
| `prime-tools` | Loading tool docstring patterns and best practices before building or modifying agent tools |

### GitHub Bug Fix

| Skill | When it activates |
|-------|-------------------|
| `rca` | Investigating a GitHub issue to identify root cause, assess impact, and create a fix strategy document |
| `implement-fix` | Implementing a fix for a GitHub issue that already has an RCA document |

### Git

| Skill | When it activates |
|-------|-------------------|
| `commit` | Committing changes — includes secret scanning, conventional commit format, and staged-all by default |
| `create-worktree` | Creating a git worktree for feature isolation with automatic environment setup |

### Validation & Reporting

| Skill | When it activates |
|-------|-------------------|
| `validate` | Running comprehensive project validation including tests, type checking, linting, and server startup |
| `cleanup-progress` | Cleaning up PROGRESS.md after feature completion, replacing verbose notes with a concise summary |
| `execution-report` | Generating an implementation report after feature completion, including a coverage gap analysis comparing what was planned vs what was actually tested; called automatically by `execute` |
| `system-review` | Performing a meta-level analysis of plan adherence to identify process improvements |

## Philosophy

- **Plan before code** — Implementation plans are mandatory, not optional
- **Review before commit** — Code review happens as part of the workflow, not as an afterthought
- **Systematic over ad-hoc** — Structured processes for debugging, planning, and validation
- **Evidence over claims** — Validate and report results, don't just declare work done

## Updating

Skills update automatically when you update the plugin:

```bash
/plugin update ai-dev-env
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT License — see [LICENSE](LICENSE) for details.

## Support

- **Issues**: https://github.com/giladresisi/ai-dev-env/issues
