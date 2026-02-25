# Security Policy

## Scope

This repository contains Claude Code skills — Markdown files that instruct AI agents how to perform development tasks. There is no compiled code, server infrastructure, or user data involved.

Security concerns that are in scope:
- A skill that instructs an agent to perform destructive or dangerous actions
- A skill that could cause an agent to leak credentials or secrets
- A skill that bypasses security checks (e.g., `--no-verify` on commits without warning)

## Reporting a Vulnerability

If you discover a skill that could cause harmful behavior, please report it via [GitHub Issues](https://github.com/giladresisi/ai-dev-env/issues) with the label `security`.

For sensitive reports that should not be public, open a blank issue titled "Security report" and request a private channel — a maintainer will follow up directly.

## Response

Reported issues will be reviewed promptly. Skills confirmed to cause harmful behavior will be patched or removed.
