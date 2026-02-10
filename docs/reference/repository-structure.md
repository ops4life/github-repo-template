# Repository Structure

This page provides a detailed explanation of the repository structure and the purpose of each file and directory.

## Directory Tree

```text
.
├── .editorconfig                       # Editor configuration
├── .gitattributes                      # Git attributes
├── .github/                            # GitHub-specific files
│   ├── CONTRIBUTING.md                 # Contribution guidelines
│   ├── ISSUE_TEMPLATE/                 # Issue templates
│   │   ├── bug_report.md               # Bug report template
│   │   ├── config.yml                  # Template configuration
│   │   ├── documentation.md            # Documentation issue template
│   │   ├── feature_request.md          # Feature request template
│   │   └── issue_template.md           # General issue template
│   ├── SECURITY.md                     # Security policy
│   ├── dependabot.yml                  # Dependabot configuration
│   ├── pull_request_template.md        # PR template
│   └── workflows/                      # GitHub Actions workflows
│       ├── automerge.yml               # Auto-merge workflow
│       ├── cleanup-caches.yaml         # Cache cleanup
│       ├── codeql.yaml                 # Security analysis
│       ├── deps-review.yaml            # Dependency review
│       ├── gitleaks.yaml               # Secret scanning
│       ├── lint-pr.yaml                # PR linting
│       ├── pre-commit-auto-update.yaml # Hook updates
│       ├── pre-commit-ci.yaml          # Pre-commit CI
│       ├── release.yaml                # Semantic release
│       ├── stale.yaml                  # Stale issue management
│       ├── template-repo-sync.yaml     # Template sync
│       └── update-license.yml          # License updates
├── .gitignore                          # Git ignore patterns
├── .gitleaks.toml                      # Gitleaks configuration
├── .pre-commit-config.yaml             # Pre-commit hooks
├── .releaserc.json                     # Semantic release config
├── .templatesyncignore                 # Template sync ignore
├── .vscode/                            # VS Code configuration
│   ├── extensions.json                 # Recommended extensions
│   └── settings.json                   # Editor settings
├── .yamllint                           # YAML linting rules
├── CHANGELOG.md                        # Auto-generated changelog
├── CODEOWNERS                          # Code ownership
├── LICENSE                             # Project license
├── README.md                           # Project documentation
├── docs/                               # MkDocs documentation
│   ├── index.md                        # Documentation homepage
│   ├── getting-started/                # Getting started guides
│   ├── guides/                         # User guides
│   ├── reference/                      # Reference documentation
│   └── assets/                         # Documentation assets
├── mkdocs.yml                          # MkDocs configuration
└── requirements.txt                    # Python dependencies
```

## Root Files

### .editorconfig

Ensures consistent coding styles across different editors and IDEs.

```ini
# EditorConfig settings
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
```

**What it does:**

- Sets indentation to 2 spaces
- Uses LF line endings (Unix-style)
- UTF-8 character encoding
- Removes trailing whitespace
- Ensures final newline in files

**Supported editors:** VS Code, IntelliJ, Sublime Text, Atom, Vim, and more

### .gitattributes

Configures Git attributes for files.

```text
# Set default behavior
* text=auto

# Ensure line endings
*.sh text eol=lf
*.yaml text eol=lf
*.yml text eol=lf

# Denote binary files
*.png binary
*.jpg binary
```

**What it does:**

- Normalizes line endings
- Marks binary files
- Sets merge strategies

### .gitignore

Specifies files and directories Git should ignore.

**Included patterns:**

- Build artifacts
- Dependency directories
- IDE-specific files
- Operating system files
- Temporary files
- MkDocs build output (`site/`)

### .releaserc.json

Configures semantic-release for automated versioning.

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

**Plugins:**

- **commit-analyzer** - Analyzes commits for version bumps
- **release-notes-generator** - Generates release notes
- **changelog** - Updates CHANGELOG.md
- **github** - Creates GitHub releases
- **git** - Commits version updates

### .templatesyncignore

Specifies files to exclude from template synchronization.

**Example patterns:**

```text
# Exclude customized files
README.md
LICENSE
CODEOWNERS

# Exclude documentation
docs/**
mkdocs.yml
requirements.txt

# Exclude project-specific configs
.github/workflows/custom-*.yaml
```

### CHANGELOG.md

Automatically generated changelog from commit messages.

**Format:**

- Organized by version
- Grouped by type (Features, Bug Fixes)
- Links to commits and PRs
- Generated by semantic-release

**Do not edit manually** - Changes will be overwritten.

### CODEOWNERS

Defines code ownership for automatic review requests.

```text
# Default owners for everything
* @username

# Specific ownership
/.github/ @username @team-devops
/docs/ @username @team-docs
```

**Benefits:**

- Automatic reviewer assignment
- Clear ownership
- Better code review coverage

### LICENSE

MIT License by default. Replace with your preferred license.

**Common alternatives:**

- MIT - Permissive, simple
- Apache 2.0 - Permissive with patent grant
- GPL v3 - Copyleft, requires sharing
- BSD 3-Clause - Permissive, simple

## .github Directory

### Issue Templates

#### bug_report.md

Template for bug reports with structured sections:

- Bug description
- Steps to reproduce
- Expected vs actual behavior
- Environment details
- Screenshots

#### feature_request.md

Template for feature requests:

- Problem description
- Proposed solution
- Alternatives considered
- Additional context

#### documentation.md

Template for documentation issues:

