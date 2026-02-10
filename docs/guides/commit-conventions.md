# Commit Conventions

This project uses [Conventional Commits](https://www.conventionalcommits.org/) to enable automated semantic versioning and changelog generation. Understanding these conventions is crucial for contributing to this project.

## Why Conventional Commits?

Conventional Commits provide:

- **Automated versioning** - Determine the next version number automatically
- **Automated changelogs** - Generate CHANGELOG.md from commit messages
- **Better history** - Human and machine-readable commit history
- **Triggered actions** - Enable automated releases and deployments

## Commit Message Format

```
<type>(<scope>): <subject>

[optional body]

[optional footer(s)]
```

### Components

#### Type (Required)

The type determines the version bump and changelog category:

| Type | Description | Changelog Section | Version Bump |
|------|-------------|-------------------|--------------|
| `feat` | New feature | Features | Minor (0.1.0) |
| `fix` | Bug fix | Bug Fixes | Patch (0.0.1) |
| `docs` | Documentation only | Documentation | None |
| `style` | Code style (formatting) | None | None |
| `refactor` | Code refactoring | None | None |
| `perf` | Performance improvements | Performance | Patch (0.0.1) |
| `test` | Adding/updating tests | None | None |
| `build` | Build system changes | None | None |
| `ci` | CI/CD configuration | None | None |
| `chore` | Maintenance tasks | None | None |
| `revert` | Revert previous commit | None | Patch (0.0.1) |

#### Scope (Optional)

The scope specifies which part of the codebase is affected:

```bash
feat(auth): add OAuth2 authentication
fix(api): resolve null pointer exception
docs(readme): update installation instructions
```

Common scopes:

- `auth` - Authentication/authorization
- `api` - API endpoints
- `ui` - User interface
- `db` - Database
- `deps` - Dependencies
- `config` - Configuration

#### Subject (Required)

The subject is a brief description of the change:

- Use imperative mood: "add" not "added" or "adds"
- Don't capitalize the first letter
- No period at the end
- Maximum 72 characters

✅ **Good:**
```bash
feat: add user authentication
fix: resolve login button issue
docs: update API documentation
```

❌ **Bad:**
```bash
feat: Added user authentication.  # Capitalized, past tense, period
fix: fixes the login thing        # Not imperative, vague
docs: Updated API docs.            # Past tense, period
```

#### Body (Optional)

The body provides additional context:

- Explain **what** and **why**, not **how**
- Wrap at 72 characters
- Separate from subject with a blank line

```bash
feat(auth): add OAuth2 authentication

Implements OAuth2 authentication flow with support for Google and GitHub providers.
Users can now sign in using their existing accounts instead of creating new credentials.
This improves the user experience and reduces password fatigue.
```

#### Footer (Optional)

The footer contains:

- **Breaking changes**
- **Issue references**
- **Other metadata**

```bash
feat(api): change response format

BREAKING CHANGE: API responses now return data in camelCase instead of snake_case.
Clients must update their parsing logic to handle the new format.

Closes #123
Refs #456
```

## Version Bumping

Conventional Commits automatically determine the next version:

### Patch Release (0.0.1)

Bug fixes and minor changes:

```bash
fix(api): resolve null pointer exception
perf(db): optimize query performance
revert: undo previous optimization
```

**Version**: 1.0.0 → 1.0.1

### Minor Release (0.1.0)

New features (backwards-compatible):

```bash
feat(auth): add OAuth2 authentication
feat(api): add user profile endpoint
```

**Version**: 1.0.0 → 1.1.0

### Major Release (1.0.0)

Breaking changes:

```bash
feat(api)!: change response format

BREAKING CHANGE: API responses now use camelCase.
```

**Version**: 1.0.0 → 2.0.0

!!! warning "Breaking Changes"
    Breaking changes trigger a major version bump. They can be indicated by:

    - Adding `!` after the type: `feat!:` or `feat(api)!:`
    - Including `BREAKING CHANGE:` in the footer

## Real-World Examples

### Adding a Feature

```bash
feat(auth): add password reset functionality

Implements password reset flow with email verification.
Users receive a time-limited reset link via email.
Includes rate limiting to prevent abuse.

Closes #142
```

### Fixing a Bug

```bash
fix(ui): resolve button alignment on mobile

The submit button was not visible on screens smaller than 768px.
Changed flexbox layout to ensure proper alignment across all screen sizes.

Fixes #98
```

### Documentation Update

```bash
docs: add API authentication guide

Created comprehensive guide for API authentication covering:
- API key generation
- Token-based authentication
- OAuth2 flow
- Security best practices
```

### Breaking Change

```bash
feat(api)!: redesign user endpoint structure

BREAKING CHANGE: User endpoints have been reorganized for better RESTful design.

Before:
- GET /user/profile
- POST /user/update

After:
- GET /api/users/:id
- PUT /api/users/:id

Migration guide available in docs/migrations/v2.0.0.md

Closes #201
```

### Multiple Scopes

```bash
feat(ui,api): implement real-time notifications

- Add WebSocket endpoint for notifications
- Create notification component in React
- Implement notification storage in Redis
- Add notification preferences UI

Closes #156, #157
```

### Revert Commit

```bash
revert: "feat(auth): add OAuth2 authentication"

This reverts commit abc123def456.

Reverting due to security vulnerability discovered in the OAuth2 library.
Will re-implement after updating to patched version.
```

## Practical Guidelines

### Writing Good Subjects

✅ **Do:**
```bash
add user authentication
fix null pointer exception
update installation instructions
remove deprecated API endpoint
```

❌ **Don't:**
```bash
added stuff                    # Vague, past tense
Fixed bug                      # Capitalized, vague
Update the readme file.        # Capitalized, period, redundant words
Quick fix                      # Not descriptive
```

### When to Include Body

Include a body when:

- The change is not obvious from the subject
- You need to explain **why** the change was made
- There are important implementation details
- You want to provide migration instructions

Skip the body when:

- The subject is self-explanatory
- The change is trivial
- The code changes speak for themselves

### Issue References

Link commits to issues:

```bash
Closes #123           # Closes the issue
Fixes #456            # Same as "Closes"
Resolves #789         # Same as "Closes"
Related to #321       # References without closing
Refs #654             # Short form
Part of #987          # Indicates partial implementation
```

Multiple issues:

```bash
Closes #123, #456
Fixes #789 and resolves #321
```

## Enforcement

This project enforces conventional commits through:

### Pre-commit Hooks

Local validation before commit:

```bash
# Runs automatically on git commit
✓ Check commit message format
✓ Validate type and scope
✓ Check subject format
```

### CI/CD Pipeline

PR title validation:

```yaml
# .github/workflows/lint-pr.yaml
- Validates PR title follows Conventional Commits
- Ensures proper formatting
- Blocks merging if invalid
```

### How to Fix Failed Validation

If your commit message fails validation:

```bash
# Amend the last commit message
git commit --amend

# Or for older commits, use interactive rebase
git rebase -i HEAD~3
```

## Tools and Resources

### Commitizen

Interactive tool for creating conventional commits:

```bash
# Install
npm install -g commitizen cz-conventional-changelog

# Use
git cz
```

### Git Hooks

Pre-commit hook to validate messages:

```bash
# Already configured in this template
# Located in .pre-commit-config.yaml
```

### IDE Extensions

- **VS Code**: [Conventional Commits](https://marketplace.visualstudio.com/items?itemName=vivaxy.vscode-conventional-commits)
- **IntelliJ**: [Conventional Commit](https://plugins.jetbrains.com/plugin/13389-conventional-commit)

## Common Questions

### Should I use `chore` or `docs` for README updates?

Use `docs:` for any documentation changes, including README files.

```bash
docs: update README with new installation steps
```

### How do I commit refactoring with bug fixes?

Use separate commits:

```bash
# First commit: refactoring
git commit -m "refactor(api): extract validation logic"

# Second commit: bug fix
git commit -m "fix(api): resolve validation edge case"
```

### Can I have multiple types in one commit?

No. Each commit should represent a single logical change. If you have multiple types, split into multiple commits:

```bash
# Bad
git commit -m "feat(api): add endpoint and fix bug"

# Good
git commit -m "feat(api): add user endpoint"
git commit -m "fix(api): resolve null pointer exception"
```

### What if I forget the format?

You'll see an error from the pre-commit hook. Amend your commit:

```bash
# See the error
git commit -m "added feature"
# Error: Commit message does not follow Conventional Commits format

# Fix it
git commit --amend -m "feat: add user authentication"
```

## Learn More

- **[Conventional Commits Specification](https://www.conventionalcommits.org/)** - Official specification
- **[Semantic Versioning](https://semver.org/)** - Version numbering scheme
- **[Keep a Changelog](https://keepachangelog.com/)** - Changelog best practices
- **[Contributing Guidelines](contributing.md)** - Full contribution workflow

---

**Remember**: Good commit messages are a gift to your future self and your collaborators. Take the time to write them well! 🎁
