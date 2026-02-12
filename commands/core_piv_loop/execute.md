# Execute: Implement from Plan

## Plan to Execute

Read plan file and execute all tasks according to the plan's specifications.

---

## Execution Mode Decision Protocol

**REQUIRED:** Before executing, analyze the plan and explicitly state your execution mode decision.

### Step 1: Parse Plan for Parallel Signals

Read the plan and check for these explicit parallel execution indicators:

1. **"Parallel Execution Strategy" section** - Strongest signal
   - If present, extract the team structure, agent roles, and phase definitions
   - This section takes precedence over generic heuristics

2. **"Parallel Execution:" metadata** - Top-level plan metadata
   - Look for: `**Parallel Execution:** ✅ **Yes**` or similar
   - Indicates plan author explicitly designed for parallelism

3. **"Team Structure" section** - Defined agent assignments
   - Lists specific agents (e.g., "Agent 1 (Backend-API)", "Agent 2 (Frontend)")
   - Shows clear role separation suitable for parallel work

4. **"Execution Order" with phases** - Multi-phase execution plan
   - Shows dependencies between phases
   - Indicates which tasks can run concurrently

### Step 2: Evaluate Complexity and Task Count

If no explicit parallel signals found, fall back to task analysis:

- Count total tasks in "Step by Step Tasks" section
- Identify task dependencies (sequential chains vs. independent tasks)
- Assess task domains (frontend/backend separation enables parallelism)
- Consider execution time (long-running tasks benefit from parallelism)

### Step 3: Output Execution Mode Decision

**MANDATORY OUTPUT:** Print your decision using this template:

