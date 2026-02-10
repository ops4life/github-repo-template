# 🛠️ GitHub Repo Template

[![Documentation](https://img.shields.io/badge/docs-GitHub%20Pages-blue?style=flat-square)](https://username.github.io/github-repo-template/)

Welcome to the Template Repository on GitHub! This repository is designed to serve as a starting point for creating new Git repositories with best practices and configurations already set up.

📚 **[View Full Documentation](https://username.github.io/github-repo-template/)** - Comprehensive guides, tutorials, and API reference

Below is a brief overview of the structure and the purpose of each file and directory in this repository.

## 📁 Repository Structure

```text
.
├── .editorconfig                       # 🖊️ Configuration for consistent coding styles
├── .gitattributes                      # 📋 Git attributes configuration
├── .github                             # 🛠️ GitHub-specific configurations
│   ├── CONTRIBUTING.md                 # 🤝 Contribution guidelines
│   ├── ISSUE_TEMPLATE                  # 📝 GitHub issue templates
│   │   ├── bug_report.md               # 🐛 Bug report template
│   │   ├── config.yml                  # ⚙️ Issue template configuration
│   │   ├── documentation.md            # 📚 Documentation issue template
│   │   ├── feature_request.md          # ✨ Feature request template
│   │   └── issue_template.md           # 📝 General issue template
│   ├── SECURITY.md                     # 🔒 Security policy and vulnerability reporting
│   ├── dependabot.yml                  # 🤖 Dependabot configuration
│   ├── pull_request_template.md        # 📝 Pull request template
│   └── workflows                       # ⚙️ GitHub Actions workflows
│       ├── automerge.yml               # 🔀 Auto-merge workflow for dependabot PRs
│       ├── cleanup-caches.yaml         # 🧹 Cleanup old workflow caches
│       ├── codeql.yaml                 # 🔍 CodeQL security analysis workflow
│       ├── deps-review.yaml            # 📋 Dependency review workflow
│       ├── gitleaks.yaml               # 🔒 Secret scanning workflow
│       ├── lint-pr.yaml                # 🧹 Linting workflow for pull requests
│       ├── pre-commit-auto-update.yaml # 🔄 Pre-commit hook auto-update workflow
│       ├── pre-commit-ci.yaml          # ✅ Pre-commit CI workflow
│       ├── release.yaml                # 🚀 Release workflow
│       ├── stale.yaml                  # ⏳ Stale issue management workflow
│       ├── template-repo-sync.yaml     # 🔄 Template repository sync workflow
│       └── update-license.yml          # 📄 License year update workflow
├── .gitignore                          # 🚫 Files and directories to be ignored by Git
├── .gitleaks.toml                      # 🔒 Gitleaks secret scanning configuration
├── .pre-commit-config.yaml             # 🛠️ Pre-commit hooks configuration
├── .releaserc.json                     # 🚀 Semantic release configuration
├── .templatesyncignore                 # 🔄 Template sync ignore patterns
├── .vscode                             # 🖥️ VSCode-specific configurations
│   ├── extensions.json                 # 🛠️ Recommended extensions for VSCode
│   └── settings.json                   # ⚙️ VSCode settings
├── .yamllint                           # 📝 YAML linting configuration
├── CHANGELOG.md                        # 📝 Change log of the project
├── CODEOWNERS                          # 👥 Defines the code owners for the repository
├── LICENSE                             # ⚖️ License for the project
└── README.md                           # 📖 Project documentation (this file)
```

## ⚙️ Semantic Commit Messages

This project uses [Semantic Commit Messages](https://www.conventionalcommits.org/) to ensure meaningful and consistent commit history. The format is as follows:

```php
<type>(<scope>): <subject>
```

### Types

- `feat`: A new feature (e.g., `feat: add login functionality`).
- `fix`: A bug fix (e.g., `fix: resolve login button issue`).
- `docs`: Documentation changes (e.g., `docs: update API documentation`).
- `style`: Code style changes (formatting, missing semi-colons, etc.) without changing logic (e.g., `style: fix indentation`).
- `refactor`: Code changes that neither fix a bug nor add a feature (e.g., `refactor: update user controller structure`).
- `test`: Adding or updating tests (e.g., `test: add unit tests for login service`).
- `chore`: Changes to build process, auxiliary tools, or libraries (e.g., `chore: update dependencies`).

### Scope

Optional: The part of the codebase affected by the change (e.g., `feat(auth): add OAuth support`)

### Subject

A brief description of the change, using the imperative mood (e.g., `fix: resolve issue with user authentication`).

## 🔒 Secret Scanning with Gitleaks

This project uses [Gitleaks](https://github.com/gitleaks/gitleaks) to detect secrets and sensitive information in the codebase. Gitleaks is configured to run both locally via pre-commit hooks and in CI/CD pipelines.

### Local Development

Gitleaks runs automatically as a pre-commit hook. To install the pre-commit hooks:

```bash
pre-commit install
```

To run Gitleaks manually:

```bash
pre-commit run gitleaks --all-files
```

### CI/CD Integration

Gitleaks runs automatically on:

- Pull requests to main/master branch
- Pushes to main/master branch

The workflow will fail if any secrets are detected, helping prevent accidental exposure of sensitive information.

### Configuration

The `.gitleaks.toml` file contains:

- Allowlist patterns for false positives
- Custom scanning rules
- Output configuration

## 🔍 CodeQL Security Analysis

This project uses [GitHub CodeQL](https://codeql.github.com/) to perform advanced security analysis and detect vulnerabilities in the codebase. CodeQL is configured to analyze JavaScript and Python code by default.

### When It Runs

CodeQL analysis runs automatically on:

- Pull requests to main/master branch
- Pushes to main/master branch
- Weekly schedule (every Monday at 00:00 UTC)

### Language Detection

The workflow is configured with `continue-on-error: true`, which means:

- If a specified language (JavaScript or Python) is not detected in the repository, the workflow will not fail
- This is useful for template repositories where different projects may use different languages
- Analysis will still run for any languages that are present

### Customization

To customize the languages analyzed, edit `.github/workflows/codeql.yaml`:

```yaml
matrix:
  language: [ 'javascript', 'python' ]
  # Supported: 'cpp', 'csharp', 'go', 'java', 'javascript', 'python', 'ruby', 'swift'
```

### Security Alerts

Security vulnerabilities detected by CodeQL are reported in the Security tab of your repository under "Code scanning alerts".

## 🚀 Semantic Release

### How It Works

1. Analyze commits: Semantic Release inspects commit messages to determine the type of changes in the codebase.
2. Generate release version: Based on the commit type, it will automatically bump the version following semantic versioning:
   - fix → Patch release (e.g., 1.0.1)
   - feat → Minor release (e.g., 1.1.0)
   - BREAKING CHANGE → Major release (e.g., 2.0.0)
3. Create release notes: It generates a changelog from the commit messages and includes it in the release.
4. Publish: It automatically publishes the new version to the repository (and any other configured registries, e.g., npm).

## 🤝 Contributing

If you find any issues or have suggestions for improving this template repository, please feel free to open an issue or submit a pull request. Contributions are always welcome!

## 📜 License

This repository is licensed under the MIT License. See the [LICENSE](LICENSE) file for more information.
