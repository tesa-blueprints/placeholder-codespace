# tesa-blueprints

Central knowledge base and standards organization for clean, secure software development.

## Quick Start: Add Standards to Your Project

Copy a ready-made CLAUDE.md starter into your project root — Claude Code will automatically follow the rules:

```bash
# TypeScript App (React/Next.js + Node.js)
cp starters/CLAUDE-typescript-app.md /path/to/your/project/CLAUDE.md

# Terraform (Azure)
cp starters/CLAUDE-terraform.md /path/to/your/project/CLAUDE.md

# Fullstack (App + Infrastructure)
cp starters/CLAUDE-fullstack.md /path/to/your/project/CLAUDE.md
```

Also copy the custom commands for interactive workflows:

```bash
cp -r commands/ /path/to/your/project/.claude/commands/
```

Then use in any Claude Code session: `/project:review`, `/project:pre-pr`, `/project:add-feature`, etc.

For the full setup guide, see [CLAUDE-INTEGRATION-GUIDE.md](CLAUDE-INTEGRATION-GUIDE.md).

## Blueprints

| Repository | Description | For Claude |
|------------|-------------|------------|
| [blueprint-project-management](https://github.com/tesa-blueprints/blueprint-project-management) | Git workflows, issues, PRs, releases, documentation | `claude-config/CLAUDE.md` |
| [blueprint-security-advisory](https://github.com/tesa-blueprints/blueprint-security-advisory) | OWASP, clean code, secrets, scanning, repo setup | `claude-config/CLAUDE.md` |
| [blueprint-terraform-guide](https://github.com/tesa-blueprints/blueprint-terraform-guide) | Terraform + Azure, modules, state, CI/CD | `claude-config/CLAUDE.md` |
| [blueprint-application-coding-guide](https://github.com/tesa-blueprints/blueprint-application-coding-guide) | TypeScript, React, Node.js, testing, auth | `claude-config/CLAUDE.md` + frontend + backend |

## Structure

Every blueprint repo follows the same structure:

```
CLAUDE.md               Overview for Claude sessions exploring this blueprint
README.md               Overview and table of contents for humans
docs/                   Human-readable guides (numbered)
templates/              Copy-ready configs, workflows, and templates
claude-config/          Reusable CLAUDE.md files to copy into your projects
```

## Starters (Combined Rules)

Pre-built, all-in-one CLAUDE.md files that combine rules from multiple blueprints. Each starter includes a **mandatory Definition of Done checklist** that Claude verifies before every commit — ensuring tests, documentation, and security are never forgotten.

| Starter | Use Case | Sources |
|---------|----------|---------|
| [CLAUDE-typescript-app.md](starters/CLAUDE-typescript-app.md) | React/Next.js + Node.js | PM + Security + App Coding |
| [CLAUDE-terraform.md](starters/CLAUDE-terraform.md) | Terraform + Azure | PM + Security + Terraform |
| [CLAUDE-fullstack.md](starters/CLAUDE-fullstack.md) | App + Infrastructure | All 4 blueprints |

## Enforcement Model

Standards are enforced at 4 levels:

| Level | What | When | Automatic? |
|-------|------|------|------------|
| **Definition of Done** (CLAUDE.md) | Tests, docs, security, changelog | Before every commit | Claude self-checks |
| **Pre-commit Hooks** | Linting, formatting, secret detection | At commit time | Yes |
| **CI Pipeline** | Tests, security scans, npm audit, SBOM | At PR time | Yes |
| **Branch Protection + Code Review** | PR approval, CODEOWNERS | At merge time | Human review |

## Custom Commands (Slash Commands)

Copy `commands/` into your project as `.claude/commands/` to get interactive workflows:

| Command | What It Does |
|---------|-------------|
| `/project:review` | Full code review against all standards |
| `/project:security-audit` | Security-focused audit (secrets, OWASP, deps) |
| `/project:pre-pr` | Pre-PR checklist with automated checks |
| `/project:setup-repo` | Guided new repo setup with all configs |
| `/project:check-docs` | Verify documentation completeness |
| `/project:add-feature` | Guided feature workflow (issue → branch → implement → test → PR) |
| `/project:fix-bug` | Guided bug fix (issue → branch → regression test → fix → PR) |
| `/project:update-deps` | Dependency audit, updates, and cleanup |
| `/project:terraform-review` | Terraform-specific review (fmt, tags, diagnostics) |
| `/project:architecture-review` | Architecture documentation and structure review |

## This Repo

`blueprint-hub` is the central entry point and working environment (GitHub Codespace) for the tesa-blueprints organization. It contains:
- `starters/` — Ready-to-use combined CLAUDE.md files
- `commands/` — Custom slash commands for Claude Code sessions
- `CLAUDE-INTEGRATION-GUIDE.md` — Full guide on how to use blueprints with Claude Code
