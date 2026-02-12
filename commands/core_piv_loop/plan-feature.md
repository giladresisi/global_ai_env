---
description: "Create comprehensive feature plan with deep codebase analysis and research"
argument-hint: [feature-description]
---

# Plan a new task

## ⚠️ CRITICAL INSTRUCTIONS

**DO NOT EXECUTE THE PLAN AFTER CREATING IT**

This command creates a plan file only. After the plan is created:
1. Show the user where the plan was saved
2. Tell them to run `/core_piv_loop:execute .agents/plans/[feature-name].md` to implement it
3. STOP - do not proceed with implementation

---

## Feature: $ARGUMENTS

## Mission

Transform a feature request into a **comprehensive implementation plan** through systematic codebase analysis, external research, and strategic planning.

**Core Principle**: We do NOT write code in this phase. Our goal is to create a context-rich implementation plan that enables one-pass implementation success for ai agents.

**Key Philosophy**: Context is King. The plan must contain ALL information needed for implementation - patterns, mandatory reading, documentation, validation commands - so the execution agent succeeds on the first attempt.

## Planning Process

### Phase 1: Feature Understanding

**Deep Feature Analysis:**

- Extract the core problem being solved
- Identify user value and business impact
- Determine feature type: New Capability/Enhancement/Refactor/Bug Fix
- Assess complexity: Low/Medium/High
- Map affected systems and components

**Create User Story Format Or Refine If Story Was Provided By The User:**

```
As a <type of user>
I want to <action/goal>
So that <benefit/value>
```

### Phase 2: Codebase Intelligence Gathering

**CRITICAL: Check for Similar Features First**

Before designing a new implementation approach, search for features that are similar to what's being requested:

**If you find a similar existing feature:**
1. **STOP and ask the user**: "I found [existing feature] which is similar to [requested feature]. Should I implement [requested feature] using the same approach as [existing feature], or do you want a different implementation strategy?"
2. **Wait for user confirmation** before proceeding with the plan
3. **Document the existing feature's approach** as the baseline pattern to follow

**Common Similar Feature Patterns:**
- Adding a new integration (e.g., "Add Stripe") → Check how existing integrations are implemented
- Adding a new API endpoint → Check existing endpoint implementations
- Adding a new authentication method → Check existing auth implementations
- Adding a new database table/model → Check existing schema patterns
- Adding a new tool/command → Check existing tool implementations
- Adding a new UI component → Check existing component patterns

**Why This Matters:**
- Users typically want consistency across similar features
- Following existing patterns ensures maintainability
- Prevents over-engineering or unnecessary architectural changes
- Saves time by reusing proven approaches

**Example:**
```
User: "Add Gemini model support with ability to switch between Claude and Gemini"

✅ CORRECT:
- Find how Claude is currently integrated (settings, API keys, model configuration)
- Ask: "I see Claude is integrated via ANTHROPIC_API_KEY and llm_provider settings.
  Should I integrate Gemini the same way (GOOGLE_API_KEY + update model selection)?
  Or do you want a different architecture?"

❌ INCORRECT:
- Design a completely new factory pattern for model switching
- Create complex tool-copying mechanisms
- Implement without checking existing integration approach
```

**After User Confirms Approach:**

**Use specialized agents and parallel analysis:**

**1. Project Structure Analysis**

- Detect primary language(s), frameworks, and runtime versions
- Map directory structure and architectural patterns
- Identify service/component boundaries and integration points
- Locate configuration files (pyproject.toml, package.json, etc.)
- Find environment setup and build processes

**2. Pattern Recognition** (Use specialized subagents when beneficial)

- Search for similar implementations in codebase
- Identify coding conventions:
  - Naming patterns (CamelCase, snake_case, kebab-case)
  - File organization and module structure
  - Error handling approaches
  - Logging patterns and standards
- Extract common patterns for the feature's domain
- Document anti-patterns to avoid
- Check CLAUDE.md for project-specific rules and conventions

**3. Dependency Analysis**

- Catalog external libraries relevant to feature
- Understand how libraries are integrated (check imports, configs)
- Find relevant documentation in docs/, ai_docs/, .agents/reference or ai-wiki if available
- Note library versions and compatibility requirements

**4. Testing Patterns**

