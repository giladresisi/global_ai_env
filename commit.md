Create a new commit for all of our uncommitted changes

## Step 1: Review Current State
Run: `git status && git diff HEAD && git status --porcelain`
This shows all uncommitted changes (modified, untracked, and staged files)

## Step 2: Stage ALL Changes
CRITICAL: You MUST run `git add .` to stage ALL files including:
- Modified files
- Untracked files
- Untracked directories

DO NOT selectively stage files with `git add <specific-file>` unless explicitly instructed.
The default behavior is to commit everything.

Run: `git add .`

## Step 3: Create Commit
Add an atomic commit message with conventional commit format:
- Type: "feat", "fix", "docs", "chore", "refactor", "test", etc.
- Scope (optional): component or area affected
- Description: brief summary (50 chars or less, imperative mood)
- Body (optional): detailed explanation if needed
- Footer: Include "Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>"

Run: `git commit -m "$(cat <<'EOF'
type(scope): description

[optional body]

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
EOF
)"`
