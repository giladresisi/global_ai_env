# Global CLAUDE.md

These instructions apply to all projects unless overridden by project-specific CLAUDE.md files.

## General Preferences

### Communication Style
- Be concise and direct
- Use technical language when appropriate
- Avoid unnecessary explanations unless asked
- No emojis unless explicitly requested

### Development Workflow
- Always use version control (git)
- Create meaningful commit messages
- Follow existing code style and conventions
- Write tests for new features
- Document complex logic

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
