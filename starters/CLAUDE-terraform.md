# Project Rules — Terraform + Azure

> This file was generated from the [tesa-blueprints](https://github.com/tesa-blueprints) organization.
> Sources: blueprint-project-management, blueprint-security-advisory, blueprint-terraform-guide.
> Use case: Terraform Infrastructure as Code with Azure as the hyperscaler.

## Available Commands

This project has custom slash commands in `.claude/commands/`. Use them:

| Command | When to Use |
|---------|-------------|
| `/project:terraform-review` | After Terraform changes — checks fmt, naming, tags, diagnostics |
| `/project:pre-pr` | Before creating a PR — runs all quality checks |
| `/project:review` | After completing work — full code review |
| `/project:security-audit` | Before release or when changing IAM/networking |
| `/project:check-docs` | After adding resources — verify docs are complete |
| `/project:add-feature {name}` | Starting new infrastructure work |
| `/project:fix-bug {name}` | Fixing infrastructure issues |

**These commands are the standard way to execute workflows.** Don't review Terraform manually — run `/project:terraform-review`. Don't create a PR without running `/project:pre-pr`.

---

## Language

- All documentation, comments, commit messages, PR descriptions, and issue descriptions must be written in English. No exceptions.

## Git & Workflow

- Every code change must have an associated GitHub Issue — no exceptions
- Issues must be created in THIS repository — never in a central/separate repo. For cross-repo references use `org/repo#123`.
- Naming: Orgs `tesa-{domain}`, Repos `tesa-{domain}-{component}`, Terraform repos `tesa-terraform-` or `tesa-terraform-module-` prefix, Secrets `{SERVICE}_{PURPOSE}` UPPER_SNAKE_CASE, Environments full names (`production`, not `prod`).
- Always work on feature branches: `feature/`, `fix/`, `chore/`, `docs/`
- Use Conventional Commits: `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`
- Every branch merges into `main` via Pull Request with at least one approval
- Reference issues with `Closes #123` in PRs and commits

## Formatting & Syntax

- Run `terraform fmt` before every commit
- Use snake_case for all Terraform resource names, variables, and outputs
- Use 2 spaces for indentation (Terraform standard)

## Variables

- Every variable must have `type` and `description`
- Use `validation` blocks for input constraints
- Mark secrets with `sensitive = true`
- Set sensible defaults only where there is a clear standard choice
- No default values for environment-specific variables (environment, project, owner)

## Resource Naming

- Azure resources: `{prefix}-{env}-{name}` format (e.g., `rg-prod-platform`)
- Storage Accounts and Container Registries: no hyphens (Azure restriction)
- Terraform resource names: descriptive, `this` only when there is exactly one instance

## Tags

- Tag ALL Azure resources with: environment, project, owner, cost-center, managed-by=terraform
- Use `local.common_tags` and assign them to every resource
- Do not add tags that change frequently

## State

- Remote state in Azure Storage — never commit .tfstate files
- State key format: `{environment}/{project}.tfstate`
- Commit `.terraform.lock.hcl` (provider lockfile)
- Never commit `terraform.tfvars` (contains real values) — use `.tfvars.example`

## Modules

- Always pin module versions to a Git tag (`ref=v1.2.0`), never to `ref=main`
- Every module has: main.tf, variables.tf, outputs.tf, versions.tf, README.md
- Document all outputs with `description`
- Keep modules focused: one concern per module
- Max. 2 levels of module nesting

## Patterns & Best Practices

- Use `for_each` instead of `count` for resources that need stable addressing
- Use `locals` for computed values — no repeated expressions
- Use `data` sources for existing resources — no hardcoded IDs
- Pin provider versions with pessimistic constraint (`~>`)
- Group files logically: main.tf, variables.tf, outputs.tf, locals.tf, data.tf, providers.tf, versions.tf, backend.tf

## Azure-Specific

- Prefer Managed Identities over Service Principals
- Use Private Endpoints for PaaS services
- Enable Diagnostic Settings for ALL resources that support them — this is mandatory
- Send all logs to a central Log Analytics Workspace
- Use `category_group = "allLogs"` and `metric { category = "AllMetrics" }` as default
- Set `prevent_deletion_if_contains_resources = true` for Resource Groups
- Set `purge_soft_delete_on_destroy = false` for Key Vaults

## CI/CD

- Plan on PRs, apply only on merge into main
- Never use `cancel-in-progress: true` for Terraform pipelines
- Use OIDC for Azure authentication in GitHub Actions
- Approval gates for production environments

## Security

- Never commit secrets, credentials, or API keys
- Use Azure Key Vault for production secrets, Managed Identities for access
- Enable Dependabot, CodeQL, Secret Scanning, and Push Protection on every repo
- Pin GitHub Actions to commit SHA, never to tags

## Documentation

- Every project must have a README with architecture overview
- Update README when infrastructure changes
- Write an ADR for significant architecture decisions

---

## Definition of Done — Mandatory Pre-Commit Checklist

**CRITICAL: Before completing ANY task, you MUST verify every item on this checklist. Do not report a task as done until all applicable items are confirmed. This is not optional.**

### After EVERY infrastructure change:

- [ ] **Formatting:** `terraform fmt -check -recursive` passes with no changes needed.
- [ ] **Validation:** `terraform validate` passes without errors.
- [ ] **Linting:** `tflint --recursive` passes without errors.
- [ ] **Plan review:** `terraform plan` has been run and the output reviewed. No unexpected destroys or changes.
- [ ] **Tags:** All new resources have the mandatory tags (environment, project, owner, cost-center, managed-by).
- [ ] **Diagnostic Settings:** All new resources that support Diagnostic Settings have them configured.
- [ ] **Documentation:** README.md is updated if you added new resources, changed the architecture, or modified inputs/outputs.
- [ ] **Variables:** All new variables have `type`, `description`, and `validation` blocks where appropriate.
- [ ] **No secrets:** No hardcoded credentials, connection strings, or keys in .tf files. Check for hardcoded strings.
- [ ] **State:** No .tfstate files staged. `.terraform.lock.hcl` is committed if provider versions changed.
- [ ] **Issue reference:** The commit message references the issue with `Closes #123` or `Fixes #456`.
- [ ] **Conventional Commit:** The commit message follows the format `{type}({scope}): {description}`.

### Self-check prompt:

Before you say "done", ask yourself:
1. If someone runs `terraform plan` on this code, will there be surprises?
2. If a new team member reads the README, will they understand the current infrastructure?
3. Are all resources tagged and logging to Log Analytics?

If the answer to any of these is "no" — you are not done.
