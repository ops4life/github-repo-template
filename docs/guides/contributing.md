# Contributing Guidelines

Thank you for your interest in contributing to this project! This document provides guidelines and instructions for contributing.

## Table of Contents

- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Contribution Workflow](#contribution-workflow)
- [Commit Message Guidelines](#commit-message-guidelines)
- [Pull Request Process](#pull-request-process)
- [Code Style](#code-style)
- [Code of Conduct](#code-of-conduct)

## Getting Started

### 1. Fork the Repository

Click the "Fork" button at the top right of the repository page. This creates a copy in your GitHub account.

### 2. Clone Your Fork

```bash
git clone https://github.com/YOUR-USERNAME/REPOSITORY-NAME.git
cd REPOSITORY-NAME
```

### 3. Add Upstream Remote

```bash
git remote add upstream https://github.com/ORIGINAL-OWNER/REPOSITORY-NAME.git
```

This allows you to sync your fork with the original repository.

## Development Setup

### 1. Install Pre-commit Hooks (Required)

Pre-commit hooks ensure code quality before commits:

```bash
pre-commit install
```

### 2. Verify Installation

```bash
pre-commit run --all-files
```

All hooks should pass. If they fail, fix the issues and try again.

### 3. Create a Feature Branch

```bash
git checkout -b feat/your-feature-name
```

!!! tip "Branch Naming"
    Use descriptive branch names that indicate the type of change:

    - `feat/add-authentication`
    - `fix/resolve-login-bug`
    - `docs/update-readme`
    - `refactor/improve-error-handling`

## Contribution Workflow

### 1. Keep Your Fork Updated

Before starting new work, sync with upstream:

```bash
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

### 2. Make Your Changes

- Write clear, concise code
- Follow existing code patterns and conventions
- Add tests if applicable
- Update documentation as needed
- Keep changes focused and atomic

### 3. Test Your Changes

```bash
# Run pre-commit hooks
pre-commit run --all-files

# Run any project-specific tests
# (add commands based on your project)
```

!!! warning "Pre-commit Must Pass"
    All pre-commit hooks must pass before submitting a PR. This ensures consistent code quality across the project.

### 4. Commit Your Changes

Follow the [Commit Message Guidelines](#commit-message-guidelines):

```bash
# Stage specific files
git add file1.js file2.py

# Commit with conventional message
git commit -m "feat(auth): add OAuth2 authentication"
```

Keep commits:

- **Atomic** - Each commit should represent a single logical change
- **Focused** - Don't mix unrelated changes
- **Well-described** - Use clear, descriptive messages

### 5. Push to Your Fork

```bash
git push origin feat/your-feature-name
```

### 6. Create a Pull Request

1. Go to your fork on GitHub
2. Click **"Compare & pull request"**
3. Fill out the pull request template completely
4. Link any related issues (e.g., "Closes #123")
5. Request review from maintainers

## Commit Message Guidelines

This project follows [Conventional Commits](https://www.conventionalcommits.org/) format for automated versioning and changelog generation.

### Format

```
<type>(<scope>): <subject>

[optional body]

[optional footer]
```

### Commit Types

| Type | Description | Version Impact |
|------|-------------|----------------|
| `feat` | New feature | Minor (0.1.0) |
| `fix` | Bug fix | Patch (0.0.1) |
| `docs` | Documentation only | None |
| `style` | Code style (formatting, etc.) | None |
| `refactor` | Code refactoring | None |
| `test` | Adding/updating tests | None |
| `chore` | Build process, dependencies | None |
| `ci` | CI/CD configuration | None |
| `revert` | Reverting a previous commit | None |

### Breaking Changes

Include `BREAKING CHANGE:` in the footer or `!` after type:

```bash
feat(api)!: change response format

BREAKING CHANGE: API responses now return data in camelCase instead of snake_case
```

This triggers a major version bump (1.0.0).

### Examples

#### Feature with Scope

```bash
feat(auth): add OAuth2 authentication

Implements OAuth2 authentication flow with Google and GitHub providers.
Includes token refresh and logout functionality.

Closes #45
```

#### Bug Fix

```bash
fix(api): resolve null pointer exception in user endpoint

Added null check before accessing user.email property.
Prevents 500 errors when user email is not set.
```

#### Documentation

```bash
docs: update README with installation instructions

Added step-by-step installation guide with prerequisites and troubleshooting section.
```

#### Multiple Scopes

```bash
feat(ui,api): implement user profile page

- Add profile page component in React
- Create GET /api/users/:id endpoint
- Add profile update form with validation
```

### Commit Message Rules

- ✅ Subject must start with an alphabetic character
- ✅ Use imperative mood ("add feature" not "added feature")
- ✅ Don't capitalize the first letter of the subject
- ✅ No period at the end of the subject
- ✅ Keep subject line under 72 characters
- ✅ Separate subject from body with a blank line
- ✅ Wrap body at 72 characters

## Pull Request Process

### Before Submitting

- [ ] All tests pass
- [ ] Pre-commit hooks pass successfully
- [ ] Documentation is updated
- [ ] Commit messages follow Conventional Commits
- [ ] Branch is up to date with main

### Pull Request Requirements

1. **Title Format**
   - Must follow Conventional Commits format
   - Example: `feat(auth): add OAuth2 authentication`

2. **Description**
   - Clearly explain what changes were made and why
   - Include screenshots for UI changes
   - List any breaking changes

3. **Linked Issues**
   - Use keywords: `fixes #123`, `closes #456`, `resolves #789`
   - Or: `related to #123`, `part of #456`

4. **Checks**
   - All CI checks must pass (green ✓)
   - At least one approval from a maintainer
   - No unresolved review comments

### Review Process

1. **Initial Review** - Maintainers review within a few days
2. **Feedback** - Address feedback by pushing new commits
3. **Re-review** - Request re-review after addressing feedback
4. **Approval** - Maintainer approves the PR
5. **Merge** - PR is merged using "Squash and merge"

!!! tip "Addressing Feedback"
    When making changes based on review feedback:

    ```bash
    # Make fixes
    git add fixed-file.js

    # Use --fixup for small fixes
    git commit --fixup HEAD

    # Or regular commit
    git commit -m "fix: address review feedback"

    # Push changes
    git push origin feat/your-feature
    ```

### After Merging

1. **Delete Branch** - Delete your feature branch on GitHub
2. **Sync Fork** - Update your local repository
3. **Celebrate** - You've contributed! 🎉

```bash
# Sync your fork
git checkout main
git pull upstream main
git push origin main

# Delete local branch
git branch -d feat/your-feature
```

## Code Style

### General Rules

- **Indentation**: 2 spaces (enforced by EditorConfig)
- **Line Endings**: LF (Unix-style)
- **Charset**: UTF-8
- **Trailing Whitespace**: Remove (automated by pre-commit)
- **Final Newline**: Required (automated by pre-commit)

### Language-Specific Guidelines

=== "Python"
    - Follow [PEP 8](https://pep8.org/)
    - Use type hints where appropriate
    - Maximum line length: 88 characters (Black formatter)
    - Use docstrings for functions and classes

=== "JavaScript/TypeScript"
    - Use Standard/Prettier formatting
    - Prefer `const` and `let` over `var`
    - Use arrow functions for callbacks
    - Add JSDoc comments for public APIs

=== "YAML"
    - 2-space indentation
    - No tabs
    - Use `---` document separator
    - Quote strings when necessary

=== "Markdown"
    - Use reference-style links for readability
    - One sentence per line
    - Use fenced code blocks with language specifiers
    - Follow markdownlint rules

## Code of Conduct

### Our Standards

We are committed to providing a welcoming and inclusive environment:

- ✅ Be respectful and professional
- ✅ Welcome newcomers and help them learn
- ✅ Accept constructive criticism gracefully
- ✅ Focus on what's best for the community
- ✅ Show empathy towards other contributors

### Unacceptable Behavior

- ❌ Harassment, discrimination, or abuse
- ❌ Trolling, insulting comments, or personal attacks
- ❌ Publishing others' private information
- ❌ Other conduct that could reasonably be considered inappropriate

### Enforcement

Violations may result in:

1. Warning from maintainers
2. Temporary or permanent ban from the project
3. Reporting to GitHub

## Questions or Need Help?

- **Questions** - Open a [Discussion](../../discussions)
- **Bug Reports** - Open an [Issue](../../issues/new?template=bug_report.md)
- **Feature Requests** - Open an [Issue](../../issues/new?template=feature_request.md)
- **Security Issues** - See our [Security Policy](security.md)

## License

By contributing, you agree that your contributions will be licensed under the same license as the project. See [LICENSE](../../LICENSE) for details.

---

**Thank you for contributing!** Your efforts help make this project better for everyone. 🙏
