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

**⚠️ EXECUTION AGENT INSTRUCTION:**

When this plan is executed, the execution agent MUST:
- Make ALL code changes required by the plan
- Delete any debug logs added during execution (console.log, print statements, etc.) that were NOT explicitly requested
- Keep pre-existing debug logs that were already in the codebase
- Leave ALL changes UNSTAGED (do NOT commit)
- Allow the user to review changes with `git diff` before committing
- This instruction will be included in every plan file generated

**⚠️ PLAN OUTPUT REQUIREMENT - CRITICAL:**

**🚨 MANDATORY: Write plan to FILE ONLY - NEVER to CLI 🚨**

**ALL plan content MUST be written to `.agents/plans/[feature-name].md` - NOT to the CLI**

**PROHIBITED:**
- ❌ Do NOT output the plan content as a message to the user
- ❌ Do NOT write plan sections in your response text
- ❌ Do NOT show the plan in markdown code blocks
- ❌ Do NOT copy/paste plan content to CLI

**REQUIRED:**
- ✅ ONLY write the plan to the file using the Write or Edit tool
- ✅ When updating an existing plan, use Edit tool to modify the file directly
- ✅ Verify file was written successfully before proceeding

**What to output to CLI:**
- ✅ Summary of what you're planning (2-3 sentences, NOT the full plan)
- ✅ Questions using AskUserQuestion (if needed)
- ✅ Confirmation that plan was saved and where
- ✅ Final report (see FINAL REPORT section)

**Why this matters:** Plan files can be 500-700 lines. Outputting to CLI wastes tokens, context, and user time. The file is the source of truth.

---

## Feature: $ARGUMENTS

## Step 0: Log Planning Start to PROGRESS.md

**MANDATORY FIRST STEP - Before any planning work:**

1. Create `PROGRESS.md` in project root if it doesn't exist
2. Add entry documenting planning phase start:

```markdown
## Feature: [Feature Name]

### Planning Phase
**Status**: In Progress
**Started**: [Current date/time]
**Plan File**: .agents/plans/[feature-name].md

Planning in progress...
```

**Use Write tool if PROGRESS.md doesn't exist, Edit tool if it does.**

This creates a persistent record visible to future agents working on this feature.

---

## Mission

Transform a feature request into a **comprehensive implementation plan** through systematic codebase analysis, external research, and strategic planning.

**Core Principle**: Create a context-rich plan that enables one-pass implementation success for AI agents.

**Key Philosophy**: Context is King. The plan must contain ALL information needed for implementation - patterns, mandatory reading, documentation, validation commands.

## Planning Process

### Phase 1: Feature Understanding

- Extract the core problem being solved
- Identify user value and business impact
- Determine feature type: New Capability/Enhancement/Refactor/Bug Fix
- Assess complexity: Low/Medium/High
- Map affected systems and components
- Create/refine user story:
  ```
  As a <type of user>
  I want to <action/goal>
  So that <benefit/value>
  ```

### Phase 2: Codebase Intelligence Gathering

**CRITICAL: Check for Similar Features First**

Before designing a new approach, search for similar existing features. If found:
1. **STOP and ask the user** using AskUserQuestion: "I found [existing feature] which is similar. Should I use the same approach or different architecture?"
2. Wait for user confirmation
3. Document the existing feature's approach as baseline

**Common patterns to check:**
- New integration → Check existing integrations
- New API endpoint → Check existing endpoints
- New auth method → Check existing auth
- New database table → Check existing schema
- New tool/command → Check existing tools
- New UI component → Check existing components

**After User Confirms Approach:**

Use specialized agents and parallel analysis:

1. **Project Structure**: Languages, frameworks, directory structure, service boundaries, config files
2. **Pattern Recognition**: Similar implementations, naming conventions, error handling, logging patterns, check CLAUDE.md
3. **Dependency Analysis**: External libraries, integration patterns, relevant documentation (docs/, ai_docs/, .agents/reference)
4. **Testing Patterns**: Test framework, test organization, coverage requirements
5. **Integration Points**: Files to update vs create, router patterns, database/model patterns, auth patterns

