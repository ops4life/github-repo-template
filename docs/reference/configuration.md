# Configuration Reference

This page provides detailed information about all configuration files in the template and how to customize them for your project.

## EditorConfig (.editorconfig)

Maintains consistent coding styles across different editors and IDEs.

### Current Configuration

```ini
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
```

### Customization

#### Language-Specific Settings

```ini
# Python: 4 spaces
[*.py]
indent_size = 4

# Makefiles: tabs required
[Makefile]
indent_style = tab

# Markdown: no trailing whitespace trimming
[*.md]
trim_trailing_whitespace = false
```

#### Project-Specific Settings

```ini
# Set max line length
[*.{js,ts}]
max_line_length = 80

# Keep trailing whitespace in patches
[*.{diff,patch}]
trim_trailing_whitespace = false
```

### Supported Properties

| Property | Values | Description |
|----------|--------|-------------|
| `indent_style` | `space`, `tab` | Indentation style |
| `indent_size` | number | Spaces per indent |
| `end_of_line` | `lf`, `crlf`, `cr` | Line ending style |
| `charset` | `utf-8`, etc. | Character encoding |
| `trim_trailing_whitespace` | `true`, `false` | Remove trailing spaces |
| `insert_final_newline` | `true`, `false` | Add final newline |
| `max_line_length` | number | Maximum line length |

## YAML Linting (.yamllint)

Configures YAML file validation and style enforcement.

### Current Configuration

```yaml
---
extends: default

rules:
  line-length:
    max: 150
    level: warning
  document-start: disable
  comments:
    min-spaces-from-content: 1
```

### Customization

#### Adjust Line Length

```yaml
rules:
  line-length:
    max: 120  # Stricter limit
    level: error  # Make it an error instead of warning
```

#### Enable Document Start

```yaml
rules:
  document-start:
    present: true  # Require --- at start
```

#### Indentation Rules

```yaml
rules:
  indentation:
    spaces: 2
    indent-sequences: true
    check-multi-line-strings: false
```

### Common Rules

| Rule | Description | Options |
|------|-------------|---------|
| `line-length` | Maximum line length | `max`, `level` |
| `indentation` | Indentation settings | `spaces`, `indent-sequences` |
| `trailing-spaces` | Trailing whitespace | `level` |
| `comments` | Comment formatting | `min-spaces-from-content` |
| `document-start` | Require `---` | `present`, `disable` |
| `empty-lines` | Empty line rules | `max` |

### Testing

```bash
# Lint all YAML files
yamllint .

# Lint specific files
yamllint .github/workflows/*.yaml

# Show config
yamllint --print-config .
```

## Gitleaks Configuration (.gitleaks.toml)

Configures secret scanning and leak detection.

### Current Configuration

```toml
# Gitleaks configuration
[extend]
useDefault = true

[allowlist]
description = "Allowlist for false positives"
```

### Customization

#### Add Path Allowlists

Exclude specific paths from scanning:

```toml
[allowlist]
description = "Allowlist for test files"

paths = [
  '''.*test.*''',
  '''.*mock.*''',
  '''.*example.*''',
  '''.*fixture.*'''
]
```

#### Add Regex Allowlists

Exclude specific patterns:

```toml
[allowlist]
regexes = [
  '''test_key_[a-zA-Z0-9]+''',
  '''example_token_.*''',
  '''integrity:.*sha512-.*'''  # npm lock files
]
```

#### Add Commit Allowlists

Exclude specific commits:

```toml
[allowlist]
commits = [
  "abc123def456",  # Known false positive
  "789ghi012jkl"
]
```

#### Custom Rules

Add project-specific secret patterns:

```toml
[[rules]]
id = "custom-api-key"
description = "Custom API key pattern"
regex = '''custom_api_[a-zA-Z0-9]{32}'''
keywords = ["custom_api"]
```

### Testing

```bash
# Scan repository
gitleaks detect --source . --verbose

# Scan specific files
gitleaks detect --source . --log-opts="path/to/file.js"

# Protect mode (pre-commit)
gitleaks protect --staged --verbose
```

## Semantic Release (.releaserc.json)

Configures automated versioning and releases.

### Current Configuration

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

### Customization

#### Multiple Branches

Support different release channels:

```json
{
  "branches": [
    "main",
    {"name": "beta", "prerelease": true},
    {"name": "alpha", "prerelease": true}
  ]
}
```

#### Custom Commit Types

Override commit type rules:

