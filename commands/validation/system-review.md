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

**CRITICAL: Create review in `.agents/system-reviews/` folder**

### Step 1: Determine Feature Name

Extract the feature/module name from PROGRESS.md or the execution report.
Example: "module-5-multi-format-enhancement" from execution report path

### Step 2: Create Detailed System Review

**Create file:** `.agents/system-reviews/{feature-name}.md`

**Use Write tool to create this file with the following structure:**

```markdown
# System Review: [Feature Name]

**Generated:** [Current date/time]

## Meta Information
- Plan reviewed: [path to plan file]
- Execution report: `.agents/execution-reports/{feature-name}.md`
- Executor: [team-based/sequential/etc]
- Date: [current date]

## Overall Alignment Score: X/10

**Scoring rationale:**
- [breakdown of scoring]

[Summary paragraph explaining overall alignment]

## Divergence Analysis

### Divergence 1: [Title]
```yaml
divergence: [what changed]
planned: [what plan specified]
actual: [what was implemented]
reason: [agent's stated reason from report]
classification: good ✅ | bad ❌ | environmental ⚠️
justified: yes/no
root_cause: [unclear plan | missing context | etc]
impact: [describe impact]
```

**Assessment:** [detailed analysis of this divergence]

[Repeat for each divergence]

## Pattern Compliance

- ✅/❌ Followed codebase architecture (details)
- ✅/❌ Used documented patterns (from CLAUDE.md) (details)
- ✅/❌ Applied testing patterns correctly (details)
- ✅/❌ Met validation requirements (details)

**Exemplary/Concerning:** [highlight notable observations]

## System Improvement Actions

### Update CLAUDE.md:

- [x]/[ ] Document [pattern X] discovered during implementation:
  ```markdown
  [Actual suggested text to add]
  ```

- [x]/[ ] Add anti-pattern warning for [Y]:
  ```markdown
  [Actual suggested text to add]
  ```

### Update [command-name] command:

- [ ] Add [specific instruction]:
  ```markdown
  [Actual suggested text to add]
  ```

### Create New Command:

- [ ] `/[command-name]` for [manual process]
  - [Description of command purpose]

## Key Learnings

### What worked well:

1. **[Category]:** [specific observation]
2. **[Category]:** [specific observation]

### What needs improvement:

1. **[Category]:** [specific gap identified]
2. **[Category]:** [specific gap identified]

### For next implementation:

1. **[Action]:** [concrete improvement to try]
2. **[Action]:** [concrete improvement to try]

## Process Quality Assessment

**Planning Phase:** ✅/⚠️/❌ [Rating]
- [observations and details]

**Execution Phase:** ✅/⚠️/❌ [Rating]
- [observations and details]

**Validation Phase:** ✅/⚠️/❌ [Rating]
- [observations and details]

**Documentation:** ✅/⚠️/❌ [Rating]
- [observations and details]

## Recommended CLAUDE.md Additions

Based on patterns discovered during this implementation, add these sections:

### 1. [Pattern Category]
```markdown
[Complete markdown text to add to CLAUDE.md]
```

### 2. [Pattern Category]
```markdown
[Complete markdown text to add to CLAUDE.md]
```

---

## Conclusion

**Overall Assessment:** [comprehensive summary]

**Process Improvements Identified:**
- [improvement 1] ✅/[ ] (status)
- [improvement 2] ✅/[ ] (status)

**Recommended Actions:**
1. [action with priority]
2. [action with priority]

**Ready for Next Module:** Yes/No - [reasoning]
```

### Step 3: Update PROGRESS.md

**Use Edit tool to add/update in PROGRESS.md under the current feature:**

```markdown
### Reports Generated

**Execution Report:** `.agents/execution-reports/{feature-name}.md`
- [Brief summary of execution report]

**System Review:** `.agents/system-reviews/{feature-name}.md`
- Alignment score: X/10
- Divergence analysis (X identified: Y justified, Z problematic)
- Process improvements and CLAUDE.md updates [completed/recommended]
- Key learnings and recommendations for next implementation
```

**Do NOT include the full review content in PROGRESS.md - only the reference.**

## Important

- **Be specific:** Don't say "plan was unclear" - say "plan didn't specify which auth pattern to use"
- **Focus on patterns:** One-off issues aren't actionable. Look for repeated problems.
- **Action-oriented:** Every finding should have a concrete asset update suggestion
- **Suggest improvements:** Don't just analyze - actually suggest the text to add to CLAUDE.md or commands