**Clarify Ambiguities (MANDATORY):**

**⚠️ CRITICAL: You MUST use AskUserQuestion tool if ANY of the following exist:**
- Requirements are unclear, ambiguous, or contradictory
- Missing critical information about scope, behavior, or constraints
- Multiple valid implementation approaches without clear preference
- Conflicts between user request and existing patterns
- Uncertainty about data models, schemas, or interfaces
- Questions about integrations, external services, or dependencies
- Unclear acceptance criteria or success metrics
- Any doubt about what the user wants or how to implement it

**DO NOT proceed with planning if you have ANY uncertainty. Stop and ask first.**

**Design Decisions (Follow Existing Patterns):**

When designing:
1. Default to mimicking existing similar features unless there's a clear reason to diverge
2. Document why if diverging from existing patterns
3. Prefer consistency over novelty
4. Design for parallel execution - identify independent tasks

Determine:
- Files to create vs modify, data flow, models/schemas, service components, API contracts, testing strategy
- Error scenarios and edge cases
- **Parallelization opportunities** - which tasks have no dependencies
- **Interface contracts** - what parallel tasks need from each other
- **Integration points** - where parallel work must synchronize

**Red Flags** (warrant asking user):
- Introducing new architectural patterns not used elsewhere
- Creating new folder structures when similar features exist
- Using different naming conventions than existing code

### Phase 3: External Research & Documentation

#### 3.1 API Research (If Applicable)

**If feature uses external APIs or unfamiliar SDKs:**

1. **Use `/core_piv_loop:explore-api [API name]` for each API**
   - Read generated research: `.agents/research/[api]-research.md`
   - Verify POC tests passed

2. **Critical Questions:**
   - Is the required feature supported?
   - What's the correct integration pattern?
   - Version compatibility issues?
   - Event types/response formats?
   - Configuration/setup required?
   - Known limitations or gotchas?

3. **Feasibility & Allowability Verification (MANDATORY):**
   - ✓ Required features technically available in API
   - ✓ API version supports use case
   - ✓ No blocking bugs or limitations
   - ✓ Performance/latency acceptable
   - ✓ Rate limits sufficient for usage
   - ✓ Features available in accessible pricing tier
   - ✓ Terms of Service permit intended use
   - ✓ No geographic/regulatory restrictions
   - ✓ Authentication method available

4. **Setup Requirements:**
   - Document prerequisite steps (account creation, approvals)
   - List required credentials and where to obtain
   - Note dashboard/console configuration needed
   - Identify webhook/callback setup if required

5. **Document in Plan:**
   - Add to CONTEXT REFERENCES > External API Research
   - Include integration patterns, compatibility constraints, POC results
   - Document setup requirements for Phase 1
   - List verification tests to confirm service access
   - Define fallback strategy if verification fails

#### 3.2 Dependency Verification & Compatibility Check

**Before adding new dependencies:**

1. **Test in isolated environment:**
   - Create temporary venv to test dependency compatibility
   - Check for conflicts with existing packages (especially deep dependencies like PyTorch, TensorFlow)
   - Document compatible version ranges

2. **Check dependency tree:**
   - Use `pipdeptree` or `npm list` to identify transitive dependencies
   - Look for version conflicts with existing packages
   - Note any peer dependency warnings

3. **Plan for conflicts:**
   - Document workarounds if conflicts exist (disable features, alternative packages)
   - Have rollback strategy ready
   - Consider feature flags to enable/disable based on environment

4. **Document in plan:**
   - Add to CONTEXT REFERENCES > Dependency Notes
   - List compatible versions
   - Note known conflicts and workarounds

#### 3.3 Technology & Pattern Research

- Research latest library versions and best practices
- Find official documentation with specific section anchors
- Locate implementation examples
- Identify common gotchas and known issues
- Check for breaking changes

#### 3.4 Compile Research References

```markdown
## Relevant Documentation
- [Library Docs](https://example.com/docs#section) - Why: Needed for X functionality
- [Framework Guide](https://example.com/guide#integration) - Why: Integration patterns
```

