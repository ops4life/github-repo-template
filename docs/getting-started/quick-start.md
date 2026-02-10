# Quick Start

Get up and running with this template in just 5 minutes.

## 1. Create Repository (1 minute)

Click **"Use this template"** on GitHub and create your new repository.

```bash
# Clone your new repository
git clone https://github.com/your-username/your-repo.git
cd your-repo
```

## 2. Install Pre-commit (1 minute)

```bash
# Install pre-commit
pip install pre-commit

# Install hooks in your repository
pre-commit install
```

!!! success "Verification"
    You should see: `pre-commit installed at .git/hooks/pre-commit`

## 3. Make Your First Commit (2 minutes)

```bash
# Make a change (e.g., update README)
echo "# My Project" > README.md

# Stage changes
git add README.md

# Commit with conventional commit message
git commit -m "docs: update project name"

# Push to GitHub
git push origin main
```

!!! info "Pre-commit Hooks"
    Pre-commit hooks will run automatically. The first run may take longer as it sets up environments.

## 4. Configure Repository Settings (1 minute)

### Enable Branch Protection

1. Go to **Settings → Branches**
2. Click **Add rule**
3. Set branch name pattern: `main`
4. Enable:
   - ✅ Require pull request reviews
   - ✅ Require status checks to pass

### Set Up Dependabot Auto-merge (Optional)

1. Go to **Settings → Secrets and variables → Actions**
2. Add new secret: `GH_TOKEN`
3. Value: Personal Access Token with `repo` and `workflow` scopes

## 5. Verify Automation (Bonus)

Check that everything is working:

### ✅ Pre-commit Hooks

```bash
# Run manually on all files
pre-commit run --all-files
```

All hooks should pass ✓

### ✅ GitHub Actions

1. Go to **Actions** tab
2. You should see workflows running or completed
3. All workflows should be green ✓

### ✅ Semantic Release

After your first conventional commit to main:

1. Go to **Releases** tab
2. You should see a new release created
3. Check **CHANGELOG.md** for updates

## What's Next?

Now that you're set up, explore these topics:

=== "Learn the Workflow"
    **[Contributing Guidelines](../guides/contributing.md)**

    Understand the full development workflow, from forking to merging.

=== "Master Commits"
    **[Commit Conventions](../guides/commit-conventions.md)**

    Learn how to write commits that trigger correct version bumps.

=== "Explore Automation"
    **[GitHub Workflows](../guides/workflows.md)**

    Discover all 12 automated workflows and how they work.

=== "Customize"
    **[Configuration](../reference/configuration.md)**

    Tailor the template to your project's needs.

## Common First Steps

### Update Project Information

```bash
# Edit README.md with your project details
vim README.md

# Update license if needed
vim LICENSE

# Set code owners
vim CODEOWNERS
```

### Configure for Your Language

If using Python:

```bash
# Add Python-specific pre-commit hooks
# Edit .pre-commit-config.yaml
```

If using JavaScript/Node:

```bash
# Add npm dependencies checking
# Edit .github/workflows/deps-review.yaml
```

If using Go:

```bash
# Configure CodeQL for Go
# Edit .github/workflows/codeql.yaml
```

### Create Your First Feature

```bash
# Create feature branch
git checkout -b feat/my-feature

# Make changes and commit
git add .
git commit -m "feat: add awesome feature"

# Push and create PR
git push origin feat/my-feature
```

## Quick Reference

### Commit Message Format

```
type(scope): subject

Examples:
feat: add user authentication
fix: resolve login bug
docs: update installation guide
chore: bump dependencies
```

### Version Bumps

| Commit Type | Version Change | Example |
|-------------|----------------|---------|
| `fix:` | Patch (0.0.1) | 1.0.0 → 1.0.1 |
| `feat:` | Minor (0.1.0) | 1.0.0 → 1.1.0 |
| `BREAKING CHANGE:` | Major (1.0.0) | 1.0.0 → 2.0.0 |

### Useful Commands

```bash
# Update pre-commit hooks
pre-commit autoupdate

# Run specific hook
pre-commit run gitleaks --all-files

# Skip hooks (use sparingly)
git commit --no-verify -m "message"

# Check workflow status
gh run list

# View workflow logs
gh run view
```

## Troubleshooting

### "Pre-commit not found"

```bash
# Install pre-commit
pip install pre-commit
# or
brew install pre-commit
```

### "Workflow not running"

1. Check Actions are enabled: **Settings → Actions**
2. Verify workflow syntax: `yamllint .github/workflows/*.yaml`
3. Check Actions tab for errors

### "Release not created"

1. Ensure commits follow Conventional Commits format
2. Check if `GH_TOKEN` is properly configured
3. Review release workflow logs

## Get Help

- 📚 **[Full Documentation](index.md)**
- 💬 **[Discussions](https://github.com/username/github-repo-template/discussions)**
- 🐛 **[Report Issues](https://github.com/username/github-repo-template/issues)**

---

**Congratulations!** You now have a fully automated, production-ready repository. Happy coding! 🚀
