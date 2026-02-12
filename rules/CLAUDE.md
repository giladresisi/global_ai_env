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
- Save plans to `.agent/plans/` directory
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
├── .agent/
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
- ❌ Don't commit sensitive data (API keys, passwords, etc.)
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