- Identify test framework and structure (pytest, jest, etc.)
- Find similar test examples for reference
- Understand test organization (unit vs integration)
- Note coverage requirements and testing standards

**5. Integration Points**

- Identify existing files that need updates
- Determine new files that need creation and their locations
- Map router/API registration patterns
- Understand database/model patterns if applicable
- Identify authentication/authorization patterns if relevant

**Clarify Ambiguities:**

- If requirements are unclear at this point, ask the user to clarify before you continue
- Get specific implementation preferences (libraries, approaches, patterns)
- Resolve architectural decisions before proceeding

**Design Decisions (Follow Existing Patterns):**

When designing the approach:
1. **Default to mimicking existing similar features** unless there's a clear reason to diverge
2. **Document why** if you're diverging from existing patterns
3. **Prefer consistency over novelty** - reuse existing architectural patterns
4. **Design for parallel execution** - identify which tasks are independent

Determine:
- Files to create vs modify (follow existing structure)
- Data flow and component interactions (mirror similar features)
- Models/schemas required (match existing naming/structure)
- Service layer components (reuse existing patterns)
- API contracts (consistent with existing endpoints)
- Testing strategy (same approach as similar features)
- Error scenarios and edge cases
- **Parallelization opportunities** - which tasks have no dependencies
- **Interface contracts** - what parallel tasks need from each other
- **Integration points** - where parallel work must synchronize

**Red Flags** (warrant asking user for clarification):
- Introducing new architectural patterns not used elsewhere
- Creating new folder structures when similar features exist
- Using different naming conventions than existing code
- Implementing features in a completely different way than similar existing features

### Phase 3: External Research & Documentation

**Use specialized subagents when beneficial for external research:**

### 3.1 API Research (If Applicable)

**If the feature uses external APIs or unfamiliar SDKs:**

1. **Use `/core_piv_loop:explore-api [API name]` for each API**
   - Wait for research completion
   - Read generated research document: `.agent/research/[api]-research.md`
   - Verify POC tests passed

2. **Critical Questions to Answer:**
   - Is the required feature supported by the API?
   - What's the correct integration pattern?
   - Are there version compatibility issues?
   - What are the event types / response formats?
   - What configuration/setup is required?
   - What are known limitations or gotchas?

3. **Document in Plan:**
   - Add to CONTEXT REFERENCES > External API Research
   - Include integration patterns
   - Note compatibility constraints
   - Reference POC test results

### 3.2 Technology & Pattern Research

**Documentation Gathering:**

- Research latest library versions and best practices
- Find official documentation with specific section anchors
- Locate implementation examples and tutorials
- Identify common gotchas and known issues
- Check for breaking changes and migration guides

**Technology Trends:**

- Research current best practices for the technology stack
- Find relevant blog posts, guides, or case studies
- Identify performance optimization patterns
- Document security considerations

### 3.3 Compile Research References

```markdown
## Relevant Documentation

- [Library Official Docs](https://example.com/docs#section)
  - Specific feature implementation guide
  - Why: Needed for X functionality
- [Framework Guide](https://example.com/guide#integration)
  - Integration patterns section
  - Why: Shows how to connect components
```

### Phase 4: Deep Strategic Thinking

**Think Harder About:**

- How does this feature fit into the existing architecture?
- What are the critical dependencies and order of operations?
- What could go wrong? (Edge cases, race conditions, errors)
- How will this be tested comprehensively?
- What performance implications exist?
- Are there security considerations?
- How maintainable is this approach?

**Design Decisions:**

- Choose between alternative approaches with clear rationale
- Design for extensibility and future modifications
- Plan for backward compatibility if needed
- Consider scalability implications

### Phase 5: Plan Structure Generation

**Create comprehensive plan with the following structure:**

**CRITICAL - Plan Length Constraint:**
- **HARD LIMIT**: Maximum plan length: **500-700 lines**
- Focus on essential information and actionable tasks
- Be concise but complete - prioritize clarity over verbosity
- If approaching the limit, consolidate similar tasks or reduce example code
- Quality over quantity - dense, information-rich content
- **You MUST verify line count after creation and adjust if needed**

Whats below here is a template for you to fill for the implementation agent:

