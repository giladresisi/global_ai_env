---
name: commit
description: Use when the user asks to commit changes, create a commit, or save work to git
---

Create a new commit for all of our uncommitted changes

## Step 0: Determine Reasoning
Before committing, identify WHY these changes were needed. The commit body MUST include a short reason.

- If the user explicitly stated a reason (e.g., "commit my changes, I fixed the login bug"), use that.
- If this skill was invoked by another skill/command and the reason is clear from context, use that.
- If the reason is NOT clear from the conversation context, use AskUserQuestion to ask: "Why were these changes needed? (This will be included in the commit message)"

## Step 1: Review Current State
Run: `git status && git diff HEAD && git status --porcelain`
This shows all uncommitted changes (modified, untracked, and staged files)

## Step 1.5: Secret / Credential Scan
Before staging anything, scan the diff output from Step 1 for explicit secrets or credentials.

Look for patterns such as:
- API keys (e.g., `sk-...`, `AIza...`, `AKIA...`, `ghp_...`, `xoxb-...`)
- Passwords or tokens assigned to variables (e.g., `password = "..."`, `token: "..."`, `secret = "..."`)
- Private keys or certificates (e.g., `-----BEGIN RSA PRIVATE KEY-----`)
- Hard-coded connection strings with credentials (e.g., `postgresql://user:pass@host`)
- Any high-entropy string (20+ chars) assigned to a variable named key, secret, token, password, credential, or similar

**If any suspected secret is found:**
1. DO NOT stage or commit anything.
2. Alert the user with the exact file path, line number, and the suspicious value (mask the middle characters, e.g., `sk-ab...xyz`).
3. Ask the user to confirm whether it is a real secret and how to proceed before continuing.
4. STOP — do not proceed to Step 2 until the user responds.

**Only continue to Step 2 if no secrets were found, or the user has confirmed the flagged values are safe.**

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
- Body (REQUIRED): must include a short reason explaining WHY these changes were needed
- Footer: Include "Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>"

Run: `git commit -m "$(cat <<'EOF'
type(scope): description

Why: <short reason these changes were needed>

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
EOF
)"`