- What needs documentation
- Current state
- Suggested improvements

#### config.yml

Configures issue template chooser:

- Custom templates
- External links
- Blank issues option

### Pull Request Template

Structured PR template with:

- Description
- Type of change
- Testing checklist
- Review checklist

### Workflows

See [GitHub Workflows Guide](../guides/workflows.md) for detailed documentation on all 12 workflows.

## Configuration Files

### .gitleaks.toml

Configures Gitleaks secret scanning.

**Sections:**

- **[extend]** - Extend default rules
- **[allowlist]** - Exclude false positives
- **[[rules]]** - Custom detection rules

**Example allowlist:**

```toml
[allowlist]
description = "Allow test secrets"
paths = [
  '''.*test.*''',
  '''.*mock.*'''
]
regexes = [
  '''test_key_.*'''
]
```

### .pre-commit-config.yaml

Configures pre-commit hooks.

**Included hooks:**

1. **trailing-whitespace** - Remove trailing whitespace
2. **end-of-file-fixer** - Ensure final newline
3. **check-yaml** - Validate YAML syntax
4. **check-added-large-files** - Prevent large commits
5. **editorconfig-checker** - Validate EditorConfig
6. **yamllint** - Lint YAML files
7. **markdownlint** - Lint Markdown files
8. **gitleaks** - Scan for secrets

**Update hooks:**

```bash
pre-commit autoupdate
```

### .yamllint

YAML linting configuration.

**Rules:**

- Line length: 150 characters
- Indentation: 2 spaces
- No trailing spaces
- Document start optional
- Comments formatting

### mkdocs.yml

MkDocs documentation configuration.

**Key sections:**

- **site_name** - Documentation title
- **theme** - Material theme configuration
- **plugins** - Search, git-revision-date
- **markdown_extensions** - Admonitions, code highlighting
- **nav** - Navigation structure

See [Configuration Reference](configuration.md) for details.

## VS Code Configuration

### .vscode/extensions.json

Recommended VS Code extensions:

- **EditorConfig** - EditorConfig support
- **YAML** - YAML language support
- **Markdown** - Markdown language features
- **GitLens** - Git supercharged

### .vscode/settings.json

VS Code workspace settings:

- File associations
- Editor formatting
- Extension configuration

## Documentation (docs/)

### Structure

```text
docs/
├── index.md                 # Homepage
├── getting-started/         # Getting started guides
│   ├── index.md             # Template usage
│   └── quick-start.md       # 5-minute setup
├── guides/                  # User guides
│   ├── contributing.md      # Contributing guidelines
│   ├── commit-conventions.md # Commit format
│   ├── security.md          # Security policy
│   └── workflows.md         # GitHub Actions
├── reference/               # Reference docs
│   ├── repository-structure.md # This file
│   ├── configuration.md     # Configuration guide
│   └── changelog.md         # Changelog
└── assets/                  # Static assets
    └── stylesheets/
        └── extra.css        # Custom CSS
```

### Content Guidelines

- **Getting Started** - Tutorials and quick starts
- **Guides** - Task-oriented documentation
- **Reference** - Technical reference material

## File Naming Conventions

### General Rules

- Use lowercase for files and directories
- Use hyphens for multi-word names: `feature-request.md`
- Use descriptive names: `cleanup-caches.yaml` not `cleanup.yaml`

### Workflow Files

- Use `.yaml` extension for workflows
- Use descriptive names: `pre-commit-ci.yaml`
- Group related workflows: `codeql.yaml`, `gitleaks.yaml`

### Documentation Files

- Use `.md` extension for Markdown
- Use lowercase with hyphens: `commit-conventions.md`
- Match navigation structure

## Customization Guide

### For Language-Specific Projects

#### Python Project

Add to root:

```text
├── pyproject.toml          # Python project config
├── setup.py                # Package setup
├── requirements.txt        # Dependencies
├── requirements-dev.txt    # Dev dependencies
└── src/                    # Source code
```

#### Node.js Project

Add to root:

```text
├── package.json            # Package configuration
├── package-lock.json       # Dependency lock
├── .npmrc                  # npm configuration
└── src/                    # Source code
```

#### Go Project

Add to root:

```text
├── go.mod                  # Module definition
├── go.sum                  # Dependency checksums
├── main.go                 # Entry point
└── cmd/                    # Command packages
```

### Project-Specific Additions

Customize for your needs:

```text
├── .env.example            # Environment variables template
├── docker-compose.yml      # Docker composition
├── Dockerfile              # Container definition
├── Makefile                # Build automation
├── tests/                  # Test directory
└── scripts/                # Utility scripts
```

## Maintenance

### What to Update

When creating a repository from this template:

1. **README.md** - Update project description
2. **LICENSE** - Change copyright holder
3. **CODEOWNERS** - Set code owners
4. **mkdocs.yml** - Update site info
5. **.github/workflows/*** - Customize as needed

### What to Keep

Keep these files mostly unchanged:

- `.editorconfig`
- `.gitattributes`
- `.gitignore`
- `.gitleaks.toml` (customize allowlists only)
- `.pre-commit-config.yaml` (update versions only)
- `.yamllint`

## Additional Resources

- **[Configuration Guide](configuration.md)** - Detailed configuration reference
- **[GitHub Workflows](../guides/workflows.md)** - Workflow documentation
- **[Contributing Guidelines](../guides/contributing.md)** - How to contribute

---

**Questions?** Open a [discussion](https://github.com/ops4life/github-repo-template/discussions).