```markdown
# Feature: <feature-name>

The following plan should be complete, but its important that you validate documentation and codebase patterns and task sanity before you start implementing.

Pay special attention to naming of existing utils types and models. Import from the right files etc.

## Feature Description

<Detailed description of the feature, its purpose, and value to users>

## User Story

As a <type of user>
I want to <action/goal>
So that <benefit/value>

## Problem Statement

<Clearly define the specific problem or opportunity this feature addresses>

## Solution Statement

<Describe the proposed solution approach and how it solves the problem>

## Feature Metadata

**Feature Type**: [New Capability/Enhancement/Refactor/Bug Fix]
**Estimated Complexity**: [Low/Medium/High]
**Primary Systems Affected**: [List of main components/services]
**Dependencies**: [External libraries or services required]
**Breaking Changes**: [Yes/No - explain if yes]

---

## CONTEXT REFERENCES

### Relevant Codebase Files IMPORTANT: YOU MUST READ THESE FILES BEFORE IMPLEMENTING!

<List files with line numbers and relevance>

- `path/to/file.py` (lines 15-45) - Why: Contains pattern for X that we'll mirror
- `path/to/model.py` (lines 100-120) - Why: Database model structure to follow
- `path/to/test.py` - Why: Test pattern example

### New Files to Create

- `path/to/new_service.py` - Service implementation for X functionality
- `path/to/new_model.py` - Data model for Y resource
- `tests/path/to/test_new_service.py` - Unit tests for new service

### Relevant Documentation YOU SHOULD READ THESE BEFORE IMPLEMENTING!

- [Documentation Link 1](https://example.com/doc1#section)
  - Specific section: Authentication setup
  - Why: Required for implementing secure endpoints
- [Documentation Link 2](https://example.com/doc2#integration)
  - Specific section: Database integration
  - Why: Shows proper async database patterns

### External API Research (If Applicable)

For each external API:

**API:** [Name] v[Version]
**Research Doc:** `.agent/research/[api]-research.md`
**Documentation:** [Primary URL with specific sections]

**Supported Features:**
- Feature [X]: ✓ Supported via [method]
- Feature [Y]: ✗ Not supported - Alternative: [approach]

**Integration Pattern:**
```[language]
# Initialization
[code example]

# Usage
[code example]

# Error handling
[code example]
```

**Critical Findings:**
- [Finding 1 that impacts implementation]
- [Finding 2 about configuration]
- [Finding 3 about limitations]

**Validation Strategy:**
- POC: [description of proof-of-concept test]
- Expected: [what success looks like]
- Fallback: [alternative if doesn't work]

### Patterns to Follow

<Specific patterns extracted from codebase - include actual code examples from the project>

**Naming Conventions:** (for example)

**Error Handling:** (for example)

**Logging Pattern:** (for example)

**Other Relevant Patterns:** (for example)

---

## PARALLEL EXECUTION STRATEGY

### Dependency Graph

```
┌─────────────────────────────────────────────────────────────┐
│ FOUNDATION LAYER (Phase 1) - Can run in parallel           │
├─────────────────────────────────────────────────────────────┤
│ Task 1.1: [Name]  │ Task 1.2: [Name]  │ Task 1.3: [Name] │
│ Agent: [Role]     │ Agent: [Role]     │ Agent: [Role]    │
│ Deps: None        │ Deps: None        │ Deps: None       │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│ CORE LAYER (Phase 2) - After Foundation                     │
├─────────────────────────────────────────────────────────────┤
│ Task 2.1: [Name]  │ Task 2.2: [Name]  │                   │
│ Agent: [Role]     │ Agent: [Role]     │                   │
│ Deps: 1.1, 1.2    │ Deps: 1.3         │                   │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│ INTEGRATION LAYER (Phase 3) - After Core                    │
├─────────────────────────────────────────────────────────────┤
│ Task 3.1: [Name]                                            │
│ Agent: [Role]                                               │
│ Deps: 2.1, 2.2                                              │
└─────────────────────────────────────────────────────────────┘
```

### Parallelization Opportunities

**Wave 1 - Fully Parallel** (No dependencies):
- Task [X]: [Description] → Agent: [Role]
- Task [Y]: [Description] → Agent: [Role]
- Task [Z]: [Description] → Agent: [Role]

**Wave 2 - Parallel after Wave 1**:
- Task [A]: [Description] → Agent: [Role] → Needs: [Task X]
- Task [B]: [Description] → Agent: [Role] → Needs: [Task Y, Z]

**Wave 3 - Sequential** (Requires Wave 2):
- Task [C]: [Description] → Agent: [Role] → Needs: [Task A, B]

### Team Coordination

**Shared Context Location:** `.agents/plans/shared-context.md`

**Interface Contracts:**

#### Contract 1: [Name]
**Provider:** Task [X] (Agent [Role])
**Consumer:** Task [A] (Agent [Role])
**Interface:**
```[language]
# Definition of what Task X delivers to Task A
```
**Delivery:** After Task [X] validation passes
**Mock for Parallel Work:**
```[language]
# How Task A can continue while Task X is in progress
```

#### Contract 2: [Name]
[Repeat for each dependency between parallel workstreams]

### Synchronization Checkpoints

**Checkpoint 1:** After Wave 1 completes
- **Participants:** Agents working on Tasks X, Y, Z
- **Validation:** All Wave 1 tasks pass individual validation
- **Integration Test:**
```bash
# Command to verify Wave 1 tasks work together
```
- **Blocker Resolution:** If any Wave 1 task fails, Wave 2 cannot start

**Checkpoint 2:** After Wave 2 completes
[Repeat structure]

### Shared Context Management

**Location:** `.agents/plans/shared-context.md`

**Purpose:** Enable parallel agents to coordinate without blocking

**Structure:**
```markdown
# Shared Context for [Feature Name]

