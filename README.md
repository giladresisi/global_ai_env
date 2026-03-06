# ai-dev-env

A collection of Claude Code skills for software development workflows — planning, acceptance criteria, execution, code review, research & priming, GitHub bug fixes, git, and validation & reporting.

## What it is

`ai-dev-env` gives your Claude Code agent a structured, opinionated development workflow through composable skills. Rather than leaving your agent to improvise how it plans, reviews, commits, and validates work, these skills encode proven patterns that trigger automatically when relevant.

The result: an agent that consistently produces implementation plans before touching code, runs structured code reviews before committing, investigates bugs systematically before proposing fixes, and validates its own work before declaring it done.

## Installation

### Skills CLI (via [skills.sh](https://skills.sh))

Install all skills:

```bash
npx skills add https://github.com/giladresisi/ai-dev-env
```

Install a specific skill:

```bash
npx skills add https://github.com/giladresisi/ai-dev-env --skill <skill-name>
```

See [skills.sh](https://skills.sh) for more details.

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

### Acceptance Criteria

| Skill | When it activates |
|-------|-------------------|
| `acceptance-criteria-define` | Defining, reviewing, or writing acceptance criteria for a request or plan — derives proposed criteria, confirms with the user, and writes them to the plan file or `acceptance_criteria.md` |
| `acceptance-criteria-validate` | Validating that all acceptance criteria were met after execution — produces a per-criterion PASS/FAIL/PARTIAL verdict and an overall ACCEPTED / REJECTED / NEEDS REVIEW verdict |

### Execution

| Skill | When it activates |
|-------|-------------------|
| `execute` | Implementing a feature plan file, choosing between sequential and team-based parallel execution with mandatory validation gates |
| `init-project` | Setting up or reinitializing a project locally for the first time |

### Code Review & Quality

| Skill | When it activates |
|-------|-------------------|
| `code-review` | Performing a technical pre-commit code review on recently changed files for bugs, security issues, and standards compliance |
| `code-review-fix` | Fixing bugs found during a code review, processing each issue with explanation and test verification |

### Research & Priming

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
| `execution-report` | Generating an implementation report after feature completion, including a coverage gap analysis comparing what was planned vs what was actually tested |
| `system-review` | Performing a meta-level analysis of plan adherence to identify process improvements |

## Skill Interactions

Skills call each other automatically. The diagram below shows which skill invokes which.

```
create-prd
  └─ explore-api          (for each external API that needs research)

plan-feature
  ├─ explore-api          (Phase 3 — for each external API that needs research)
  └─ acceptance-criteria-define   (Phase 7 — after plan is written, to define AC before execution starts)

execute
  ├─ acceptance-criteria-define   (Step 0 — if no acceptance criteria are found in the plan,
  │                                acceptance_criteria.md, or the request; delegates to this
  │                                skill if available, otherwise falls back to inline flow)
  │
  └─ [post-execution, run in parallel as subagents]
       ├─ execution-report         (always — documents what was done, divergences, and coverage gaps)
       ├─ acceptance-criteria-validate  (validates each criterion from the plan against the implementation)
       └─ code-review              (technical review of all changed files for bugs and standards compliance)
```

All post-execution subagents (`execution-report`, `acceptance-criteria-validate`, `code-review`) are launched in parallel after the Output Report is generated. Each is skipped silently if not installed.

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