\`\`\`
## EXECUTION MODE DECISION

**Plan:** [plan name]
**Decision:** [TEAM-BASED PARALLEL | SEQUENTIAL]

**Reasoning:**
- Parallel signals found: [Yes/No]
  - Has "Parallel Execution Strategy" section: [Yes/No]
  - Has explicit team structure: [Yes/No]
  - Has phase-based execution order: [Yes/No]
- Task count: [N tasks]
- Task dependencies: [describe dependency structure]
- Complexity assessment: [Simple/Medium/Complex]

**Conclusion:**
[1-2 sentences explaining why you chose team-based or sequential execution]

**Team structure (if parallel):**
[List agent roles and responsibilities from plan, or design team if not specified]
\`\`\`

### Step 4: Proceed with Chosen Strategy

Based on your decision, jump to either:
- **Team-Based Parallel Execution** section (if team-based)
- **Sequential Execution** section (if sequential)

---

## Decision Criteria (Priority Order)

Use these criteria in priority order. Higher priority criteria override lower ones.

### Priority 1: Explicit Parallel Strategy in Plan (HIGHEST)
- ✅ **Use Team-Based:** Plan has "Parallel Execution Strategy" section
- ✅ **Use Team-Based:** Plan has "Team Structure" with named agents
- ✅ **Use Team-Based:** Plan has "Execution Order" with parallel phases

### Priority 2: Plan Metadata
- ✅ **Use Team-Based:** Complexity marked as 🔴 Complex or ⚠️ Medium with 4+ tasks
- ✅ **Use Team-Based:** Parallel Execution metadata says "✅ Yes"
- ⚠️ **Consider Team-Based:** Complexity marked as ⚠️ Medium with frontend + backend split

### Priority 3: Task Analysis (Fallback)
- ✅ **Use Team-Based:** 4+ tasks with clear domain separation (frontend/backend)
- ✅ **Use Team-Based:** 3+ independent long-running tasks (e.g., multiple test suites)
- ❌ **Use Sequential:** 1-3 simple tasks with sequential dependencies
- ❌ **Use Sequential:** All tasks tightly coupled (changes require coordination)

### Priority 4: Efficiency Tradeoff
- ⚠️ **Avoid Team-Based:** Team overhead > time savings (very simple plans)
- ✅ **Use Team-Based:** Estimated sequential time > 2 hours
- ✅ **Use Team-Based:** Validation can run in parallel with implementation

---

## Execution Mode

After completing the Execution Mode Decision Protocol above, you will execute using one of these strategies:

- **Sequential Execution:** For simple plans (see criteria in Decision Protocol)
- **Team-Based Parallel Execution:** For complex plans or plans with explicit parallel strategy

**NOTE:** If the plan includes a "Parallel Execution Strategy" section, you MUST use team-based execution, even if task count is low. The plan author has explicitly designed the work for parallelism.

---

## Sequential Execution

For simple plans (1-3 independent tasks, no parallel execution strategy):

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

After completing implementation tasks:

- Create all test files specified in the plan
- Implement all test cases mentioned
- Follow the testing approach outlined
- Ensure tests cover edge cases

### 4. Run Validation Commands

Execute ALL validation commands from the plan in order.

If any command fails:
- Fix the issue
- Re-run the command
- Continue only when it passes

### 5. Final Verification

Before completing:

- ✅ All tasks from plan completed
- ✅ All tests created and passing
- ✅ All validation commands pass
- ✅ Code follows project conventions
- ✅ Documentation added/updated as needed

---

## Team-Based Parallel Execution

For complex plans (4+ tasks, parallel opportunities, or explicit parallel strategy):

### 1. Plan Analysis & Team Setup

#### a. Read and Parse Plan
- Read ENTIRE plan carefully
- If plan has "Parallel Execution Strategy" section, extract team structure and phases
- If no explicit team structure, analyze tasks to design team composition
- Identify task dependencies and blocking relationships
- Note validation commands and testing strategy

#### b. Create Team
Use TeamCreate tool:
- team_name: "execute-{plan-name}"
- description: "Executing {plan-name} with parallel agents"

### 2. Task Breakdown & Assignment

#### a. Create Tasks from Plan
For each task in the plan, create using TaskCreate

#### b. Map Tasks to Agents

**Common Team Structures:**

**3-Agent Team (Frontend/Backend split):**
- Agent 1 (Backend-API): API endpoints, routers, configuration
- Agent 2 (Backend-Core): Core services, business logic
- Agent 3 (Frontend): UI components, hooks, types

**4-Agent Team (Full-stack + Database):**
- Agent 1 (Database): Migrations, schema changes
- Agent 2 (Backend-Processing): Core services and processing
- Agent 3 (Backend-API): API layer
- Agent 4 (Frontend): UI and integration

### 3. Spawn Teammates & Execute in Waves

Create general-purpose agents for each role and execute tasks in dependency-based waves.

### 4. Integration Verification & Team Shutdown

Verify all tasks complete, tests pass, then gracefully shut down team.

---

## Best Practices

### Parsing Plan for Execution Guidance

**Always check for explicit parallel execution sections first:**

1. **Read plan header** - Look for metadata like complexity markers and parallel execution flags
2. **Search for section headers** - "Parallel Execution Strategy", "Team Structure", "Execution Order"
3. **Extract team definitions** - If plan defines agents, use those exact role names
4. **Follow plan's phase structure** - If plan defines Phase 1/2/3, use those as waves
5. **Respect plan dependencies** - If plan says "Agent 2 depends on Agent 1", enforce blocking

**Example: Plan with Explicit Strategy**

If plan contains "Parallel Execution Strategy" section with 3 agents defined, you MUST:
1. Create team with 3 agents using exact roles from plan
2. Assign tasks according to plan's agent mapping
3. Follow plan's execution order (phases)

### When to Use Parallel Execution

**Use team-based parallel execution when:**
- ✅ Plan explicitly defines parallel execution strategy
- ✅ 4+ tasks with clear domain separation
- ✅ Frontend and backend work can proceed independently
- ✅ Multiple long-running test suites can run concurrently

**Use sequential execution when:**
- ✅ 1-3 simple, fast tasks
- ✅ Tasks have tight sequential dependencies
- ✅ All work affects the same files (high conflict risk)

---

## Decision Examples

### Example 1: Plan with Explicit Parallel Strategy

**Plan Structure:**
- Has "Parallel Execution Strategy" section ✅
- Defines 3 agents with specific roles ✅
- Shows phase-based execution order ✅

**Decision:**
TEAM-BASED PARALLEL - Plan explicitly defines 3-agent team structure. Must use team-based execution as plan author designed for parallelism.

### Example 2: Simple Sequential Plan

**Plan Structure:**
- No "Parallel Execution Strategy" section ❌
- 2 tasks with sequential dependency
- Complexity: Simple

**Decision:**
SEQUENTIAL - No parallel signals, only 2 tightly coupled tasks. Team overhead not justified.

### Example 3: Complex Plan (Design Team Yourself)

**Plan Structure:**
- No explicit parallel strategy ❌
- 8 tasks: 4 frontend + 4 backend
- Complexity: Complex
- Clear domain separation

**Decision:**
TEAM-BASED PARALLEL - Clear frontend/backend separation justifies team execution. Design 3-agent team to parallelize work.

---

## Output Report

After execution completes, provide summary:

### Completed Tasks
- List of all tasks completed
- Files created/modified with paths

### Tests Added
- Test files created
- Test results

### Validation Results
Output from validation commands

### Execution Metrics (if team-based)
- Number of agents used
- Estimated time saved vs sequential
- Coordination overhead

### Ready for Commit
- Confirm all changes complete
- Confirm all validations pass
