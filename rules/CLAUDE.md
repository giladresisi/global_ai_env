# Global CLAUDE.md

These instructions apply to all projects unless overridden by project-specific CLAUDE.md files.

## General Preferences

### Communication Style
- Be concise and direct
- Use technical language when appropriate
- Avoid unnecessary explanations unless asked
- No emojis unless explicitly requested

### Reporting Changes
- When changes from a single user request exceed 10 lines across all files (code and non-code):
  - Don't display full file contents in CLI output
  - Instead, summarize: "filename: L10-17, L25-32" (line ranges changed)
  - Example: "Updated main.py: L10-17, L25-32 | config.yaml: L5-8"
- For changes ≤10 lines total: showing full changes is acceptable
- **File paths:** Always preserve backslashes in Windows paths (e.g., `C:\Users\gilad\.claude\file.md` not `C:\Users\gilad.claude\file.md`)

### Development Workflow
- Always use version control (git)
- Create meaningful commit messages
- Follow existing code style and conventions
- Write tests for new features
- Document complex logic
- Test dependencies in isolation before adding to main environment
- Use incremental testing - validate one component at a time before integration

### Planning & Execution
- For non-trivial tasks, create a plan before implementing
- Save plans to `.agents/plans/` directory
- Include validation steps in all plans
- Mark plan complexity: ✅ Simple, ⚠️ Medium, 🔴 Complex

### Code Quality
- Prefer simple, maintainable code over clever solutions
- Don't over-engineer - implement only what's requested
- Don't add unnecessary abstractions
- Security first - watch for common vulnerabilities (SQL injection, XSS, etc.)
- Only add comments where logic isn't self-evident

### Testing
- Test critical paths
- Use existing test credentials when available
- Prefer automated tests over manual testing where practical
- Validate edge cases: normal completion, errors, interruption/cancellation, concurrent operations
- For async code: verify cleanup code executes in all scenarios (use finally blocks)

### Async Generator Cleanup Pattern

**CRITICAL:** Always use `finally` blocks for guaranteed cleanup in async generators.

```python
async def stream_generator():
    run_id = None
    error_occurred = False
    try:
        run_id = start_trace()
        # Stream generation
        yield data
    except Exception as e:
        error_occurred = True
        # Error handling
    finally:
        # ALWAYS runs - close traces, cleanup resources
        if run_id:
            close_trace(run_id, error=error_occurred)
```

**Why:**
- Async generators don't guarantee cleanup without `finally` blocks
- Try/except pattern insufficient for stream interruption scenarios
- Critical for: trace closure, database connections, file handles, external API cleanup

