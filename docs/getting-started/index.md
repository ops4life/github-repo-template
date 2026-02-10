# Getting Started

This guide will help you understand how to use this template to create new repositories with all the built-in automation and best practices.

## What You Get

When you use this template, you get a fully configured repository with:
  
- ✅ 12 automated GitHub Actions workflows
- ✅ Secret scanning with Gitleaks
- ✅ Security analysis with CodeQL
- ✅ Automated dependency updates
- ✅ Semantic versioning and releases
- ✅ Pre-commit hooks for quality control
- ✅ Issue and PR templates
- ✅ Contributing guidelines

## Prerequisites

Before using this template, ensure you have:

- **Git** installed on your machine
- **GitHub account** with repository creation permissions
- **Python 3.7+** (for pre-commit hooks)
- **Text editor or IDE** of your choice

## Creating a Repository from This Template

### Step 1: Use This Template

1. Navigate to the [template repository](https://github.com/ops4life/github-repo-template)
2. Click the **"Use this template"** button at the top right
3. Select **"Create a new repository"**
4. Choose an owner and repository name
5. Select visibility (Public or Private)
6. Click **"Create repository from template"**

!!! tip "Template vs Fork"
    Using the template creates a clean repository without the template's commit history. This is different from forking, which preserves all history.

### Step 2: Clone Your New Repository

```bash
# Clone the repository
git clone https://github.com/your-username/your-repo-name.git

# Navigate into the repository
cd your-repo-name
```

### Step 3: Install Pre-commit Hooks

Pre-commit hooks run automated checks before each commit:

```bash
# Install pre-commit (if not already installed)
pip install pre-commit

# Install the hooks
pre-commit install

# (Optional) Run hooks on all files to test
pre-commit run --all-files
```

### Step 4: Configure Repository Settings

#### Enable GitHub Actions

GitHub Actions should be enabled by default. Verify in:

**Settings → Actions → General → Actions permissions**

Set to: "Allow all actions and reusable workflows"

#### Configure Branch Protection

Protect your main branch:

1. **Settings → Branches → Add rule**
2. **Branch name pattern**: `main` (or `master`)
3. **Enable**:
   - Require pull request reviews before merging
   - Require status checks to pass
   - Require branches to be up to date
   - Include administrators

#### Set Up Dependabot Secrets (Optional)

For auto-merge functionality:

1. **Settings → Secrets and variables → Actions**
2. Add `GH_TOKEN` with a Personal Access Token (classic)
3. Required scopes: `repo`, `workflow`

### Step 5: Customize for Your Project

#### Update Repository Information

Edit these files to reflect your project:

- **README.md** - Update project description, features, usage
- **LICENSE** - Verify license is appropriate for your project
- **CODEOWNERS** - Set code owners for your team
- **.releaserc.json** - Customize semantic release configuration

#### Configure CodeQL Languages

Edit `.github/workflows/codeql.yaml` to match your language:

```yaml
strategy:
  matrix:
    language: [ 'javascript', 'python' ]
    # Add your languages: 'cpp', 'csharp', 'go', 'java', 'ruby', 'swift'
```

#### Adjust Gitleaks Configuration

Review `.gitleaks.toml` and add any project-specific allowlists:

```toml
[allowlist]
paths = [
  '''.*test.*''',
  '''.*mock.*'''
]
```

## What's Automated?

Once set up, the following happens automatically:

### On Every Push

- ✅ Secret scanning (Gitleaks)
- ✅ Pre-commit hooks execution
- ✅ CodeQL security analysis (weekly + on push to main)

### On Pull Requests

- ✅ PR title validation (Conventional Commits)
- ✅ Pre-commit CI checks
- ✅ Dependency review
- ✅ All security scans

### On Merge to Main

- ✅ Automatic version bump (based on commits)
- ✅ Changelog generation
- ✅ GitHub release creation
- ✅ Git tag creation

### Scheduled

- ✅ Pre-commit hook updates (weekly)
- ✅ CodeQL scans (weekly)
- ✅ Stale issue management (daily)
- ✅ License year updates (yearly)

## Next Steps

- **[Quick Start Guide](quick-start.md)** - 5-minute setup
- **[Contributing Guidelines](../guides/contributing.md)** - Learn the workflow
- **[Commit Conventions](../guides/commit-conventions.md)** - Understand semantic commits
- **[GitHub Workflows](../guides/workflows.md)** - Deep dive into automation

## Troubleshooting

### Pre-commit Hooks Not Running

```bash
# Reinstall hooks
pre-commit uninstall
pre-commit install

# Update hook versions
pre-commit autoupdate
```

### GitHub Actions Not Triggering

1. Verify Actions are enabled in repository settings
2. Check workflow file syntax with `yamllint`
3. Review Actions tab for error messages

### Semantic Release Not Working

1. Ensure commits follow Conventional Commits format
2. Verify `GH_TOKEN` is configured (if required)
3. Check release workflow logs in Actions tab

## Getting Help

- **Documentation Issues** - [Report here](https://github.com/ops4life/github-repo-template/issues/new?template=documentation.md)
- **Questions** - [Start a discussion](https://github.com/ops4life/github-repo-template/discussions)
- **Bugs** - [File a bug report](https://github.com/ops4life/github-repo-template/issues/new?template=bug_report.md)