## Global Decisions
- [Decision 1]: [Rationale]
- [Decision 2]: [Rationale]

## Naming Conventions
- [Convention 1]
- [Convention 2]

## Interface Registry
| Task | Provides | File Location | Status |
|------|----------|---------------|--------|
| 1.1  | [API]    | path/to/file  | ✅     |
| 1.2  | [Schema] | path/to/file  | 🚧     |

## Blockers & Questions
| Task | Question | Answer | Resolved By |
|------|----------|--------|-------------|
| 2.1  | [Q]      | [A]    | Agent X     |

## Integration Test Results
| Checkpoint | Status | Notes |
|------------|--------|-------|
| Wave 1     | ✅     | All pass |
| Wave 2     | 🚧     | In progress |
```

**Update Protocol:**
- Agents update status when starting/completing tasks
- Agents post questions in Blockers section
- Other agents answer unblocked questions
- Integration test results posted after each checkpoint

### Agent Team Roles

**Backend Specialist:**
- Tasks involving: API endpoints, business logic, database models
- Skills: Python/Node, SQL, API design

**Frontend Specialist:**
- Tasks involving: UI components, state management, user interactions
- Skills: React/Vue, CSS, accessibility

**Test Specialist:**
- Tasks involving: Unit tests, integration tests, E2E tests
- Skills: Testing frameworks, test data generation

**Integration Specialist:**
- Tasks involving: System integration, deployment, configuration
- Skills: DevOps, CI/CD, troubleshooting

**Coordination:**
- Each agent owns their tasks in the wave
- Agents read Shared Context before starting
- Agents update Shared Context when completing
- Agents post blockers immediately when encountered

---

## IMPLEMENTATION PLAN

### Phase 1: Foundation

<Describe foundational work needed before main implementation>

**Parallelization:** ✅ All tasks in this phase can run concurrently

**Tasks:**

- Set up base structures (schemas, types, interfaces)
- Configure necessary dependencies
- Create foundational utilities or helpers

#### Task: [API Name] Integration Setup (If Applicable)
**Purpose:** Configure [API] with proper initialization order and validation

**Steps:**
1. Review API research: `.agent/research/[api]-research.md`
2. Set up configuration (env vars, etc.)
3. Implement initialization with correct import order
4. Add startup validation (API key test, connection test)
5. Create POC integration test

**Validation:**
```bash
# Command to verify setup works
[validation command]
```
**Expected:** [what success output looks like]

**Reference:** [link to API research findings]

### Phase 2: Core Implementation

<Describe the main implementation work>

**Tasks:**

- Implement core business logic
- Create service layer components
- Add API endpoints or interfaces
- Implement data models

### Phase 3: Integration

<Describe how feature integrates with existing functionality>

**Tasks:**

- Connect to existing routers/handlers
- Register new components
- Update configuration files
- Add middleware or interceptors if needed

### Phase 4: Testing & Validation

<Describe testing approach>

**Tasks:**

- Implement unit tests for each component
- Create integration tests for feature workflow
- Add edge case tests
- Validate against acceptance criteria

---

## STEP-BY-STEP TASKS

IMPORTANT: Tasks are organized by execution waves for parallel execution. Tasks within the same wave can be executed concurrently by different agents.

### Task Format Guidelines

Use information-dense keywords for clarity:

- **CREATE**: New files or components
- **UPDATE**: Modify existing files
- **ADD**: Insert new functionality into existing code
- **REMOVE**: Delete deprecated code
- **REFACTOR**: Restructure without changing behavior
- **MIRROR**: Copy pattern from elsewhere in codebase

### Task Metadata Format

Each task includes:
- **WAVE**: Execution wave number (tasks in same wave = parallel)
- **AGENT_ROLE**: Suggested agent specialization
- **DEPENDS_ON**: Task IDs that must complete first (empty = no deps)
- **BLOCKS**: Task IDs that wait for this task
- **PROVIDES**: What this task delivers for downstream tasks
- **INTEGRATION_TEST**: How to verify this integrates with parallel tasks

---

### WAVE 1: Foundation Layer (Fully Parallel)

#### Task 1.1: {ACTION} {target_file}

- **WAVE**: 1
- **AGENT_ROLE**: [backend/frontend/test/infra]
- **DEPENDS_ON**: []
- **BLOCKS**: [Task IDs that need this]
- **PROVIDES**: [Interface/contract for downstream tasks]
- **IMPLEMENT**: {Specific implementation detail}
- **PATTERN**: {Reference to existing pattern - file:line}
- **IMPORTS**: {Required imports and dependencies}
- **GOTCHA**: {Known issues or constraints to avoid}
- **VALIDATE**: `{executable validation command}`
- **INTEGRATION_TEST**: `{command to test with other Wave 1 tasks}`

#### Task 1.2: {ACTION} {target_file}

- **WAVE**: 1
- **AGENT_ROLE**: [role]
- **DEPENDS_ON**: []
- **BLOCKS**: [Task IDs]
- **PROVIDES**: [What it delivers]
- **IMPLEMENT**: {Details}
- **PATTERN**: {Reference}
- **VALIDATE**: `{command}`
- **INTEGRATION_TEST**: `{command}`

#### Task 1.3: {ACTION} {target_file}

[Repeat format for all Wave 1 tasks]

**Wave 1 Integration Checkpoint:**
```bash
# Run after ALL Wave 1 tasks complete
# Verifies that all parallel tasks integrate correctly
[integration test command]
```

---

### WAVE 2: Core Layer (Parallel after Wave 1)

**Prerequisites:** All Wave 1 tasks must pass validation and integration tests

#### Task 2.1: {ACTION} {target_file}

- **WAVE**: 2
- **AGENT_ROLE**: [role]
- **DEPENDS_ON**: [Task 1.1, Task 1.2]
- **BLOCKS**: [Task IDs]
- **PROVIDES**: [What it delivers]
- **IMPLEMENT**: {Details}
- **PATTERN**: {Reference}
- **USES_FROM_WAVE_1**: Task 1.1 provides [X], Task 1.2 provides [Y]
- **VALIDATE**: `{command}`
- **INTEGRATION_TEST**: `{command with Wave 2 peers}`

#### Task 2.2: {ACTION} {target_file}

- **WAVE**: 2
- **AGENT_ROLE**: [role]
- **DEPENDS_ON**: [Task 1.3]
- **BLOCKS**: [Task IDs]
- **PROVIDES**: [What it delivers]
- **IMPLEMENT**: {Details}
- **VALIDATE**: `{command}`

**Wave 2 Integration Checkpoint:**
```bash
# Run after ALL Wave 2 tasks complete
[integration test command]
```

---

### WAVE 3: Integration Layer (Sequential)

**Prerequisites:** All Wave 2 tasks complete

#### Task 3.1: {ACTION} {target_file}

- **WAVE**: 3
- **AGENT_ROLE**: [integration-specialist]
- **DEPENDS_ON**: [Task 2.1, Task 2.2]
- **BLOCKS**: []
- **PROVIDES**: [Final integrated feature]
- **IMPLEMENT**: {Integration logic}
- **VALIDATE**: `{command}`
- **INTEGRATION_TEST**: `{end-to-end test}`

**Final Integration Checkpoint:**
```bash
# Verifies entire feature works end-to-end
[full feature test command]
```

<Continue with additional tasks organized by wave...>

---

## TESTING STRATEGY

<Define testing approach based on project's test framework and patterns discovered during research>

### Unit Tests

<Scope and requirements based on project standards>

Design unit tests with fixtures and assertions following existing testing approaches

### Integration Tests

<Scope and requirements based on project standards>

### Tool Registration Tests (If Applicable)

**When**: Creating tools with decorators like `@agent.tool`, `@mcp.tool`, or similar registration patterns

**Scope**: Tool registration, availability, and invocation

**Critical Tests Required**:
1. **Tool Registration Test**: Verify tool is registered in the tool registry
2. **Singleton Usage Test**: Verify endpoints/services use singleton instances with tools
3. **E2E Invocation Test**: Verify tool can be invoked through standard interfaces
4. **Tool Availability Test**: Verify tool is accessible in all required modes

**Pattern to Follow** (adapt to your framework):
```[language]
# Test that tool is registered
def test_tool_is_registered():
    """Verify tool is registered in tool registry."""
    registry = get_tool_registry()
    assert "[your_tool_name]" in registry.list_tools()

