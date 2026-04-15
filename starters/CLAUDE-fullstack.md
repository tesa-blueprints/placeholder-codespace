# Project Rules — Fullstack (TypeScript App + Terraform)

> This file was generated from the [tesa-blueprints](https://github.com/tesa-blueprints) organization.
> Sources: All 4 blueprints combined.
> Use case: Fullstack project with TypeScript application code AND Terraform infrastructure.

## Available Commands

This project has custom slash commands in `.claude/commands/`. Use them:

| Command | When to Use |
|---------|-------------|
| `/project:add-feature {name}` | Starting new feature work — guides full workflow |
| `/project:fix-bug {name}` | Starting a bug fix — regression test first, then fix |
| `/project:pre-pr` | Before creating a PR — runs all quality checks |
| `/project:review` | After completing work — full code review against these rules |
| `/project:security-audit` | Before release or when touching auth/data/IAM code |
| `/project:terraform-review` | After Terraform changes — fmt, naming, tags, diagnostics |
| `/project:check-docs` | After adding features — verify docs are complete |
| `/project:update-deps` | Monthly or when dependency PRs pile up |
| `/project:architecture-review` | After architecture changes |

**These commands are the standard way to execute workflows.** Don't do a code review manually — run `/project:review`. Don't create a PR without `/project:pre-pr`. Don't start a feature without `/project:add-feature`.

---

## Language

- All documentation, comments, commit messages, PR descriptions, and issue descriptions must be written in English. No exceptions.

## Git & Workflow

- Every code change must have an associated GitHub Issue — no exceptions. Create the issue first, then code.
- Issues must be created in THIS repository — never in a central/separate repo. For cross-repo references use `org/repo#123`.
- Repo naming: `{domain}-{component}` lowercase with hyphens. Terraform repos: prefix `terraform-` or `terraform-module-`. Secrets: `{SERVICE}_{PURPOSE}` UPPER_SNAKE_CASE. Environments: full names (`production`, not `prod`).
- Always work on feature branches, never directly on `main`
- Branch naming: `{type}/{description}` — e.g., `feature/user-profile`, `fix/login-redirect`, `chore/update-deps`
- Allowed prefixes: `feature/`, `fix/`, `chore/`, `docs/`, `refactor/`, `test/`
- Use Conventional Commits: `{type}({scope}): {description}`
- Every branch merges into `main` via Pull Request with at least one approval
- PR size: ideally 100-300 lines, maximum 500 lines
- Reference issues with `Closes #123` in PRs and commits

---

## Application Code

### TypeScript

- Use TypeScript strict mode — never disable strict checks
- No `any` — use `unknown` and narrow with type guards
- Prefer `const` over `let`, never use `var`
- Use named exports, avoid default exports (exception: Next.js Pages/Layouts)
- Prefer interfaces for object shapes, types for unions/intersections
- Use type-only imports: `import type { User } from './types'`

### Code Style

- Keep functions under 20 lines — extract when they exceed that
- Max. 3 parameters — use an options object beyond that
- Use early returns instead of deep nesting
- No empty catch blocks — always handle or re-throw errors
- No magic numbers — use named constants
- No `console.log` in production code — use a structured logger
- Import order: Node.js built-ins → External packages → Internal modules → Relative imports

### Naming (Application)

- PascalCase: Types, Interfaces, Classes, React Components
- camelCase: Variables, Functions, Methods
- UPPER_SNAKE_CASE: Constants
- kebab-case: Filenames (except React Components: PascalCase.tsx)

### Frontend (React / Next.js)

- Server Components by default — `'use client'` only when needed
- One component per file (PascalCase.tsx), colocate tests and types
- Semantic HTML, all images with `alt`, use `next/image`
- Forms: React Hook Form + Zod
- State: Server Components > Context > Zustand. No Redux.
- Tailwind CSS, mobile-first
- Suspense Boundaries for loading, Error Boundaries for errors

### Backend (Node.js)

- Layered Architecture: Route Handler → Service → Repository
- Validate all inputs with Zod at the route handler level
- Consistent error format: `{ error: { code: "...", message: "..." } }`
- Health check at `GET /health`, readiness check at `GET /health/ready`
- Structured JSON logging with pino + correlation IDs
- Graceful shutdown (SIGTERM/SIGINT handlers)
- Rate limiting on public endpoints, explicit CORS config

### Testing

- Every new feature must include automated tests — no exceptions
- Every bug fix must include a regression test
- When modifying features, update existing tests to reflect new behavior
- Tests must pass in CI before merging — never skip or disable
- Coverage targets: >= 80% for business logic

### Database (Prisma)

- Always use migrations — never change schemas manually
- Avoid N+1: use `include` or `select`
- Transactions for related operations, soft delete for business data
- Index foreign keys and frequently queried fields

---

## Infrastructure (Terraform + Azure)

### Formatting

- Run `terraform fmt` before every commit
- Use snake_case for all resource names, variables, and outputs

### Variables & Modules

- Every variable must have `type` and `description` with `validation` blocks
- Pin module versions to Git tags (`ref=v1.2.0`), never `ref=main`
- Keep modules focused: one concern per module

### Resource Naming

- Azure resources: `{prefix}-{env}-{name}` format (e.g., `rg-prod-platform`)
- Tag ALL Azure resources with: environment, project, owner, cost-center, managed-by=terraform

### State

- Remote state in Azure Storage — never commit .tfstate files
- Commit `.terraform.lock.hcl`, never commit `terraform.tfvars`

### Azure Best Practices

- Prefer Managed Identities over Service Principals
- Use Private Endpoints for PaaS services
- Enable Diagnostic Settings for ALL resources — send logs to Log Analytics Workspace
- Use `for_each` over `count`, `locals` for computed values, `data` sources for existing resources

### Terraform CI/CD

- Plan on PRs, apply only on merge into main
- Never `cancel-in-progress: true` for Terraform
- OIDC for Azure auth, approval gates for production

---

## Security (Global)

- Never commit secrets, API keys, passwords, tokens, or certificates
- Use environment variables for config, `.env.example` for documentation
- Validate all user inputs at system boundaries using Zod
- Use parameterized queries — never string concatenation for SQL
- Use httpOnly, secure, sameSite cookies for session tokens
- Pin production dependencies to exact or tilde versions
- Run `npm audit` before adding dependencies
- Never disable ESLint security rules or bypass pre-commit hooks
- Never return stack traces or system information to end users

## Documentation

- Every project must have architecture documentation (diagram + description)
- Update README.md when adding features or changing architecture
- Write an ADR for architecture decisions
- Maintain the changelog under `[Unreleased]`

---

## Definition of Done — Mandatory Pre-Commit Checklist

**CRITICAL: Before completing ANY task, you MUST verify every item on this checklist. Do not report a task as done until all applicable items are confirmed. This is not optional.**

### After EVERY application code change:

- [ ] **Tests:** New/modified code has corresponding automated tests (feature → test, bug fix → regression test, behavior change → updated tests)
- [ ] **Tests pass:** `npm test` passes with zero failures
- [ ] **Linting:** `eslint . --max-warnings 0` passes
- [ ] **Type check:** `tsc --noEmit` passes
- [ ] **No secrets:** No API keys, passwords, tokens, or credentials in the code

### After EVERY infrastructure change:

- [ ] **Formatting:** `terraform fmt -check -recursive` passes
- [ ] **Validation:** `terraform validate` passes
- [ ] **Linting:** `tflint --recursive` passes
- [ ] **Plan:** `terraform plan` reviewed — no unexpected destroys or changes
- [ ] **Tags:** All new resources have mandatory tags
- [ ] **Diagnostics:** All new resources have Diagnostic Settings configured

### After EVERY change (application or infrastructure):

- [ ] **Documentation:** README.md is updated if you added a feature, changed the API, modified configuration, or altered the architecture
- [ ] **Changelog:** Feature, fix, or breaking change → entry added under `[Unreleased]`
- [ ] **Architecture:** If architecture changed, documentation/diagram is updated
- [ ] **Issue reference:** Commit message references the issue (`Closes #123`)
- [ ] **Conventional Commit:** Message format `{type}({scope}): {description}`

### Self-check prompt:

Before you say "done", ask yourself:
1. If a new developer clones this repo tomorrow, will the README and docs be accurate?
2. If this code breaks in production, will the tests catch it?
3. If a security auditor reviews this commit, will they find any issues?
4. If someone runs `terraform plan`, will there be surprises?

If the answer to any of these is "no" — you are not done.
