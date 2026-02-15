---
description: Analyze implementation against plan for process improvements
---

# System Review

Perform a meta-level analysis of how well the implementation followed the plan and identify process improvements.

## Purpose

**System review is NOT code review.** You're not looking for bugs in the code - you're looking for bugs in the process.

**Your job:**

- Analyze plan adherence and divergence patterns
- Identify which divergences were justified vs problematic
- Surface process improvements that prevent future issues
- Suggest updates to Layer 1 assets (CLAUDE.md, plan templates, commands)

**Philosophy:**

- Good divergence reveals plan limitations → improve planning
- Bad divergence reveals unclear requirements → improve communication
- Repeated issues reveal missing automation → create commands

## Context & Inputs

You will analyze key artifacts:

**PROGRESS.md:**
Read this to find all notes for the current feature including:
- Planning phase notes
- Execution phase notes
- Execution report (what was actually done and why)

**Plan Command:**
Read this to understand the planning process and what instructions guide plan creation.
.claude/commands/plan-feature.md

**Generated Plan:**
Read this to understand what the agent was SUPPOSED to do.
Look up plan file location from PROGRESS.md

**Execute Command:**
Read this to understand the execution process and what instructions guide implementation.
.claude/commands/execute.md

## Analysis Workflow

### Step 0: Read PROGRESS.md

**MANDATORY FIRST STEP:**

1. Read `PROGRESS.md` to find the feature being reviewed
2. Extract the plan file location from planning phase notes
3. Read the execution report section to understand what was actually done
4. Use this context for the remaining analysis steps

### Step 1: Understand the Planned Approach

Read the generated plan (from PROGRESS.md reference) and extract:

- What features were planned?
- What architecture was specified?
- What validation steps were defined?
- What patterns were referenced?

### Step 2: Understand the Actual Implementation

Read the execution report from PROGRESS.md and extract:

- What was implemented?
- What diverged from the plan?
- What challenges were encountered?
- What was skipped and why?

### Step 3: Classify Each Divergence

For each divergence identified in the execution report, classify as:

**Good Divergence ✅** (Justified):

- Plan assumed something that didn't exist in the codebase
- Better pattern discovered during implementation
- Performance optimization needed
- Security issue discovered that required different approach

**Bad Divergence ❌** (Problematic):

- Ignored explicit constraints in plan
- Created new architecture instead of following existing patterns
- Took shortcuts that introduce tech debt
- Misunderstood requirements

### Step 4: Trace Root Causes

For each problematic divergence, identify the root cause:

- Was the plan unclear, where, why?
- Was context missing, where, why?
- Was validation missing, where, why?
- Was manual step repeated, where, why?

### Step 5: Generate Process Improvements

Based on patterns across divergences, suggest:

- **CLAUDE.md updates:** Universal patterns or anti-patterns to document
- **Plan command updates:** Instructions that need clarification or missing steps
- **New commands:** Manual processes that should be automated
- **Validation additions:** Checks that would catch issues earlier

## Output Format

**CRITICAL: Append review to PROGRESS.md, NOT a separate file**

Add a new section to PROGRESS.md under the current feature:

```markdown
---

### System Review
**Generated**: [Current date/time]

#### Meta Information
- Plan reviewed: [path from PROGRESS.md]
- Execution report: PROGRESS.md (this file)
- Date: [current date]
```

### Report Structure:

#### Overall Alignment Score: \_\_/10

Scoring guide:

- 10: Perfect adherence, all divergences justified
- 7-9: Minor justified divergences
- 4-6: Mix of justified and problematic divergences
- 1-3: Major problematic divergences

#### Divergence Analysis

For each divergence from the execution report:

```yaml
divergence: [what changed]
planned: [what plan specified]
actual: [what was implemented]
reason: [agent's stated reason from report]
classification: good ✅ | bad ❌
justified: yes/no
root_cause: [unclear plan | missing context | etc]
```

#### Pattern Compliance

Assess adherence to documented patterns:

- [ ] Followed codebase architecture
- [ ] Used documented patterns (from CLAUDE.md)
- [ ] Applied testing patterns correctly
- [ ] Met validation requirements

#### System Improvement Actions

Based on analysis, recommend specific actions:

**Update CLAUDE.md:**

- [ ] Document [pattern X] discovered during implementation
- [ ] Add anti-pattern warning for [Y]
- [ ] Clarify [technology constraint Z]

**Update Plan Command ($1):**

- [ ] Add instruction for [missing step]
- [ ] Clarify [ambiguous instruction]
- [ ] Add validation requirement for [X]

**Create New Command:**

- [ ] `/[command-name]` for [manual process repeated 3+ times]

**Update Execute Command ($3):**

- [ ] Add [validation step] to execution checklist

#### Key Learnings

**What worked well:**

- [specific things that went smoothly]

**What needs improvement:**

- [specific process gaps identified]

**For next implementation:**

- [concrete improvements to try]
```

**Use Edit tool to append to PROGRESS.md for the current feature section.**

## Important

- **Be specific:** Don't say "plan was unclear" - say "plan didn't specify which auth pattern to use"
- **Focus on patterns:** One-off issues aren't actionable. Look for repeated problems.
- **Action-oriented:** Every finding should have a concrete asset update suggestion
- **Suggest improvements:** Don't just analyze - actually suggest the text to add to CLAUDE.md or commands