### Phase 4: Deep Strategic Thinking

**Think About:**
- How does this fit existing architecture?
- Critical dependencies and order of operations?
- What could go wrong? (Edge cases, race conditions, errors)
- How will this be tested comprehensively?
- Performance implications?
- Security considerations?
- Maintainability?

**Design for:**
- Alternative approaches with clear rationale
- Extensibility and future modifications
- Backward compatibility if needed
- Scalability implications

### Phase 5: Plan Structure Generation

**CRITICAL - Plan Length Constraint:**
- **HARD LIMIT**: Maximum plan length: **500-700 lines**
- Focus on essential information and actionable tasks
- Be concise but complete
- Quality over quantity - dense, information-rich content
- **You MUST verify line count after creation and adjust if needed**

**Plan Template Structure:**

```markdown
# Feature: <feature-name>

**⚠️ CRITICAL - DO NOT COMMIT CHANGES:**
- Implement ALL changes required by this plan
- Delete any debug logs you added during execution (console.log, print, etc.) that were NOT explicitly requested
- Keep pre-existing debug logs that were already in the codebase
- Leave ALL changes UNSTAGED (do NOT run git add or git commit)
- User will review changes with `git diff` before committing
- Only make code changes - no git operations

Validate documentation and codebase patterns before implementing. Pay attention to naming of existing utils, types, and models. Import from correct files.

## Feature Description

<Detailed description of feature, purpose, and user value>

## User Story

As a <user type>
I want to <action/goal>
So that <benefit/value>

## Problem Statement

<Define the specific problem or opportunity>

## Solution Statement

<Describe the proposed solution approach>

## Feature Metadata

**Feature Type**: [New Capability/Enhancement/Refactor/Bug Fix]
**Complexity**: [Low/Medium/High]
**Primary Systems Affected**: [Components/services]
**Dependencies**: [External libraries or services]
**Breaking Changes**: [Yes/No - explain if yes]

---

## CONTEXT REFERENCES

### Relevant Codebase Files - MUST READ BEFORE IMPLEMENTING

- `path/to/file.py` (lines 15-45) - Why: Pattern for X to mirror
- `path/to/model.py` (lines 100-120) - Why: Database model structure

### New Files to Create

- `path/to/new_service.py` - Service implementation for X
- `tests/path/to/test_new_service.py` - Unit tests

### Relevant Documentation - READ BEFORE IMPLEMENTING

- [Doc Link](https://example.com/doc#section) - Why: Required for secure endpoints

### External API Research (If Applicable)

**API:** [Name] v[Version]
**Research Doc:** `.agents/research/[api]-research.md`
**Documentation:** [Primary URL]

**Integration Pattern:**
```[language]
# Initialization, usage, error handling examples
```

**Critical Findings:** [Key findings impacting implementation]
**Validation Strategy:** [POC description, expected results, fallback]

### Patterns to Follow

**Naming Conventions:** [Examples from codebase]
**Error Handling:** [Examples from codebase]
**Logging Pattern:** [Examples from codebase]

---

## PARALLEL EXECUTION STRATEGY

### Dependency Graph

```
┌─────────────────────────────────────────────┐
│ WAVE 1: Foundation (Parallel)               │
├─────────────────────────────────────────────┤
│ Task 1.1: [Name] │ Task 1.2: [Name]        │
│ Agent: [Role]    │ Agent: [Role]           │
└─────────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────┐
│ WAVE 2: Core (After Wave 1)                 │
├─────────────────────────────────────────────┤
│ Task 2.1: [Name] - Deps: 1.1, 1.2          │
└─────────────────────────────────────────────┘
```

### Parallelization Summary

**Wave 1 - Fully Parallel:** Tasks [X, Y, Z] - No dependencies
**Wave 2 - Parallel after Wave 1:** Tasks [A, B] - Needs Wave 1
**Wave 3 - Sequential:** Task [C] - Needs Wave 2

### Interface Contracts

**Contract 1:** Task [X] provides [interface] → Task [A] consumes
**Mock for Parallel Work:** [How Task A can continue while Task X in progress]

### Synchronization Checkpoints

**After Wave 1:** Verify all Wave 1 tasks pass: `[integration test command]`
**After Wave 2:** Verify Wave 2 integration: `[integration test command]`

---

## IMPLEMENTATION PLAN

### Phase 1: Foundation & External Service Verification

**CRITICAL: If using external APIs/services, Phase 1 MUST include:**
1. Complete all external service setup (accounts, API keys, config)
2. Verify all services work as expected
3. Test integration patterns from research
4. Confirm feasibility before Phase 2

**Tasks:**

#### Task 1.1: External Service Setup & Verification (If Applicable - BLOCKER)

**⚠️ BLOCKER:** If verification fails, update plan or escalate.

**Setup Steps:**
1. Review: `.agents/research/[api]-research.md`
2. Create accounts/API keys for [services]
3. Configure dashboards (webhooks, OAuth, settings)
4. Store credentials in `.env` securely
5. Implement service initialization
6. Add connection validation

**Verification Tests:**
```bash
# Test connectivity and features for each service
[commands to verify services]
```

**Checklist:**
- [ ] All accounts created and accessible
- [ ] API keys generated and stored securely
- [ ] Required features confirmed available
- [ ] Rate limits verified sufficient
- [ ] Authentication working
- [ ] Response format matches expectations
- [ ] No unexpected errors
- [ ] Setup documented

**Expected Success:** [Exact output showing success]

**If Fails:**
1. Document blocker (which service, error)
2. Evaluate options (fix config, different tier, alternative, modify requirements)
3. Update plan OR escalate - STOP until resolved

**DO NOT PROCEED TO OTHER TASKS UNTIL THIS PASSES**

---

#### Task 1.2: [ACTION] [target_file]

**Purpose:** [What this accomplishes]
**Dependencies:** Task 1.1 (if external APIs), otherwise []
**Steps:** [Implementation steps]
**Validation:** `[command]`

### Phase 2: Core Implementation

[Main implementation work - business logic, services, APIs, models]

### Phase 3: Integration

[How feature integrates - routers, registration, config, middleware]

### Phase 4: Testing & Validation

[Testing approach - unit, integration, edge cases, acceptance]

---

## STEP-BY-STEP TASKS

Tasks organized by execution waves for parallel execution.

### Task Format

- **CREATE**: New files/components
- **UPDATE**: Modify existing files
- **ADD**: Insert new functionality
- **REMOVE**: Delete deprecated code
- **REFACTOR**: Restructure without behavior change
- **MIRROR**: Copy pattern from elsewhere

### Task Metadata

- **WAVE**: Execution wave (same wave = parallel)
- **AGENT_ROLE**: Suggested agent specialization
- **DEPENDS_ON**: Task IDs that must complete first
- **BLOCKS**: Task IDs waiting for this
- **PROVIDES**: What this delivers for downstream tasks
- **INTEGRATION_TEST**: Verify integration with parallel tasks

---

### WAVE 1: Foundation

**CRITICAL:** If using external APIs, Task 1.1 MUST be service setup/verification.

#### Task 1.1: External Service Setup (REQUIRED if external APIs)

- **WAVE**: 1
- **AGENT_ROLE**: integration-specialist
- **DEPENDS_ON**: []
- **BLOCKS**: [All tasks using external services]
- **PROVIDES**: Verified service access, credentials, confirmed features
- **IMPLEMENT**: Create accounts, generate keys, configure dashboards, test connectivity, verify features
- **VERIFICATION**: `[commands to verify all services]`
- **IF_FAILS**: Document blocker, evaluate alternatives, update plan OR escalate
- **VALIDATE**: `[all verification tests]`

#### Task 1.2: {ACTION} {target}

- **WAVE**: 1
- **AGENT_ROLE**: [role]
- **DEPENDS_ON**: [Task 1.1 if APIs, else []]
- **BLOCKS**: [IDs]
- **PROVIDES**: [Interface for downstream]
- **IMPLEMENT**: [Details]
- **PATTERN**: [Reference file:line]
- **VALIDATE**: `[command]`
- **INTEGRATION_TEST**: `[command with Wave 1 peers]`

**Wave 1 Checkpoint:** `[integration test after all Wave 1 complete]`

---

### WAVE 2: Core (After Wave 1)

#### Task 2.1: {ACTION} {target}

- **WAVE**: 2
- **AGENT_ROLE**: [role]
- **DEPENDS_ON**: [Task 1.x]
- **BLOCKS**: [IDs]
- **PROVIDES**: [What it delivers]
- **IMPLEMENT**: [Details]
- **USES_FROM_WAVE_1**: Task 1.1 provides [X], Task 1.2 provides [Y]
- **VALIDATE**: `[command]`

**Wave 2 Checkpoint:** `[integration test]`

---

### WAVE 3: Integration (Sequential)

#### Task 3.1: {ACTION} {target}

- **WAVE**: 3
- **AGENT_ROLE**: integration-specialist
- **DEPENDS_ON**: [Task 2.x]
- **BLOCKS**: []
- **PROVIDES**: Final integrated feature
- **IMPLEMENT**: [Integration logic]
- **VALIDATE**: `[end-to-end test]`

**Final Checkpoint:** `[full feature test]`

---

## TESTING STRATEGY

**⚠️ CRITICAL: ALL tests that can be automated MUST be automated**

The default is automation. Manual testing is only acceptable when automation is physically impossible (e.g., hardware interaction, legal/auth barriers that cannot be bypassed in a test context). "It's hard to automate" or "it requires a browser" are NOT valid reasons — use Playwright MCP.

### Automation-First Decision Rule

Before marking any test as manual, answer: **"Could Playwright MCP, pytest, or any other automated tool perform this check?"**
- If YES → automate it. No exceptions.
- If NO → document the specific, concrete reason why automation is impossible.

### Tool Selection by Test Type

| What you're testing | Tool |
|---|---|
| Backend logic, services, APIs | `pytest` (backend `tests/auto/`) |
| Frontend UI, user flows | Playwright (`frontend/tests/*.spec.ts`) |
| Full-stack integration (UI → API → DB) | Playwright |
| Third-party dashboards (LangSmith, Supabase, etc.) | Playwright — browse the actual site and assert state |
| Third-party REST APIs (verify external state) | `pytest` or Playwright `fetch()` within tests |
| Backend + third-party combined | `pytest` calling external APIs directly |

**Key principle — Playwright MCP covers third-party sites too.** If validation requires checking state in an external dashboard (e.g., LangSmith traces, Supabase table contents, Stripe payments, email delivery), write a Playwright test that navigates to that site and asserts what you need. Do NOT replace this with a manual "go check the dashboard" step.

**Example**: Instead of "Verify traces appear in LangSmith dashboard (manual)" → write a Playwright test that queries the LangSmith REST API or browses smith.langchain.com and asserts runs exist with the expected structure.

### Test Automation Requirements

For EACH test, specify:
1. **Test Type**: Unit / Integration / E2E / Third-Party Validation
2. **Automation Status**: ✅ Automated / ⚠️ Manual (requires concrete justification)
3. **Tool/Framework**: pytest / Playwright / Jest
4. **Test Location**: Exact file path
5. **Execution Command**: Exact command to run
6. **Manual Justification** (if ⚠️): The specific reason automation is impossible — not just difficult

**Automation Preference:** ✅ Automated → ⚠️ Manual (last resort, must justify)

### Unit Tests

**Automation**: ✅ Fully Automated
**Tool**: [pytest / Jest / unittest]
**Location**: [backend/tests/auto/ or frontend/src/**/*.test.ts]
**Execution**: `[command]`

