---
description: Execute an implementation plan
argument-hint: [path-to-plan]
---

# Execute: Implement from Plan

## Plan to Execute

Read plan file: `$ARGUMENTS`

## Execution Mode

Determine execution strategy based on plan complexity:

- **Simple plans (1-3 independent tasks):** Execute sequentially without team
- **Complex plans (4+ tasks or parallel opportunities):** Use team-based parallel execution

## Team-Based Parallel Execution

### 1. Plan Analysis & Team Setup

**Read and analyze the plan:**
- Read the ENTIRE plan carefully
- Identify all tasks and their dependencies
- Map dependency graph (which tasks block which other tasks)
- Identify validation commands and testing strategy
- Determine wave structure based on dependencies

**Create execution team:**
```
Use TeamCreate to create a team named "execute-{plan-name}"
```

**Team roles:**
- **Team Lead (you):** Orchestrate execution, manage task list, verify integration
- **Implementation Agents:** Execute code tasks in parallel
- **Test Agent:** Create and run tests
- **Validation Agent:** Run validation commands and verify results

### 2. Task Breakdown & Assignment

**Create task list from plan:**

For each task in the plan, use `TaskCreate` to create:
- **Subject:** Brief task title (imperative form)
- **Description:** Full task specification from plan
- **ActiveForm:** Present continuous form for progress tracking
- **Dependencies:** Use `TaskUpdate` to set `addBlockedBy` for dependent tasks

**Task categories:**
- Implementation tasks (code changes)
- Test tasks (test file creation, test implementation)
- Validation tasks (running checks, linting, type checking)
- Integration tasks (ensuring components work together)

**Assign tasks to create waves:**
- **Wave 1:** Tasks with no dependencies (can run immediately)
- **Wave 2:** Tasks that depend only on Wave 1 completion
- **Wave N:** Tasks that depend on previous waves

### 3. Spawn Teammates

**Spawn implementation agents for Wave 1:**

For each independent task group, spawn a `general-purpose` agent:
```
Use Task tool with subagent_type="general-purpose"
```

**Example team structure:**
- `frontend-impl`: Handle frontend implementation tasks
- `backend-impl`: Handle backend implementation tasks
- `test-impl`: Create and run tests
- `validator`: Run validation commands

**Assign tasks to teammates:**
Use `TaskUpdate` with `owner` parameter to assign tasks by ID

### 4. Wave-Based Parallel Execution

**For each wave:**

1. **Start wave:** Assign all wave tasks to available teammates
2. **Monitor progress:** Teammates will send updates when tasks complete
3. **Handle blockers:** If a teammate is blocked, help resolve or reassign
4. **Verify wave completion:** Check all wave tasks are completed
5. **Proceed to next wave:** Unblock dependent tasks and repeat

**Coordination protocol:**
- Teammates mark tasks `in_progress` when starting
- Teammates mark tasks `completed` when done
- Team lead uses `TaskList` to track overall progress
- Team lead assigns next available tasks from subsequent waves
- Communication via `SendMessage` for blockers or questions

### 5. Testing & Validation Phase

**After all implementation waves complete:**

1. **Test agent tasks:**
   - Create all test files from plan
   - Implement all test cases
   - Run tests and report results

2. **Validation agent tasks:**
   - Run linting commands
   - Run type checking
   - Run any custom validation from plan
   - Report all results

**Run in parallel where possible:**
- Independent test suites can run concurrently
- Validation commands that don't conflict can run together

### 6. Integration Verification

**Team lead responsibilities:**

- ✅ Verify all tasks from plan completed
- ✅ Check all tests passing
- ✅ Confirm all validation commands pass
- ✅ Test integration between components
- ✅ Verify code follows project conventions
- ✅ Ensure documentation added/updated

**Integration checks:**
- Do frontend and backend connect properly?
- Are all imports resolving correctly?
- Do modified components still work with existing code?
- Are there any runtime errors?

### 7. Team Shutdown

**Graceful shutdown:**
1. Use `TaskList` to confirm all tasks completed
2. Use `SendMessage` with `type: "shutdown_request"` for each teammate
3. Wait for shutdown confirmations
4. Use `TeamDelete` to clean up team resources

## Sequential Execution (Simple Plans)

