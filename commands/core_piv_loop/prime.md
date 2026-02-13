---
description: Prime agent with codebase understanding
args: "[focus_area]"
---

# Prime: Load Project Context

## Objective

Build understanding of the codebase - either high-level overview or focused deep-dive based on whether a focus area argument is provided.

## Usage

- `/core_piv_loop:prime` - High-level overview (architecture, patterns, structure)
- `/core_piv_loop:prime "authentication system"` - Deep-dive into specific area
- `/core_piv_loop:prime "API endpoints"` - Focus on particular functionality

## Decision: Which Mode?

**Check if focus area argument was provided:**
- Arguments provided → Use **FOCUSED mode**
- No arguments → Use **HIGH-LEVEL mode**

---

## HIGH-LEVEL Mode (No Arguments)

**Goal:** Understand architecture, patterns, and structure with minimal token usage.

### Step 1: Directory Structure

Use Bash to show directory tree (2 levels max):
```bash
tree -L 2 -I 'node_modules|__pycache__|.git|dist|build|.next|coverage|venv|.venv'
```

If tree not available, use ls or alternative.

### Step 2: Count Files (Optional)

Only if in a git repo:
```bash
git ls-files | wc -l
```

### Step 3: Read Minimal Documentation

**Read at most 3 files:**
1. Root README.md (if exists)
2. CLAUDE.md or .claude/config (if exists)
3. One config file: package.json OR pyproject.toml OR tsconfig.json OR Cargo.toml

**DO NOT read:**
- Implementation files (src/, lib/, etc.)
- Test files
- Subdirectory READMEs
- Multiple config files

### Step 4: Check Git State (If Git Repo)

```bash
git status
git log -10 --oneline
```

### Step 5: Generate Report

**Project Overview**
- Purpose and type of application (from README)
- Primary technologies and frameworks (from config)

**Architecture**
- Directory structure and organization
- Key directories and their likely purposes (inferred from names)
- Main architectural patterns (if obvious from structure)

**Tech Stack**
- Languages (inferred from config and directory structure)
- Frameworks and major dependencies (from config)
- Build tools

**Current State** (if git repo)
- Active branch
- Recent activity summary

**Keep report concise - bullet points, no deep implementation details.**

---

## FOCUSED Mode (With Argument)

**Goal:** Deep understanding of a specific area, ignore everything else.

### Step 1: Extract Keywords

From the focus area argument, identify key search terms.

Example: "authentication system" → keywords: auth, authentication, login, session, user

### Step 2: Find Relevant Files

Use Grep to search for files containing focus keywords:
```bash
grep -r "keyword1\|keyword2" --files-with-matches --include="*.py" --include="*.ts" --include="*.js"
```

Or use Glob for pattern matching:
```bash
**/*auth*/**
**/*login*/**
```

### Step 3: Read All Relevant Files

- Read ALL files found in Step 2 that are directly related
- Read imported dependencies if critical to understanding
- Read related documentation
- Go deep into implementation details

**No limit on file reading in focused mode.**

### Step 4: Generate Report

**Focus Area:** [provided argument]

**Files Analyzed**
- List all file paths read (grouped by directory)

**Implementation Overview**
- Key functions/classes/components with brief descriptions
- Main data structures or types
- Entry points and exports

**Dependencies & Integrations**
- External libraries used
- Internal modules/files this area depends on
- Areas that depend on this code

**Patterns & Conventions**
- Code style observations
- Design patterns used
- Testing approach (if tests found)

**Technical Notes**
- Important implementation details
- Potential issues or concerns
- Suggested related areas to explore

**Provide detailed, specific, actionable information.**

---

## Tips

- **High-level mode** optimizes for minimal context usage - great for initial orientation
- **Focused mode** goes deep - use when planning work on a specific feature/area
- Focus arguments should be descriptive: "user authentication flow" not just "auth"
- Can run high-level first, then focused on specific areas as needed
