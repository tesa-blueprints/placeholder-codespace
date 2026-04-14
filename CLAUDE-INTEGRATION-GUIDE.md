# How to Use tesa-blueprints with Claude Code

This guide explains how to integrate the tesa-blueprints standards into your projects so that Claude Code automatically follows them.

## Quick Start

### Option A: Use a Ready-Made Starter (Recommended)

We provide pre-built, combined CLAUDE.md files in the `starters/` directory. Pick the one that matches your project type and copy it into your project root:

```bash
# For a TypeScript application (React/Next.js + Node.js)
cp starters/CLAUDE-typescript-app.md /path/to/your/project/CLAUDE.md

# For a Terraform project (Azure)
cp starters/CLAUDE-terraform.md /path/to/your/project/CLAUDE.md

# For a fullstack project (TypeScript app + Terraform infra)
cp starters/CLAUDE-fullstack.md /path/to/your/project/CLAUDE.md
```

That's it. When Claude Code opens your project, it will automatically read the `CLAUDE.md` and follow all the rules.

### Option B: Assemble Your Own

If you need a custom combination, copy individual `claude-config/CLAUDE.md` files from the relevant blueprints:

```bash
# Start with project management rules (always include)
cp blueprint-project-management/claude-config/CLAUDE.md my-project/CLAUDE.md

# Append security rules
cat blueprint-security-advisory/claude-config/CLAUDE.md >> my-project/CLAUDE.md

# Add coding rules (pick what applies)
cat blueprint-application-coding-guide/claude-config/CLAUDE.md >> my-project/CLAUDE.md
cat blueprint-application-coding-guide/claude-config/CLAUDE-frontend.md >> my-project/CLAUDE.md
cat blueprint-application-coding-guide/claude-config/CLAUDE-backend.md >> my-project/CLAUDE.md
```

## Available Starters

| File | Use Case | What's Included |
|------|----------|-----------------|
| `CLAUDE-typescript-app.md` | React/Next.js + Node.js app | Git workflow, TypeScript, frontend, backend, testing, security, documentation |
| `CLAUDE-terraform.md` | Terraform + Azure IaC | Git workflow, HCL standards, naming, tags, state, Azure patterns, security |
| `CLAUDE-fullstack.md` | App code + Infrastructure | Everything from both above, organized by domain |

## Available Blueprints

Each blueprint serves two purposes: **human-readable guides** (in `docs/`) and **Claude-readable config** (in `claude-config/`).

| Blueprint | Focus | Key Files |
|-----------|-------|-----------|
| [blueprint-project-management](https://github.com/tesa-blueprints/blueprint-project-management) | Git, issues, PRs, releases | `claude-config/CLAUDE.md` |
| [blueprint-security-advisory](https://github.com/tesa-blueprints/blueprint-security-advisory) | Security, clean code, deps | `claude-config/CLAUDE.md` |
| [blueprint-terraform-guide](https://github.com/tesa-blueprints/blueprint-terraform-guide) | Terraform + Azure | `claude-config/CLAUDE.md` |
| [blueprint-application-coding-guide](https://github.com/tesa-blueprints/blueprint-application-coding-guide) | TypeScript, React, Node.js | `claude-config/CLAUDE.md`, `CLAUDE-frontend.md`, `CLAUDE-backend.md` |

## How It Works

1. Claude Code reads `CLAUDE.md` from your project root when it opens a session
2. The rules in that file guide Claude's behavior: how it writes code, creates commits, handles errors, etc.
3. Claude follows these rules automatically — you don't need to repeat them in every prompt

## What If I Need to Customize?

The starters are a starting point. You can:

- **Remove sections** that don't apply to your project
- **Add project-specific rules** at the bottom of the file
- **Override rules** — rules later in the file take precedence

Example: Adding a project-specific rule:

```markdown
# ... (existing rules from starter) ...

## Project-Specific Rules

- This project uses Drizzle ORM instead of Prisma
- The API is GraphQL, not REST — use code-first schema with Pothos
- Feature flags are managed via Azure App Configuration
```

## Templates & Workflows

Beyond the CLAUDE.md files, the blueprints also contain **templates** you should copy into your project:

```bash
# GitHub Issue + PR Templates
cp -r blueprint-project-management/templates/.github/ my-project/.github/

# Security workflows (CodeQL, Dependabot, Dependency Review)
cp blueprint-security-advisory/templates/.github/dependabot.yml my-project/.github/
cp -r blueprint-security-advisory/templates/.github/workflows/ my-project/.github/workflows/

# Pre-commit hooks
cp blueprint-security-advisory/templates/.pre-commit-config.yaml my-project/

# TypeScript configs
cp blueprint-application-coding-guide/templates/tsconfig.json my-project/
cp blueprint-application-coding-guide/templates/.eslintrc.json my-project/
cp blueprint-application-coding-guide/templates/.prettierrc my-project/

# CI/CD pipeline
cp -r blueprint-application-coding-guide/templates/.github/workflows/ my-project/.github/workflows/
```

## How Enforcement Works

The starters include a **Definition of Done** section at the bottom — a mandatory checklist that Claude verifies before every commit. This is the key enforcement mechanism.

### The 4 Enforcement Levels

```
Level 1: Definition of Done (CLAUDE.md)
  → Claude self-checks: tests, docs, security, changelog before every commit
    ↓
Level 2: Pre-commit Hooks (husky + lint-staged)
  → Automatic: linting, formatting, secret detection at commit time
    ↓
Level 3: CI Pipeline (GitHub Actions)
  → Automatic: tests, security scans, npm audit, SBOM at PR time
    ↓
Level 4: Branch Protection + Code Review
  → Human: PR approval, CODEOWNERS review at merge time
```

### What the Definition of Done Enforces

Every starter ends with a block like this that Claude reads and follows:

- Tests exist for every new feature and bug fix
- All tests pass before committing
- README and docs are updated when features/architecture change
- Changelog is maintained
- No secrets in code
- Commit messages follow Conventional Commits with issue references
- Architecture documentation is updated when architecture changes

Claude will not report a task as "done" until all applicable items are confirmed.

## New Repository Checklist

When creating a new repository, follow these steps:

1. Copy the appropriate starter CLAUDE.md into the project root
2. Copy GitHub templates (issues, PRs, CODEOWNERS)
3. Copy CI/CD workflows and security scans
4. Copy pre-commit config for local enforcement
5. Follow the mandatory setup checklist in [blueprint-security-advisory/docs/09-github-repository-setup.md](https://github.com/tesa-blueprints/blueprint-security-advisory/blob/main/docs/09-github-repository-setup.md)
6. Enable Branch Protection, Dependabot, CodeQL, and Secret Scanning in GitHub settings
7. Verify: Run through the Definition of Done checklist manually once to confirm everything works