### Integration Tests

**Automation**: ✅ Fully Automated
**Tool**: [pytest / Playwright]
**Location**: [backend/tests/auto/ or frontend/tests/]
**Execution**: `[command]`

### End-to-End Tests

**Automation**: ✅ Automated (Playwright is capable of full E2E including auth flows)
**Tool**: Playwright (`frontend/tests/*.spec.ts`)
**Location**: `frontend/tests/[feature].spec.ts`
**Execution**: `cd frontend && npx playwright test [feature]`

Describe E2E scenarios: login → action → assert result in UI and/or database.

### Third-Party Service Validation

For any feature that produces observable state in an external service:

**Automation**: ✅ Automated via Playwright or direct API call
**Examples**:
- LangSmith traces → Playwright test queries `/api/v1/runs/query` and asserts run structure
- Supabase table state → pytest queries Supabase REST API or uses supabase-py client
- Email delivery → pytest queries email API (Resend, SendGrid) for delivery status
- Webhook delivery → pytest checks webhook provider dashboard API

**Pattern** (third-party REST API in Playwright):
```typescript
// In frontend/tests/[feature].spec.ts
const res = await fetch('https://third-party.api/endpoint', {
  headers: { 'x-api-key': process.env.THIRD_PARTY_API_KEY! }
});
const data = await res.json();
expect(data.someField).toBe(expectedValue);
```

