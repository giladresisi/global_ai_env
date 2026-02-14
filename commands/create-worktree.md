---
name: create-worktree
description: Create a git worktree with automatic environment setup (copy .env files, venv, install dependencies)
---

# Create Worktree with Environment Setup

When this command is invoked with `/create-worktree <branch-name> [base-branch]`, execute the following steps:

## Step 1: Parse Arguments

Extract the branch name and optional base branch from the command arguments.

## Step 2: Create Git Worktree

Execute the git worktree add command:

```bash
# If base-branch provided:
git worktree add ../{branch-name} -b {branch-name} {base-branch}

# If no base-branch provided:
git worktree add ../{branch-name} -b {branch-name}
```

The worktree will be created in the parent directory at `../{branch-name}`.

## Step 3: Identify Source and Target Paths

- **Source:** Current working directory (the original repo)
- **Target:** `../{branch-name}` (the new worktree)

## Step 4: Copy .env Files

Use Glob to find all .env files in:
- Top-level: `.env`
- Frontend: `frontend/.env`, `frontend/.env.local`, etc.
- Backend: `backend/.env`, `backend/.env.local`, etc.

For each .env file found:
1. Use Bash to copy it to the corresponding location in the target worktree
2. Example: If `backend/.env` exists, copy to `../{branch-name}/backend/.env`

```bash
# Windows
copy backend\.env ..\{branch-name}\backend\.env

# Unix/Mac
cp backend/.env ../{branch-name}/backend/.env
```

## Step 5: Copy venv Folders

Use Bash to check for venv folders:
- Top-level: `venv/`
- Backend: `backend/venv/`

For each venv folder found:
1. Copy the entire folder to the target worktree
2. Use appropriate copy command for the platform

```bash
# Windows (use robocopy for efficiency)
robocopy venv ..\{branch-name}\venv /E /NFL /NDL /NJH /NJS /nc /ns /np

# Unix/Mac
cp -r venv ../{branch-name}/venv
```

## Step 6: Install Python Dependencies

Use Glob to find all `requirements.txt` files in:
- Top-level: `requirements.txt`
- Backend: `backend/requirements.txt`

For each requirements.txt found where a venv exists in the same directory:
1. Navigate to the target worktree directory
2. Run pip install using the venv's Python

```bash
# Windows
cd ..\{branch-name}\backend
venv\Scripts\python -m pip install -r requirements.txt

# Unix/Mac
cd ../{branch-name}/backend
venv/bin/python -m pip install -r requirements.txt
```

## Step 7: Install npm Dependencies

Use Glob to find all `package.json` files in:
- Top-level: `package.json`
- Frontend: `frontend/package.json`
- Backend: `backend/package.json`

For each package.json found:
1. Navigate to the target worktree directory containing the package.json
2. Run npm install

```bash
# Windows
cd ..\{branch-name}\frontend
npm install

# Unix/Mac
cd ../{branch-name}/frontend
npm install
```

**Important:** Install npm dependencies in ALL directories that have a package.json file (top-level, frontend/, backend/, or any other subdirectories).

## Step 8: Report Completion

Provide a summary to the user:
- Worktree created at: `../{branch-name}`
- .env files copied: [list]
- venv folders copied: [list]
- Python dependencies installed in: [list]
- npm dependencies installed in: [list]

## Platform Detection

Detect the platform using:
```bash
# Check if Windows (look for .bat or .ps1 files, or check OS)
uname -s  # Returns "MINGW64_NT" or similar on Windows Git Bash
```

Use appropriate commands (copy vs cp, robocopy vs cp -r, Scripts vs bin) based on platform.

## Error Handling

- If git worktree add fails, stop and report error
- If .env copy fails, warn but continue
- If venv copy fails, warn but continue
- If pip install fails, warn but continue
- If npm install fails, warn but continue
- Always report which steps succeeded and which failed

## Example Execution

User runs: `/create-worktree feature-auth main`

Expected output:
```
Creating worktree 'feature-auth' based on 'main'...
✓ Worktree created at ../feature-auth

Copying .env files...
✓ Copied backend/.env
✓ Copied frontend/.env

Copying venv folders...
✓ Copied backend/venv (large folder, may take a moment)

Installing Python dependencies...
✓ Installed backend/requirements.txt (15 packages)

Installing npm dependencies...
✓ Installed frontend/package.json (127 packages)
✓ Installed backend/package.json (43 packages)

Worktree setup complete! You can now work in ../feature-auth
```