```json
{
  "plugins": [
    ["@semantic-release/commit-analyzer", {
      "preset": "angular",
      "releaseRules": [
        {"type": "docs", "release": "patch"},
        {"type": "refactor", "release": "patch"},
        {"type": "style", "release": "patch"}
      ]
    }]
  ]
}
```

#### Changelog Customization

```json
{
  "plugins": [
    ["@semantic-release/changelog", {
      "changelogFile": "CHANGELOG.md",
      "changelogTitle": "# Changelog\n\nAll notable changes to this project will be documented in this file."
    }]
  ]
}
```

#### Npm Publishing

Add npm package publishing:

```json
{
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    "@semantic-release/npm",  // Add npm plugin
    "@semantic-release/github",
    "@semantic-release/git"
  ]
}
```

## Pre-commit Hooks (.pre-commit-config.yaml)

Configures automated pre-commit checks.

### Current Configuration

```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v5.0.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-added-large-files

  # Additional hooks...
```

### Customization

#### Add Language-Specific Hooks

Python:

```yaml
- repo: https://github.com/psf/black
  rev: 24.1.1
  hooks:
    - id: black
      language_version: python3.11

- repo: https://github.com/PyCQA/flake8
  rev: 7.0.0
  hooks:
    - id: flake8
```

JavaScript/TypeScript:

```yaml
- repo: https://github.com/pre-commit/mirrors-eslint
  rev: v8.56.0
  hooks:
    - id: eslint
      files: \.[jt]sx?$
      types: [file]

- repo: https://github.com/pre-commit/mirrors-prettier
  rev: v3.1.0
  hooks:
    - id: prettier
```

Go:

```yaml
- repo: https://github.com/dnephin/pre-commit-golang
  rev: v0.5.1
  hooks:
    - id: go-fmt
    - id: go-vet
    - id: go-lint
```

#### Customize Existing Hooks

```yaml
- repo: https://github.com/pre-commit/pre-commit-hooks
  rev: v5.0.0
  hooks:
    - id: trailing-whitespace
      args: ['--markdown-linebreak-ext=md']
    - id: check-added-large-files
      args: ['--maxkb=500']  # Limit to 500KB
    - id: check-yaml
      args: ['--allow-multiple-documents']
```

#### Exclude Files

```yaml
- repo: https://github.com/adrienverge/yamllint
  rev: v1.35.1
  hooks:
    - id: yamllint
      exclude: ^(docker-compose\.yml|\.github/dependabot\.yml)$
```

### Common Hooks

| Hook | Purpose | Configuration |
|------|---------|---------------|
| `trailing-whitespace` | Remove trailing spaces | `args: ['--markdown-linebreak-ext=md']` |
| `end-of-file-fixer` | Add final newline | N/A |
| `check-yaml` | Validate YAML | `args: ['--allow-multiple-documents']` |
| `check-json` | Validate JSON | N/A |
| `check-added-large-files` | Prevent large commits | `args: ['--maxkb=N']` |
| `mixed-line-ending` | Enforce line endings | `args: ['--fix=lf']` |

### Testing

```bash
# Run all hooks
pre-commit run --all-files

# Run specific hook
pre-commit run yamllint --all-files

# Update hook versions
pre-commit autoupdate

# Skip hooks (use sparingly)
git commit --no-verify
```

## MkDocs (mkdocs.yml)

Configures documentation site generation.

### Current Configuration

```yaml
site_name: GitHub Repo Template
theme:
  name: material
  palette:
    - scheme: default
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode
    - scheme: slate
      toggle:
        icon: material/brightness-4
        name: Switch to light mode
```

### Customization

#### Site Information

```yaml
site_name: My Project
site_description: A comprehensive project description
site_author: Your Name
site_url: https://username.github.io/project/
copyright: Copyright &copy; 2024 Your Name
```

#### Theme Colors

```yaml
theme:
  palette:
    - scheme: default
      primary: blue      # Header color
      accent: indigo     # Interactive elements
    - scheme: slate
      primary: teal
      accent: lime
```

Available colors: `red`, `pink`, `purple`, `deep purple`, `indigo`, `blue`, `light blue`, `cyan`, `teal`, `green`, `light green`, `lime`, `yellow`, `amber`, `orange`, `deep orange`

#### Theme Features

```yaml
theme:
  features:
    - navigation.instant     # Instant loading
    - navigation.tracking    # URL updates with scroll
    - navigation.tabs        # Top-level tabs
    - navigation.sections    # Sections in sidebar
    - navigation.expand      # Expand sections
    - navigation.top         # Back to top button
    - search.suggest         # Search suggestions
    - search.highlight       # Highlight search terms
    - content.code.copy      # Copy code button
    - content.action.edit    # Edit page button
```

