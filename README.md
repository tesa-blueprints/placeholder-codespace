# tesa-blueprints

Central knowledge base and standards organization for clean, secure software development.

## Blueprints

| Repository | Description | Contents |
|------------|-------------|----------|
| [blueprint-project-management](https://github.com/tesa-blueprints/blueprint-project-management) | Project Management Standards | Documentation, GitHub Projects, Branching, Code Reviews, Releases |
| [blueprint-security-advisory](https://github.com/tesa-blueprints/blueprint-security-advisory) | Security Standards | OWASP Top 10, Clean Code, Dependencies, Secrets, Code Scanning |
| [blueprint-terraform-guide](https://github.com/tesa-blueprints/blueprint-terraform-guide) | Terraform + Azure Guide | Project Structure, Modules, State, Azure Best Practices, CI/CD Pipelines |
| [blueprint-application-coding-guide](https://github.com/tesa-blueprints/blueprint-application-coding-guide) | Application Coding Guide | TypeScript, React/Next.js, Node.js, API Design, Testing, Auth |

## Structure

Every blueprint repo follows the same structure:

```
docs/               Human-readable guides (numbered)
templates/          Copy-ready templates (configs, workflows, etc.)
claude-config/      Claude configuration files to copy into projects
CLAUDE.md           Claude rules for working on the blueprint itself
README.md           Overview and table of contents
```

## Usage

### For Humans

1. Open the relevant blueprint repo
2. Read the guides in `docs/`
3. Copy templates from `templates/` into your own project

### For Claude (AI Assistant)

1. Copy `claude-config/CLAUDE.md` from the relevant blueprint
2. Place it in the root directory of your project
3. Claude will automatically adhere to the defined standards

```bash
# Example: Copy security rules and coding guide into a project
cp blueprint-security-advisory/claude-config/CLAUDE.md my-project/CLAUDE-security.md
cp blueprint-application-coding-guide/claude-config/CLAUDE.md my-project/CLAUDE.md
```

## This Repo

`placeholder-codespace` serves as a working environment (GitHub Codespace) for managing the blueprints. It does not contain any production code.