# Test that singleton pattern is used
def test_service_uses_singleton():
    """Verify service uses singleton instance with tools."""
    from app.service import service_instance, singleton_instance
    assert service_instance is singleton_instance

# Test end-to-end invocation
def test_tool_invocation():
    """Verify tool can be invoked through standard interface."""
    result = invoke_tool("[your_tool_name]", {"param": "value"})
    assert result.success
```

**Common Pitfall**: Using factory functions creates instances WITHOUT tools. Always use singleton instances for tool-enabled services.

### Edge Cases

<List specific edge cases that must be tested for this feature>

---

## VALIDATION COMMANDS

<Define validation commands based on project's tools discovered in Phase 2>

Execute every command to ensure zero regressions and 100% feature correctness.

### Level 0: API Integration Validation (If Applicable)

For each external API:

#### [API Name] Validation
```bash
# Verify API configuration
[command to check env vars, config]

# Test API connectivity
[command to test API key, connection]

# Verify response format
[command to test actual API call]
```

**Expected Output:**
- Configuration: [what should be set]
- Connection: [success indicators]
- Response: [expected format/data]

**If Failed:**
- Check: [troubleshooting steps]
- Fallback: [alternative approach]

### Level 1: Syntax & Style

<Project-specific linting and formatting commands>

### Level 2: Unit Tests

<Project-specific unit test commands>

### Level 3: Integration Tests

<Project-specific integration test commands>

### Level 4: Manual Validation

<Feature-specific manual testing steps - API calls, UI testing, etc.>

### Level 5: Additional Validation (Optional)

<MCP servers or additional CLI tools if available>

---

## ACCEPTANCE CRITERIA

<List specific, measurable criteria that must be met for completion>

- [ ] Feature implements all specified functionality
- [ ] All validation commands pass with zero errors
- [ ] Unit test coverage meets requirements (80%+)
- [ ] Integration tests verify end-to-end workflows
- [ ] Code follows project conventions and patterns
- [ ] No regressions in existing functionality
- [ ] Documentation is updated (if applicable)
- [ ] Performance meets requirements (if applicable)
- [ ] Security considerations addressed (if applicable)

---

## COMPLETION CHECKLIST

- [ ] All tasks completed in order
- [ ] Each task validation passed immediately
- [ ] All validation commands executed successfully
- [ ] Full test suite passes (unit + integration)
- [ ] No linting or type checking errors
- [ ] Manual testing confirms feature works
- [ ] Acceptance criteria all met
- [ ] Code reviewed for quality and maintainability

---

## NOTES

<Additional context, design decisions, trade-offs>
```

