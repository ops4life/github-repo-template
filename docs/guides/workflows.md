# GitHub Workflows

This template includes 12 automated GitHub Actions workflows that provide comprehensive CI/CD, security, and maintenance automation. This guide explains each workflow, when it runs, and how to customize it.

## Workflow Overview

| Workflow | Purpose | Trigger | Frequency |
|----------|---------|---------|-----------|
| [Auto-merge PR](#auto-merge-pr) | Auto-merge approved PRs | Pull request | On demand |
| [Cleanup Caches](#cleanup-caches) | Remove PR caches | PR closed | On demand |
| [CodeQL](#codeql) | Security analysis | Push, PR, Schedule | Weekly + events |
| [Dependency Review](#dependency-review) | Supply chain security | Pull request | Per PR |
| [Gitleaks](#gitleaks) | Secret scanning | Push, PR | Per push/PR |
| [Lint PR](#lint-pr) | PR title validation | Pull request | Per PR |
| [Pre-commit Auto-update](#pre-commit-auto-update) | Update hooks | Schedule | Weekly |
| [Pre-commit CI](#pre-commit-ci) | Run pre-commit checks | Push, PR | Per push/PR |
| [Release](#release) | Semantic versioning | Push to main | Per merge |
| [Stale](#stale) | Issue management | Schedule | Daily |
| [Template Sync](#template-sync) | Sync template updates | Schedule, Manual | Weekly |
| [Update License](#update-license) | License year update | Schedule | Yearly |

## Security Workflows

### CodeQL

**Advanced semantic security analysis**

```yaml
# .github/workflows/codeql.yaml
name: CodeQL
on:
  push:
    branches: [main, master]
  pull_request:
    branches: [main, master]
  schedule:
    - cron: '0 0 * * 1'  # Weekly on Mondays
```

**What it does:**

- Analyzes code for security vulnerabilities
- Detects: SQL injection, XSS, command injection, path traversal
- Supports multiple languages (JavaScript, Python by default)
- Creates security alerts in the Security tab

**Configuration:**

```yaml
strategy:
  matrix:
    language: ['javascript', 'python']
    # Add your languages:
    # 'cpp', 'csharp', 'go', 'java', 'ruby', 'swift'
```

**Customization:**

Edit language matrix to match your project:

```yaml
# For a Go project
matrix:
  language: ['go']

# For a full-stack project
matrix:
  language: ['javascript', 'python', 'java']
```

**View results:** Repository → Security tab → Code scanning alerts

### Gitleaks

**Secret scanning and leak prevention**

```yaml
# .github/workflows/gitleaks.yaml
name: Gitleaks
on:
  push:
    branches: [main, master]
  pull_request:
    branches: [main, master]
```

**What it does:**

- Scans for accidentally committed secrets
- Detects: API keys, passwords, tokens, certificates
- Blocks PRs containing secrets
- Runs on every push and PR

**Configuration:**

Edit `.gitleaks.toml` to customize:

```toml
# Add allowlists for false positives
[allowlist]
description = "Allowlist for false positives"

paths = [
  '''.*test.*''',
  '''.*mock.*'''
]

regexes = [
  '''test_key_.*'''
]
```

**Manual execution:**

```bash
# Run locally
gitleaks detect --source . --verbose

# Run via pre-commit
pre-commit run gitleaks --all-files
```

### Dependency Review

**Supply chain security for pull requests**

```yaml
# .github/workflows/deps-review.yaml
name: Dependency Review
on:
  pull_request:
    branches: [main, master]
```

**What it does:**

- Reviews dependencies added in PRs
- Checks for known vulnerabilities
- Validates license compatibility
- Blocks high-severity vulnerabilities

**View results:** PR checks → Dependency Review

## Quality Workflows

### Pre-commit CI

**Automated code quality checks**

```yaml
# .github/workflows/pre-commit-ci.yaml
name: Pre-commit CI
on:
  push:
  pull_request:
```

**What it does:**

- Runs all pre-commit hooks in CI
- Validates code formatting
- Checks YAML and Markdown syntax
- Ensures consistent code style

**Included hooks:**

- EditorConfig validation
- Trailing whitespace removal
- YAML linting
- Markdown linting
- Gitleaks secret scanning

**Local execution:**

```bash
# Run all hooks
pre-commit run --all-files

# Run specific hook
pre-commit run yamllint --all-files
```

### Lint PR

**Pull request title validation**

```yaml
# .github/workflows/lint-pr.yaml
name: Lint PR
on:
  pull_request:
    types: [opened, edited, synchronize]
```

**What it does:**

- Validates PR title follows Conventional Commits
- Ensures proper formatting
- Blocks PRs with invalid titles
- Required for automated releases

**Valid PR titles:**

```bash
✅ feat: add user authentication
✅ fix(api): resolve null pointer exception
✅ docs: update installation guide
✅ chore(deps): bump dependencies

❌ Added feature           # No type
❌ Fix bug                 # Capitalized
❌ feat: Added feature.    # Past tense, period
```

**Configuration:**

Edit `.github/workflows/lint-pr.yaml`:

```yaml
# Customize allowed types
types: |
  feat
  fix
  docs
  style
  refactor
  test
  chore
  ci
```

## Automation Workflows

### Release

**Automated semantic versioning and releases**

```yaml
# .github/workflows/release.yaml
name: Release
on:
  push:
    branches: [main]
  workflow_dispatch:
```

**What it does:**

- Analyzes commit messages
- Determines version bump (major/minor/patch)
- Generates CHANGELOG.md
- Creates GitHub release
- Creates git tag

**Version bumping:**

| Commit Type | Version Change | Example |
|-------------|----------------|---------|
| `fix:` | Patch (0.0.1) | 1.0.0 → 1.0.1 |
| `feat:` | Minor (0.1.0) | 1.0.0 → 1.1.0 |
| `BREAKING CHANGE:` | Major (1.0.0) | 1.0.0 → 2.0.0 |

**Configuration:**

Edit `.releaserc.json`:

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    "@semantic-release/github",
    "@semantic-release/git"
  ]
}
```

**Manual trigger:**

Actions tab → Release workflow → Run workflow

### Auto-merge PR

**Automatically merge approved pull requests**

```yaml
# .github/workflows/automerge.yml
name: Auto-merge PR
on: pull_request_target
```

**What it does:**

- Auto-enables merge for specific PRs
- Merges Dependabot PRs automatically
- Merges automated update PRs
- Uses squash merge method

**Auto-merges for:**

- `github.actor == 'dependabot[bot]'`
- `github.head_ref == 'update-pre-commit-hooks'`
- `startsWith(github.head_ref, 'update-license-')`

**Requirements:**

- All status checks must pass
- Branch must be up to date
- PR must be approved (if required by branch protection)

**Setup:**

1. Create Personal Access Token (classic)
2. Add to repository secrets: `GH_TOKEN`
3. Required scopes: `repo`, `workflow`

Settings → Secrets and variables → Actions → New repository secret

### Pre-commit Auto-update

**Keep pre-commit hooks up to date**

```yaml
# .github/workflows/pre-commit-auto-update.yaml
name: Pre-commit Auto-update
on:
  schedule:
    - cron: '0 0 * * 1'  # Weekly on Mondays
  workflow_dispatch:
```

**What it does:**

- Updates pre-commit hook versions weekly
- Creates PR with updates
- Runs tests to verify compatibility
- Auto-merges if all checks pass

**Manual trigger:**

Actions tab → Pre-commit Auto-update → Run workflow

## Maintenance Workflows

### Cleanup Caches

**Remove workflow caches for closed PRs**

```yaml
# .github/workflows/cleanup-caches.yaml
name: Cleanup caches
on:
  pull_request_target:
    types: [closed]
```

**What it does:**

- Cleans up GitHub Actions caches
- Runs when PRs are closed
- Frees up cache storage
- Prevents cache bloat

**Benefits:**

- Reduces storage costs
- Improves cache efficiency
- Keeps workspace clean

### Stale

**Manage inactive issues and pull requests**

```yaml
# .github/workflows/stale.yaml
name: Stale
on:
  schedule:
    - cron: '0 0 * * *'  # Daily at midnight
```

**What it does:**

- Labels inactive issues as "stale"
- Labels inactive PRs as "stale"
- Closes issues stale for too long
- Encourages issue resolution

**Configuration:**

Edit `.github/workflows/stale.yaml`:

```yaml
days-before-stale: 60
days-before-close: 7
stale-issue-label: 'stale'
stale-pr-label: 'stale'
exempt-issue-labels: 'pinned,security'
```

**Exempt issues:**

Add labels to prevent closing:

- `pinned` - Keep open indefinitely
- `security` - Security issues
- `help wanted` - Awaiting contributions

### Template Sync

**Sync updates from template repository**

```yaml
# .github/workflows/template-repo-sync.yaml
name: Template Sync
on:
  schedule:
    - cron: '0 0 * * 1'  # Weekly on Mondays
  workflow_dispatch:
```

**What it does:**

- Pulls updates from template repository
- Creates PR with template changes
- Respects `.templatesyncignore` patterns
- Allows selective sync

**Configuration:**

Edit `.templatesyncignore` to exclude files:

```
# Exclude customized files
README.md
LICENSE
docs/**
mkdocs.yml
requirements.txt
```

**Manual trigger:**

Actions tab → Template Sync → Run workflow

### Update License

**Keep LICENSE year current**

```yaml
# .github/workflows/update-license.yml
name: Update License
on:
  schedule:
    - cron: '0 0 1 1 *'  # Yearly on January 1st
  workflow_dispatch:
```

**What it does:**

- Updates copyright year in LICENSE
- Creates PR with updated year
- Runs automatically on January 1st
- Auto-merges after checks pass

**Manual trigger:**

Actions tab → Update License → Run workflow

## Workflow Permissions

All workflows use minimal required permissions:

```yaml
# Example: Minimal permissions
permissions:
  contents: read
  pull-requests: write
```

**Common permissions:**

- `contents: read` - Read repository contents
- `contents: write` - Create commits, tags, releases
- `pull-requests: write` - Create, update PRs
- `security-events: write` - Create security alerts
- `issues: write` - Create, update issues

## Customization Guide

### Add a New Workflow

1. Create file in `.github/workflows/`
2. Define trigger and permissions
3. Add jobs and steps
4. Test with workflow_dispatch
5. Commit and push

```yaml
name: My Custom Workflow

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read

jobs:
  my-job:
    runs-on: ubuntu-24.04
    steps:
      - uses: actions/checkout@v6
      - name: My Step
        run: echo "Hello World"
```

### Modify Existing Workflow

1. Edit workflow file in `.github/workflows/`
2. Test changes with `workflow_dispatch`
3. Create PR to review changes
4. Merge when ready

### Disable a Workflow

Option 1: Delete the workflow file

```bash
git rm .github/workflows/stale.yaml
git commit -m "chore: disable stale workflow"
```

Option 2: Disable in GitHub UI

Repository → Actions → Select workflow → ⋯ → Disable workflow

## Troubleshooting

### Workflow Not Triggering

**Check:**

1. Actions enabled: Settings → Actions → General
2. Workflow file syntax: `yamllint .github/workflows/*.yaml`
3. Trigger conditions: Review `on:` section
4. Branch names: Verify correct branch patterns

### Workflow Failing

**Debug steps:**

1. View logs: Actions tab → Select run → Expand steps
2. Check permissions: Review `permissions:` section
3. Verify secrets: Settings → Secrets and variables
4. Test locally: Reproduce steps in local environment

### Semantic Release Not Working

**Common issues:**

- ❌ Commits don't follow Conventional Commits
- ❌ `GH_TOKEN` not configured
- ❌ No commits since last release
- ❌ Commits are documentation/chore only (don't trigger releases)

**Solutions:**

```bash
# Check commit format
git log --oneline

# Ensure conventional commits
git commit --amend

# Trigger release manually
# Actions → Release → Run workflow
```

## Best Practices

### Security

- ✅ Use minimal permissions
- ✅ Pin actions to specific SHA
- ✅ Don't expose secrets in logs
- ✅ Review third-party actions before use

### Performance

- ✅ Use caching for dependencies
- ✅ Run jobs in parallel when possible
- ✅ Use path filters to skip unnecessary runs
- ✅ Clean up old caches regularly

### Maintenance

- ✅ Keep actions up to date
- ✅ Monitor workflow run times
- ✅ Review failed runs promptly
- ✅ Document custom workflows

## Additional Resources

- **[GitHub Actions Documentation](https://docs.github.com/en/actions)** - Official docs
- **[GitHub Actions Marketplace](https://github.com/marketplace?type=actions)** - Find actions
- **[Semantic Release](https://semantic-release.gitbook.io/)** - Release automation
- **[Conventional Commits](https://www.conventionalcommits.org/)** - Commit format

---

**Questions?** Open a [discussion](https://github.com/username/github-repo-template/discussions) or [issue](https://github.com/username/github-repo-template/issues).