#### Plugins

```yaml
plugins:
  - search:
      lang: en
      separator: '[\s\-\.]+'
  - git-revision-date-localized:
      enable_creation_date: true
      type: timeago
  - minify:
      minify_html: true
```

#### Markdown Extensions

```yaml
markdown_extensions:
  - admonition              # Call-out boxes
  - pymdownx.details        # Collapsible details
  - pymdownx.superfences:   # Code blocks with syntax highlighting
      custom_fences:
        - name: mermaid
          class: mermaid
          format: !!python/name:pymdownx.superfences.fence_code_format
  - pymdownx.tabbed:        # Content tabs
      alternate_style: true
  - tables                  # Markdown tables
  - toc:                    # Table of contents
      permalink: true
```

#### Navigation Structure

```yaml
nav:
  - Home: index.md
  - Getting Started:
      - Overview: getting-started/index.md
      - Quick Start: getting-started/quick-start.md
  - Guides:
      - Contributing: guides/contributing.md
      - Commits: guides/commit-conventions.md
  - Reference:
      - Structure: reference/repository-structure.md
```

#### Custom CSS

```yaml
extra_css:
  - assets/stylesheets/extra.css
  - assets/stylesheets/custom.css
```

#### Social Links

```yaml
extra:
  social:
    - icon: fontawesome/brands/github
      link: https://github.com/username
    - icon: fontawesome/brands/twitter
      link: https://twitter.com/username
    - icon: fontawesome/brands/linkedin
      link: https://linkedin.com/in/username
```

### Testing

```bash
# Install dependencies
pip install -r requirements.txt

# Serve locally
mkdocs serve

# Build site
mkdocs build

# Build with strict mode (fail on warnings)
mkdocs build --strict
```

## Dependabot (.github/dependabot.yml)

Configures automated dependency updates.

### Current Configuration

```yaml
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
```

### Customization

#### Multiple Ecosystems

```yaml
version: 2
updates:
  # GitHub Actions
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
    commit-message:
      prefix: "chore(deps)"

  # npm
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
    open-pull-requests-limit: 10

  # pip
  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "weekly"

  # Docker
  - package-ecosystem: "docker"
    directory: "/"
    schedule:
      interval: "monthly"
```

#### Schedule Options

```yaml
schedule:
  interval: "daily"      # Every day
  interval: "weekly"     # Every Monday
  interval: "monthly"    # First of month
  time: "04:00"         # At 4 AM UTC
  day: "monday"         # Specific day
```

#### Version Constraints

```yaml
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
    ignore:
      - dependency-name: "lodash"
        versions: ["4.x"]  # Ignore 4.x updates
```

#### Pull Request Limits

```yaml
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
    open-pull-requests-limit: 5  # Max 5 open PRs
```

## VS Code Settings (.vscode/)

### extensions.json

```json
{
  "recommendations": [
    "editorconfig.editorconfig",
    "redhat.vscode-yaml",
    "davidanson.vscode-markdownlint",
    "eamodio.gitleaks"
  ]
}
```

Add language-specific extensions:

```json
{
  "recommendations": [
    // Python
    "ms-python.python",
    "ms-python.vscode-pylance",

    // JavaScript/TypeScript
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",

    // Go
    "golang.go"
  ]
}
```

### settings.json

```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll": true
  },
  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true
}
```

## Template Sync (.templatesyncignore)

Specifies files to exclude from template synchronization.

### Current Configuration

```text
# Exclude docs
docs/**
mkdocs.yml
requirements.txt

# Exclude customized files
README.md
LICENSE
CODEOWNERS
```

### Patterns

```text
# Exclude specific files
README.md
LICENSE

# Exclude directories
docs/**
src/**

# Exclude by pattern
*.local
.env*

# Keep workflow but exclude specific ones
!.github/workflows/
.github/workflows/custom-*.yaml
```

## Additional Resources

- **[EditorConfig Documentation](https://editorconfig.org/)** - Editor configuration
- **[YAML Lint Rules](https://yamllint.readthedocs.io/)** - YAML linting
- **[Gitleaks Documentation](https://github.com/gitleaks/gitleaks)** - Secret scanning
- **[Semantic Release](https://semantic-release.gitbook.io/)** - Release automation
- **[Pre-commit](https://pre-commit.com/)** - Git hook framework
- **[MkDocs Material](https://squidfunk.github.io/mkdocs-material/)** - Documentation theme

---

**Questions?** Open a [discussion](https://github.com/username/github-repo-template/discussions).