**Pattern** (third-party REST API in pytest):
```python
# In backend/tests/auto/test_[feature].py
import requests, os
res = requests.get('https://third-party.api/endpoint',
    headers={'Authorization': f"Bearer {os.getenv('THIRD_PARTY_API_KEY')}"})
assert res.status_code == 200
assert res.json()['someField'] == expected_value
```

### Manual Tests (Only if automation is physically impossible)

Valid reasons to mark a test as manual:
- Requires physical hardware (camera, microphone, Bluetooth, NFC)
- Requires a human to perform a legal/compliance action (sign a document, pass a CAPTCHA)
- Third-party enforces strict bot detection that cannot be bypassed even with real browser automation

**NOT** valid reasons:
- ~~"It requires a browser"~~ — use Playwright
- ~~"It requires visiting a third-party site"~~ — use Playwright or their REST API
- ~~"It's hard to set up"~~ — set it up
- ~~"It's an observability feature"~~ — automate the assertion

#### Manual Test [#]: [Name]
**Why Manual**: [Specific, concrete reason automation is impossible]
**Steps**: [1, 2, 3]
**Expected**: [Success criteria]

### Edge Cases

For each edge case:
- **Test**: [Name]
- **Scenario**: [What edge case]
- **Automation**: ✅ [pytest/Playwright] — `[command]`
- If truly manual: **Why**: [Specific reason]