## Output Format

**Filename**: `.agents/plans/{kebab-case-descriptive-name}.md`

- Replace `{kebab-case-descriptive-name}` with short, descriptive feature name
- Examples: `add-user-authentication.md`, `implement-search-api.md`, `refactor-database-layer.md`

**Directory**: Create `.agents/plans/` if it doesn't exist

---

## POST-PLANNING VERIFICATION

### Save and Verify Line Count

**REQUIRED STEPS AFTER CREATING PLAN:**

```bash
mkdir -p .agents/plans
# Plan saved to: .agents/plans/[feature-name].md

# CRITICAL: Verify line count is 500-700 lines
wc -l .agents/plans/[feature-name].md

# If not in range, adjust the plan NOW before reporting to user
```

**Line Count Adjustment**:
- If < 500 lines: Add more examples, edge cases, validation details, or expand on patterns
- If > 700 lines: Consolidate tasks, remove redundant explanations, trim verbose sections, use more concise language

**Final Checklist**:
- [ ] Plan is 500-700 lines (VERIFY with `wc -l` - this is MANDATORY)
- [ ] Feature name is kebab-case
- [ ] File paths are exact and correct
- [ ] Validation commands are copy-pasteable and executable
- [ ] Code examples are from actual codebase (not generic)
- [ ] Tasks are in dependency order
- [ ] Another agent could execute without conversation context
- [ ] All patterns reference specific file:line numbers
- [ ] **Tasks organized into parallel execution waves**
- [ ] **Each task has WAVE, DEPENDS_ON, and AGENT_ROLE metadata**
- [ ] **Interface contracts defined between dependent tasks**
- [ ] **Integration checkpoints specified for each wave**
- [ ] **Shared context structure documented**
- [ ] **At least 30% of tasks can execute in parallel** (or explain why not)

