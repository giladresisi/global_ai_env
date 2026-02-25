---
name: core_piv_loop:plan-feature
description: Use when creating a comprehensive implementation plan for a new feature including codebase analysis, API research, and parallel execution strategy
---

# Plan a new task

## ⚠️ CRITICAL INSTRUCTIONS

**DO NOT EXECUTE THE PLAN AFTER CREATING IT.**

After the plan is created:
1. Show the user where the plan was saved
2. Tell them to run `/core_piv_loop:execute .agents/plans/[feature-name].md`
3. STOP

**Execution agent rules** (include verbatim in every generated plan):
- Make ALL code changes required by the plan
- Delete debug logs added during execution (keep pre-existing ones)
- Leave ALL changes UNSTAGED — do NOT run `git add` or `git commit`

**🚨 WRITE PLAN TO FILE ONLY — NEVER TO CLI 🚨**
- Write to `.agents/plans/[feature-name].md` using Write/Edit tool
- Do NOT output plan content in your response
- CLI output: summary only (2–3 sentences), questions, confirmation, final report

---

## Feature: $ARGUMENTS

## Step 0: Log Planning Start

Before writing anything, **check if `PROGRESS.md` already has a relevant section for this feature** — especially if this skill was triggered by referencing something in `PROGRESS.md` (e.g. the user pointed at a section, or the feature description matches an existing entry).

**If a relevant section already exists:** update its top fields only (status, add Plan File line). Do NOT create a duplicate entry.

```markdown
**Status**: ✅ Planned
**Plan File**: .agents/plans/[feature-name].md
```

**If no relevant section exists:** add a new entry using the template below.

```markdown
## Feature: [Feature Name]
### Planning Phase
**Status**: In Progress
**Started**: [date]
**Plan File**: .agents/plans/[feature-name].md
```

Use Write if `PROGRESS.md` doesn't exist, Edit if it does.

---

## Planning Process

### Phase 1: Feature Understanding

- Extract core problem, user value, feature type (New Capability/Enhancement/Refactor/Bug Fix), complexity
- Map affected systems
- Draft user story: As a / I want / So that

### Phase 2: Codebase Intelligence

**Check for similar features first.** If found, use AskUserQuestion before proceeding.

Check existing patterns for: integrations, endpoints, auth, schema, tools, UI components.

Gather in parallel:
1. Project structure, frameworks, service boundaries, config files
2. Naming conventions, error handling, logging patterns, CLAUDE.md
3. External libraries, docs/, ai_docs/, .agents/reference
4. Test framework, organization, coverage requirements
5. Integration points: routers, models, auth patterns

**Use AskUserQuestion if ANY of these exist:**
- Unclear, ambiguous, or contradictory requirements
- Multiple valid approaches without clear preference
- Conflicts between request and existing patterns
- Uncertainty about data models, schemas, or interfaces

Default to existing patterns. Document any divergences. Design for parallel execution.

### Phase 3: External Research

**APIs** (if applicable): Run `/core_piv_loop:explore-api [name]` for each.
Verify: features available, version compatible, rate limits sufficient, ToS permits use, auth accessible, no blockers.

**Dependencies**: Test in isolated venv first. Check conflict tree with `pipdeptree`/`npm list`. Document compatible versions and known conflicts.

**Technology**: Latest versions, official docs (with section anchors), common gotchas, breaking changes.

### Phase 4: Strategic Design

Think through:
- How does this fit existing architecture?
- Critical dependencies and order of operations?
- What could go wrong? (edge cases, race conditions, errors)
- Performance, security, maintainability implications?

### Phase 5: Write the Plan

Read `~/.claude/skills/plan-feature/PLAN_TEMPLATE.md` and use it as the structure for the output plan file, filling every section with feature-specific content.

**Output**: `.agents/plans/{kebab-case-name}.md`
**Length**: 500–700 lines — verify with `wc -l` and adjust

### Phase 6: Coverage Review Pass

Run automatically after plan is written. No user input needed unless a gap can't be resolved.

1. **Map new code paths** — for every file created/modified: functions, branches, async flows, error paths, API surface, data mutations
2. **Map project impact** — existing files that import/call changed code; existing tests that may need updating after the change
3. **Gap analysis** — for each path, verify a test in the plan covers it: ✅ Covered or ⚠️ Gap
4. **Fill gaps** — add automated test by default; if genuinely impossible to automate, add manual test with one-sentence justification ("requires physical hardware", "CAPTCHA blocks automation"). "Hard to automate" and "requires a browser" are NOT valid reasons — use Playwright.
5. **Re-verify** — repeat until all paths are ✅ or documented manual-only. Update Test Automation Summary in plan.

---

## Post-Planning Verification

```bash
test -f .agents/plans/[feature-name].md && echo "✅ Plan exists" || echo "❌ Missing"
wc -l .agents/plans/[feature-name].md
```

- [ ] Plan written to file (not CLI)
- [ ] File exists and is 500–700 lines
- [ ] Coverage Review Pass complete (Steps 1–5)
- [ ] All ambiguities resolved via AskUserQuestion
- [ ] Tasks in parallel execution waves with WAVE, DEPENDS_ON, AGENT_ROLE
- [ ] Interface contracts defined; 30%+ tasks parallelizable (or explain why not)
- [ ] Every test marked ✅/⚠️ with tool, file path, and run command
- [ ] Manual tests have automation-impossibility justification
- [ ] Test Automation Summary updated after gap-filling

---

## Final Report

Output to CLI after saving the plan. **Do NOT include plan content in this message.**

```
✅ Plan Created

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 Feature: [name]
📄 Plan: .agents/plans/[feature-name].md
📏 Lines: [n] (target: 500–700)
⚡ Complexity: [Low/Medium/High]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📝 [2–3 sentence summary of feature and approach]

⚡ Parallel Execution:
- Waves: [#] | Concurrent tasks: [#] | Max speedup: [x]x | Sequential: [#]

🧪 Coverage Summary:
- New code paths: [#]/[#] covered ([XX]%)
- Existing code re-validated: [#] areas, [#] tests added/updated
- Automated: [#] ([XX]%) — [tools used]
- Manual: [#] ([XX]%) — [one-line reason each, or "None"]
- Gaps remaining: [# or "None"] — [reason for each, if any]

⚠️  Risks: [2–4 with mitigations]
🔍 Patterns used: [similar features referenced, or "None — new pattern"]
📊 Tasks: [#] total ([x] parallel, [y] sequential)
🎯 Confidence: [x]/10 for one-pass success
👥 Optimal team: [#] agents

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚀 /core_piv_loop:execute .agents/plans/[feature-name].md

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**STOP — DO NOT EXECUTE**