For simple plans, execute without team overhead:

### 1. Read and Understand
- Read the ENTIRE plan carefully
- Understand all tasks and their dependencies
- Note the validation commands to run
- Review the testing strategy

### 2. Execute Tasks in Order

For EACH task in "Step by Step Tasks":

#### a. Navigate to the task
- Identify the file and action required
- Read existing related files if modifying

#### b. Implement the task
- Follow the detailed specifications exactly
- Maintain consistency with existing code patterns
- Include proper type hints and documentation
- Add structured logging where appropriate

#### c. Verify as you go
- After each file change, check syntax
- Ensure imports are correct
- Verify types are properly defined

### 3. Implement Testing Strategy
- Create all test files specified in the plan
- Implement all test cases mentioned
- Follow the testing approach outlined
- Ensure tests cover edge cases

### 4. Run Validation Commands

Execute ALL validation commands from the plan in order:

```bash
# Run each command exactly as specified in plan
```

If any command fails:
- Fix the issue
- Re-run the command
- Continue only when it passes

### 5. Final Verification
- ✅ All tasks from plan completed
- ✅ All tests created and passing
- ✅ All validation commands pass
- ✅ Code follows project conventions
- ✅ Documentation added/updated as needed

## Output Report

Provide summary (format depends on execution mode):

### Execution Summary
- **Execution mode:** Sequential or Team-based Parallel
- **Plan source:** Path to plan file
- **Execution time:** Approximate duration
- **Team size:** Number of agents spawned (if parallel)

### Completed Tasks
- List of all tasks completed (by wave if parallel)
- Files created (with paths and which agent created them)
- Files modified (with paths and which agent modified them)
- Task completion order and any blockers encountered

### Tests Added
- Test files created
- Test cases implemented
- Test results (pass/fail counts)
- Coverage information (if applicable)

### Validation Results
```bash
# Output from each validation command
# Include which agent ran each command (if parallel)
```

### Integration Verification
- Component integration status
- Cross-module compatibility checks
- Any integration issues found and resolved

### Performance Metrics (Team-Based Only)
- Number of waves executed
- Tasks per wave breakdown
- Parallelization efficiency
- Bottlenecks identified (tasks that blocked multiple waves)

### Ready for Commit
- ✅ All changes are complete
- ✅ All validations pass
- ✅ Integration verified
- ✅ Ready for `/commit` command

## Best Practices

### When to Use Parallel Execution
- ✅ Plan has 4+ independent tasks
- ✅ Clear frontend/backend separation
- ✅ Multiple test suites that can run independently
- ✅ Long-running validation commands that can be parallelized
- ❌ Avoid for simple plans (overhead not worth it)
- ❌ Avoid if tasks have complex interdependencies (creates bottlenecks)

### Task Dependency Patterns
- **No dependencies:** Perfect for Wave 1 parallel execution
- **Sequential chain:** Must execute in order (A → B → C)
- **Fan-out:** One task enables many (A → B,C,D can parallelize B,C,D)
- **Fan-in:** Many tasks feed into one (A,B,C → D, wait for all before D)

### Communication Patterns
- **Team lead → Teammate:** Task assignment, clarification, shutdown requests
- **Teammate → Team lead:** Completion notifications, blocker reports, questions
- **Teammate → Teammate:** Rare, only for direct coordination needs

### Error Handling
- If a teammate fails a task, team lead should:
  1. Review the error and context
  2. Decide: reassign, help debug, or take over
  3. Update task status and dependencies if needed
  4. Continue wave execution with remaining tasks

### Optimization Tips
- Group related tasks (e.g., all frontend tasks to one agent)
- Assign test creation early so tests can run as soon as code is ready
- Run validation commands in parallel where safe (linting + type checking)
- Keep team size reasonable (3-5 agents max for most plans)

## Notes

- If you encounter issues not addressed in the plan, document them
- If you need to deviate from the plan, explain why and communicate with team
- If tests fail, fix implementation until they pass (coordinate with test agent)
- Don't skip validation steps
- For parallel execution, maintain clear task ownership to avoid conflicts
- Use atomic commits per wave or per major milestone
- Team coordination overhead is real - only parallelize when it provides clear benefit