### Test Automation Summary

**Total Tests**: [#]
- ✅ **Automated**: [#] ([%]%)
  - Backend (pytest): [#]
  - Frontend/E2E (Playwright): [#]
  - Third-party validation: [#]
- ⚠️ **Manual**: [#] ([%]%) — [specific reason for each]

**Goal**: 100% automation. Any manual test requires explicit justification.

**Execution Agent Instructions**:
- CREATE all automated test files as part of implementation tasks (not afterthoughts)
- RUN automated tests after each wave checkpoint
- Third-party validation tests go in the same test suite as other tests — not in a separate "manual verification" section
- DOCUMENT manual test results in execution report only when automation is genuinely impossible

---

## VALIDATION COMMANDS

Execute every command to ensure zero regressions and 100% correctness.

### Level 0: External Service Validation (If Applicable - MUST PASS FIRST)

**CRITICAL:** Confirms external service setup working. If fails, feature is blocked.

For each service:

#### [Service] Validation
```bash
# Verify account/credentials
[check account access, API key validity]

# Test connectivity
[connection test]

# Verify required features
[test specific features - not just connection]

# Confirm rate limits
[check usage vs limits]

# Test integration pattern
[POC test from research]
```

**Expected:** Account active, connection successful, features accessible, rate limits sufficient
**If Failed:** Check credentials, network, service status; upgrade plan; redesign; or STOP and escalate
**DO NOT PROCEED TO LEVEL 1+ UNTIL ALL SERVICES PASS**

### Level 1: Syntax & Style

[Project linting/formatting commands]

### Level 2: Unit Tests

[Unit test commands]

### Level 3: Integration Tests

[Integration test commands]

### Level 4: Manual Validation

[Feature-specific manual testing - API calls, UI testing]

### Level 5: Additional (Optional)

[MCP servers or additional tools]

---

## ACCEPTANCE CRITERIA

- [ ] **External Services (if applicable):**
  - [ ] All accounts created, accessible, features verified
  - [ ] API keys stored securely, rate limits sufficient
  - [ ] Integration tests passing, setup documented
- [ ] Feature implements all specified functionality
- [ ] All validation commands pass (including Level 0)
- [ ] Unit test coverage 80%+
- [ ] Integration tests verify end-to-end workflows
- [ ] Code follows project conventions
- [ ] No regressions
- [ ] Documentation updated (if applicable)
- [ ] Performance/security addressed (if applicable)

---

## COMPLETION CHECKLIST

- [ ] **Phase 1 External Service Verification (if applicable):**
  - [ ] Services set up and verified BEFORE implementation
  - [ ] Level 0 validation passes
  - [ ] No blockers from service limitations
- [ ] All tasks completed in wave order
- [ ] Each task validation passed
- [ ] All validation commands executed (Levels 0-5)
- [ ] **Test Automation:**
  - [ ] All automated tests created and passing
  - [ ] 80%+ automated coverage achieved
  - [ ] Manual tests documented with clear instructions
- [ ] Full test suite passes (unit + integration + E2E)
- [ ] No linting/type errors
- [ ] Manual testing completed (if any manual tests)
- [ ] All acceptance criteria met
- [ ] Code reviewed for quality
- [ ] **⚠️ Debug logs added during execution REMOVED (keep pre-existing logs only)**
- [ ] **⚠️ CRITICAL: Changes left UNSTAGED (NOT committed) for user review**

---

## NOTES

[Additional context, design decisions, trade-offs]
```

## Output Format

**🚨 REMINDER: Write to FILE only - NOT to CLI 🚨**

**Filename**: `.agents/plans/{kebab-case-descriptive-name}.md`
**Directory**: Create `.agents/plans/` if doesn't exist

**CRITICAL:** Use Write or Edit tool to create the plan file. Do NOT output plan content in your CLI response.

---

## POST-PLANNING VERIFICATION

**REQUIRED STEPS AFTER CREATING PLAN:**

```bash
mkdir -p .agents/plans
# Plan saved to: .agents/plans/[feature-name].md

# CRITICAL: Verify file exists and was written
test -f .agents/plans/[feature-name].md && echo "✅ Plan file created" || echo "❌ Plan file missing"

# CRITICAL: Verify line count is 500-700 lines
wc -l .agents/plans/[feature-name].md

# If not in range, adjust NOW
```

**🚨 VERIFICATION CHECKPOINT:**
- [ ] **Plan written to FILE** (used Write or Edit tool, NOT CLI output)
- [ ] **File exists** (verified with test -f)
- [ ] **Line count verified** (500-700 lines)

**Line Count Adjustment:**
- If < 500: Add examples, edge cases, validation details
- If > 700: Consolidate tasks, remove redundancy, use concise language

**Final Checklist:**
- [ ] **All ambiguities resolved using AskUserQuestion**
- [ ] **Plan written to `.agents/plans/[feature-name].md` (NOT CLI)**
- [ ] Plan is 500-700 lines (VERIFY with `wc -l` - MANDATORY)
- [ ] Feature name is kebab-case
- [ ] File paths exact and correct
- [ ] Validation commands executable
- [ ] Code examples from actual codebase
- [ ] Tasks in dependency order
- [ ] Another agent could execute without context
- [ ] Patterns reference specific file:line
- [ ] **Tasks in parallel execution waves**
- [ ] **Each task has WAVE, DEPENDS_ON, AGENT_ROLE**
- [ ] **Interface contracts defined**
- [ ] **Integration checkpoints specified**
- [ ] **30%+ tasks can execute in parallel** (or explain why not)
- [ ] **Test Automation Planned:**
  - [ ] Every test marked ✅/⚠️/🔄
  - [ ] Automated tests specify tool (pytest, Playwright MCP, etc.)
  - [ ] Automated tests include execution commands
  - [ ] Manual tests explain why not automated (brief)
  - [ ] Test summary with percentages
  - [ ] 80%+ automated coverage targeted
- [ ] **External Service Setup (if applicable):**
  - [ ] Phase 1 Task 1.1 is service setup/verification
  - [ ] All services identified, setup documented
  - [ ] Verification tests defined with expected outputs
  - [ ] Fallback strategy documented
  - [ ] Level 0 validation includes service checks

---

## Quality Criteria

### Context Completeness ✓
- [ ] All necessary patterns identified
- [ ] External library usage documented with links
- [ ] Integration points mapped
- [ ] Gotchas and anti-patterns captured
- [ ] Every task has executable validation

### Implementation Ready ✓
- [ ] Another developer could execute without context
- [ ] Tasks ordered by dependency
- [ ] Each task atomic and testable
- [ ] Pattern references include file:line

### Parallel Execution Ready ✓
- [ ] Tasks in execution waves
- [ ] Dependencies documented
- [ ] Interface contracts defined
- [ ] Checkpoints identified
- [ ] Agent roles assigned
- [ ] Integration tests per wave

### External Service Ready ✓ (If Applicable)
- [ ] All APIs/services identified
- [ ] Feasibility/allowability verified
- [ ] Setup requirements documented
- [ ] Phase 1 Task 1.1 is service setup
- [ ] Verification tests with success outputs
- [ ] Fallback strategy documented
- [ ] Level 0 validation includes services

### Test Automation Planning ✓
- [ ] Every test categorized (✅/⚠️/🔄)
- [ ] Automated tests specify tool
- [ ] Automated tests include commands
- [ ] Manual tests explain why
- [ ] Test summary with percentages
- [ ] 80%+ coverage goal
- [ ] Execution agent has clear test instructions

### Pattern Consistency ✓
- [ ] Follows codebase conventions
- [ ] New patterns justified
- [ ] No reinvention of existing patterns
- [ ] Testing matches project standards

### Information Density ✓
- [ ] No generic references (all specific)
- [ ] URLs include section anchors
- [ ] Validation commands non-interactive
- [ ] Plan length 500-700 lines (concise)

---

## FINAL REPORT

After creating the plan, output this report and STOP (do not execute):

**🚨 CRITICAL REMINDER: Full plan content is in the FILE, NOT this message 🚨**
**Do NOT include plan sections, tasks, or implementation details in this report.**
**This report should be a SUMMARY only (see below).**

```
✅ Plan Created Successfully

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 Feature: [Feature name]
📄 Plan Location: .agents/plans/[feature-name].md
📏 Plan Length: [XXX] lines (target: 500-700)
⚡ Complexity: [Low/Medium/High]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📝 Summary:
[2-3 sentence summary of feature and approach]

⚡ Parallel Execution:
- Waves: [#] execution waves
- Parallelization: [#] tasks concurrent
- Max Speedup: [X]x with [N] parallel agents
- Sequential: [#] must run sequentially

🧪 Test Automation:
- Total: [#] tests
- Automated: [#] ([XX]%) - using [tools: pytest, Playwright MCP, etc.]
- Manual: [#] ([XX]%) - [brief reason if >20%]
- Goal: 80%+ [✅ Met / ⚠️ [XX]% - explain]

⚠️  Key Risks:
[2-4 risks with mitigations]

🔍 Similar Features:
[Features used as patterns, or "None - new pattern"]

❓ User Questions: [# of AskUserQuestion calls, or "None - clear"]

📊 Tasks: [#] total ([X] parallel, [Y] sequential)
🎯 Confidence: [X]/10 for one-pass success
👥 Team Size: [#] agents for optimal parallelization

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚀 To execute this plan, run:

    /core_piv_loop:execute .agents/plans/[feature-name].md

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**STOP HERE - DO NOT PROCEED WITH EXECUTION**