---

## Quality Criteria

### Context Completeness ✓

- [ ] All necessary patterns identified and documented
- [ ] External library usage documented with links
- [ ] Integration points clearly mapped
- [ ] Gotchas and anti-patterns captured
- [ ] Every task has executable validation command

### Implementation Ready ✓

- [ ] Another developer could execute without additional context
- [ ] Tasks ordered by dependency (can execute top-to-bottom)
- [ ] Each task is atomic and independently testable
- [ ] Pattern references include specific file:line numbers

### Parallel Execution Ready ✓

- [ ] Tasks organized into execution waves
- [ ] Dependencies between tasks explicitly documented
- [ ] Interface contracts defined for cross-task communication
- [ ] Synchronization checkpoints identified
- [ ] Shared context structure documented
- [ ] Agent role assignments suggested
- [ ] Mock strategies provided for blocked parallel work
- [ ] Integration tests defined for each wave

### Pattern Consistency ✓

- [ ] Tasks follow existing codebase conventions
- [ ] New patterns justified with clear rationale
- [ ] No reinvention of existing patterns or utils
- [ ] Testing approach matches project standards

### Information Density ✓

- [ ] No generic references (all specific and actionable)
- [ ] URLs include section anchors when applicable
- [ ] Task descriptions use codebase keywords
- [ ] Validation commands are non-interactive and executable
- [ ] Plan length is 500-700 lines maximum (concise but complete)

## Success Metrics

**One-Pass Implementation**: Execution agent can complete feature without additional research or clarification

**Validation Complete**: Every task has at least one working validation command

**Context Rich**: The Plan passes "No Prior Knowledge Test" - someone unfamiliar with codebase can implement using only Plan content

**Confidence Score**: #/10 that execution will succeed on first attempt

---

## FINAL REPORT

After creating the plan, output this report to the user and STOP (do not execute):

```
✅ Plan Created Successfully

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 Feature: [Feature name]
📄 Plan Location: .agents/plans/[feature-name].md
📏 Plan Length: [XXX] lines (target: 500-700 lines)
⚡ Complexity: [Low/Medium/High]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📝 Summary:
[2-3 sentence summary of feature and approach]

⚡ Parallel Execution:
- Waves: [Number] execution waves
- Parallelization: [Number] tasks can run concurrently
- Max Speedup: [X]x with [N] parallel agents
- Sequential Tasks: [Number] must run sequentially

⚠️  Key Risks:
[List 2-4 key risks with mitigations]

🔍 Similar Features Found:
[List similar features that were used as patterns, or "None - new pattern"]

📊 Estimated Tasks: [Number] total ([X] parallel, [Y] sequential)
🎯 Confidence Score: [X]/10 for one-pass success
👥 Recommended Team Size: [Number] agents for optimal parallelization

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚀 To execute this plan, run:

    /core_piv_loop:execute .agents/plans/[feature-name].md

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**STOP HERE - DO NOT PROCEED WITH EXECUTION**
