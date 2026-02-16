**Create comprehensive plan with the following structure:**

Feature description: $ARGUMENTS

Make sure the structured plan you create is 500-700 lines long. You have failed if you create a plan that's not within this range.

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

### Patterns to Follow

<Specific patterns extracted from codebase - include actual code examples from the project>

**Naming Conventions:** (for example)

**Error Handling:** (for example)

**Logging Pattern:** (for example)

**Other Relevant Patterns:** (for example)

---

## IMPLEMENTATION PLAN

### Phase 1: Foundation

<Describe foundational work needed before main implementation>

**Tasks:**

- Set up base structures (schemas, types, interfaces)
- Configure necessary dependencies
- Create foundational utilities or helpers

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
- Create end-to-end (e2e) tests for complete user workflows
- Add edge case tests
- Request user input if needed for e2e tests (credentials, paths, API keys)
- Validate against acceptance criteria

---

## STEP-BY-STEP TASKS

IMPORTANT: Execute every task in order, top to bottom. Each task is atomic and independently testable.

### Task Format Guidelines

Use information-dense keywords for clarity:

- **CREATE**: New files or components
- **UPDATE**: Modify existing files
- **ADD**: Insert new functionality into existing code
- **REMOVE**: Delete deprecated code
- **REFACTOR**: Restructure without changing behavior
- **MIRROR**: Copy pattern from elsewhere in codebase
- **REQUEST**: Ask user for required configuration (e.g., paths, API keys)

### {ACTION} {target_file}

- **IMPLEMENT**: {Specific implementation detail}
- **PATTERN**: {Reference to existing pattern - file:line}
- **IMPORTS**: {Required imports and dependencies}
- **GOTCHA**: {Known issues or constraints to avoid}
- **VALIDATE**: `{executable validation command}`

### Example Task for E2E Tests with User Configuration

**CREATE** `tests/features/your_feature/test_e2e_tool_invocation.py`

- **IMPLEMENT**: End-to-end test that validates complete workflow through API
- **REQUEST**: Before running e2e tests, use AskUserQuestion to request required configuration:
  - "E2E tests need [specific configuration]. Please provide [what is needed]"
  - Example: "E2E tests need a valid Obsidian vault path for testing. What path should I use?"
- **CONFIGURE**: Update `.env` or test configuration with user-provided values
- **VERIFY**: Check that configuration is valid before proceeding
- **PATTERN**: Reference existing e2e test patterns from codebase
- **VALIDATE**: `pytest path/to/test_e2e_*.py -v`

<Continue with all tasks in dependency order...>

---

## TESTING STRATEGY

<Define testing approach based on project's test framework and patterns discovered in Phase 2>

### Unit Tests

<Scope and requirements based on project standards>

Design unit tests with fixtures and assertions following existing testing approaches

### Integration Tests

<Scope and requirements based on project standards>

### End-to-End (E2E) Tests

**Purpose**: Validate complete user workflows from API request through tool execution to response

**Requirements**:
- Test real user scenarios, not just component interactions
- Use actual dependencies when possible (real vault paths, actual API calls)
- Verify tool registration and invocation through the agent
- Test both streaming and non-streaming API endpoints if applicable

**User Input Handling**:
If e2e tests require user-specific configuration (e.g., vault paths, API keys, credentials):
1. Document required inputs clearly in the test file comments
2. During implementation, **request this information from the user** using AskUserQuestion tool
3. Add instructions for how to configure these values (e.g., `.env` file, test fixtures)
4. Use `pytest.mark.skipif` to skip tests when required config is missing
5. Include clear error messages explaining what configuration is needed

**Example Configuration Request**:
When implementing e2e tests that need a vault path:
- Use AskUserQuestion to ask: "E2E tests need a valid Obsidian vault path. What path should I use for testing?"
- Update `.env` or test configuration with the provided value
- Verify the path exists before running tests
- Provide clear documentation in test file about required setup

### Edge Cases

<List specific edge cases that must be tested for this feature>

**Edge Case Coverage Checklist:**
- [ ] Normal completion - Happy path scenarios work correctly
- [ ] Error scenarios - Proper error handling and messages
- [ ] Interruption/cancellation - Graceful handling of interrupted operations (timeouts, client disconnects)
- [ ] Concurrent operations - No race conditions or state conflicts
- [ ] For async generators/streams - Cleanup code executes in all scenarios (use finally blocks)
- [ ] Boundary conditions - Empty inputs, null values, max limits
- [ ] Invalid inputs - Proper validation and rejection

---

## VALIDATION COMMANDS

<Define validation commands based on project's tools discovered in Phase 2>

Execute every command to ensure zero regressions and 100% feature correctness.

**IMPORTANT**: Before running e2e tests, ensure all required user configuration is obtained:
- If tests need specific paths, API keys, or credentials, request them from the user
- Update configuration files (`.env`, test fixtures) with provided values
- Verify configuration is valid before proceeding with test execution
- Document any skipped tests due to missing configuration

### Level 1: Syntax & Style

<Project-specific linting and formatting commands>

### Level 2: Unit Tests

<Project-specific unit test commands>

### Level 3: Integration Tests

<Project-specific integration test commands>

### Level 4: End-to-End Tests

**Prerequisites**: Ensure user has provided required configuration (vault paths, API keys, etc.)

<Project-specific e2e test commands>

Examples:
- `pytest tests/ -m e2e -v` - Run all e2e tests
- `pytest path/to/test_e2e_tool_invocation.py -v` - Run specific e2e test
- Verify tests actually invoke tools through the agent, not just test isolated components

**If Configuration Missing**:
- Tests should skip gracefully with clear messages
- Document what configuration is needed
- Provide instructions for setting up required values

### Level 5: Manual Validation

<Feature-specific manual testing steps - API calls, UI testing, etc.>

### Level 6: Additional Validation (Optional)

<MCP servers or additional CLI tools if available>

---

## ACCEPTANCE CRITERIA

<List specific, measurable criteria that must be met for completion>

- [ ] Feature implements all specified functionality
- [ ] All validation commands pass with zero errors
- [ ] Unit test coverage meets requirements (80%+)
- [ ] Integration tests verify component interactions
- [ ] End-to-end tests validate complete user workflows through the API
- [ ] E2E tests confirm tools are registered and invokable by the agent
- [ ] Required user configuration obtained and documented for e2e tests
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
- [ ] Full test suite passes (unit + integration + e2e)
- [ ] Required user configuration obtained for e2e tests
- [ ] E2E tests confirm end-to-end functionality works
- [ ] No linting or type checking errors
- [ ] Manual testing confirms feature works
- [ ] Acceptance criteria all met
- [ ] Code reviewed for quality and maintainability
- [ ] README.md updated with current project structure if any files/directories were added or removed

---

## NOTES

<Additional context, design decisions, trade-offs>

<!-- EOF -->
```

## Output Format

**Filename**: `.agents/plans/{kebab-case-descriptive-name}.md`

- Replace `{kebab-case-descriptive-name}` with short, descriptive feature name
- Examples: `add-user-authentication.md`, `implement-search-api.md`, `refactor-database-layer.md`

**Directory**: Create `.agents/plans/` if it doesn't exist

## Report

After creating the Plan, provide:

- Summary of feature and approach
- Full path to created Plan file
- Complexity assessment
- Key implementation risks or considerations