**Examples:**
- ✅ Good: Resource cleanup in `finally` block
- ❌ Bad: Cleanup in `try` or after `yield` (won't run if generator interrupted)

### Dependency Management

Before adding new dependencies:

1. **Test in isolated environment first:**
   ```bash
   # Create temporary venv
   python -m venv test_env
   test_env/Scripts/activate
   pip install new-dependency
   # Test for conflicts
   ```

2. **Check dependency tree:**
   ```bash
   pip install pipdeptree
   pipdeptree -p new-dependency
   ```

3. **Document compatible version ranges:**
   - In requirements.txt or plan
   - Note known conflicts
   - Have rollback plan (disable feature if dependencies conflict)

4. **Gradual rollout:**
   - Test one dependency at a time
   - Verify existing functionality still works
   - Document workarounds for conflicts

### Debugging Methodology

When encountering bugs:

1. **Instrument early:**
   - Add debug logging at key lifecycle points
   - Use proper logging framework for persistent logs
   - OK to use `print()` for temporary debugging - MUST remove before commit

2. **Test in isolation:**
   - Create minimal reproduction case
   - Eliminate variables one at a time
   - Gradually add complexity

3. **Document findings:**
   ```markdown
   Debug Log:
   1. Observed: [symptom]
   2. Hypothesis: [theory]
   3. Test: [what you tried]
   4. Result: ✅/❌ [outcome]
   5. Conclusion: [root cause]
   ```

4. **Verify async patterns:**
   - Check if cleanup code executes in all scenarios
   - Test with interrupted operations (timeouts, cancellations)
   - Validate error handling paths

5. **Edge case checklist:**
   - Normal completion ✅
   - Error scenarios ✅
   - Interruption/cancellation ✅
   - Concurrent operations ✅

### Credentials & Secrets Management

**CRITICAL: Never write real credentials in non-gitignored files.**

#### Real Credentials (API Keys, Passwords, Tokens)

**DO:**
- ✅ Store real credentials ONLY in `.env` or other gitignored files
- ✅ Create testing utility modules that read credentials from `.env`:
  - `tests/utils/test-auth.ts` (frontend)
  - `tests/utils/test_auth.py` (backend)
  - These utils fetch real credentials at runtime from environment
- ✅ Verify `.env` is in `.gitignore` before writing credentials

**DO NOT:**
- ❌ Write real credentials in `.env.example`
- ❌ Write real credentials in code files (even in tests)
- ❌ Write real credentials in documentation
- ❌ Gitignore new files that weren't already gitignored
- ❌ Commit real API keys, passwords, tokens, or secrets

#### Example/Mock Credentials

In files like `.env.example`, `README.md`, or documentation:

**Use hidden/masked values:**
```
# BAD - Real credentials visible
TEST_EMAIL=test@test.com
TEST_PASSWORD=123456
API_KEY=sk_live_abc123xyz

# GOOD - Masked credentials
TEST_EMAIL=test@...
TEST_PASSWORD=****
API_KEY=sk_live_...
```

#### Testing Pattern

**Example testing utility (TypeScript):**
```typescript
// tests/utils/test-auth.ts
export const getTestCredentials = () => ({
  email: process.env.TEST_EMAIL!,
  password: process.env.TEST_PASSWORD!
});
```

**Example testing utility (Python):**
```python
# tests/utils/test_auth.py
import os

def get_test_credentials():
    return {
        "email": os.getenv("TEST_EMAIL"),
        "password": os.getenv("TEST_PASSWORD")
    }
```

**Usage in tests:**
```typescript
import { getTestCredentials } from './utils/test-auth';
const { email, password } = getTestCredentials();
```

### Documentation
- Keep README.md up to date
- Document setup steps and prerequisites
- Include environment variable examples
- Don't create documentation unless explicitly requested

## Tool Preferences

### Language-Specific

#### Python
- Use virtual environments (venv)
- Keep requirements.txt updated
- Follow PEP 8 style guide
- Use type hints where helpful

#### JavaScript/TypeScript
- Use TypeScript for new projects
- Follow existing linting rules
- Keep dependencies minimal

### Database
- Always use migrations for schema changes
- Never modify production data without explicit permission
- Use Row-Level Security for multi-tenant apps

### Git
- Create feature branches for new work
- Write descriptive commit messages
- Don't force push to main/master
- Ask before destructive operations (reset, force push, etc.)

## Project Organization

Preferred structure:
```
project/
├── .agents/
│   └── plans/           # Implementation plans
├── docs/                # Documentation
├── tests/               # Test files
├── src/                 # Source code
├── .env.example         # Environment template
├── CLAUDE.md            # Project-specific instructions
├── README.md            # Project overview
└── PROGRESS.md          # Current status (optional)
```

## What NOT to Do

- ❌ Don't create files unnecessarily
- ❌ Don't refactor code that wasn't requested
- ❌ Don't add features beyond the scope
- ❌ Don't commit sensitive data (API keys, passwords, etc.) - see "Credentials & Secrets Management"
- ❌ Don't write real credentials in non-gitignored files (.env.example, docs, code)
- ❌ Don't make breaking changes without discussion
- ❌ Don't bypass security checks or validation

## Asking for Help

When stuck:
1. Explain what you tried
2. Share error messages
3. Ask specific questions
4. Suggest alternatives if available

## Notes

- Project-specific CLAUDE.md files override these global settings
- Update this file as you discover new preferences
- Keep it concise - focus on what matters most
