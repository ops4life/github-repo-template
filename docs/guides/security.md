# Security Policy

Security is a top priority for this project. This document outlines our security measures, how to report vulnerabilities, and best practices for users.

## Reporting a Vulnerability

If you discover a security vulnerability, please report it responsibly. **Do NOT publicly disclose security vulnerabilities.**

### How to Report

#### 1. GitHub Security Advisories (Recommended)

The preferred method for reporting vulnerabilities:

1. Navigate to the **Security** tab of this repository
2. Click **"Report a vulnerability"**
3. Fill out the vulnerability report form
4. Submit confidentially

#### 2. Email

Alternatively, contact the maintainers via email:

- Include a detailed description of the vulnerability
- Provide steps to reproduce if possible
- Suggest fixes if you have them

### What to Include

When reporting a vulnerability, please provide:

```markdown
**Vulnerability Description**
Clear description of the security issue

**Impact Assessment**
- Who is affected?
- What can an attacker do?
- How severe is it?

**Steps to Reproduce**
1. Step one
2. Step two
3. Step three

**Suggested Fixes** (optional)
Ideas for how to address the vulnerability

**Contact Information**
Your email for follow-up questions
```

!!! example "Good Vulnerability Report"
    **Title**: SQL Injection in User Search Endpoint

    **Description**: The `/api/users/search` endpoint is vulnerable to SQL injection attacks through the `query` parameter.

    **Impact**: An attacker can extract sensitive database information or modify data.

    **Steps to Reproduce**:
    1. Send GET request to `/api/users/search?query=' OR '1'='1`
    2. Observe that all users are returned regardless of the query
    3. More malicious payloads can be crafted to extract data

    **Suggested Fix**: Use parameterized queries instead of string concatenation for SQL queries.

## Security Measures

This template implements multiple layers of security:

### Automated Security Scanning

#### Gitleaks - Secret Scanning

Prevents accidental exposure of secrets:

- **Pre-commit**: Scans before each commit
- **CI/CD**: Scans on every push and PR
- **Configuration**: `.gitleaks.toml`

```bash
# Run manually
pre-commit run gitleaks --all-files

# Or directly
gitleaks detect --source . --verbose
```

**Protected secrets:**

- API keys and tokens
- Passwords and connection strings
- Private keys and certificates
- OAuth tokens
- Cloud credentials (AWS, GCP, Azure)

#### CodeQL - Security Analysis

Advanced semantic code analysis:

- **Languages**: JavaScript, Python (configurable)
- **Schedule**: Weekly + on push to main
- **Detects**: SQL injection, XSS, command injection, path traversal, etc.

View alerts: **Security tab → Code scanning alerts**

#### Dependabot - Dependency Updates

Automated security updates for dependencies:

- **Frequency**: Weekly scans
- **Action**: Creates PRs for vulnerable dependencies
- **Auto-merge**: Configured for patch and minor updates

Configuration: `.github/dependabot.yml`

#### Dependency Review

Supply chain security for pull requests:

- **Runs on**: Every PR
- **Checks**: New dependencies for known vulnerabilities
- **Blocks**: PRs introducing high-severity vulnerabilities

### Development Security

#### Pre-commit Hooks

Automated checks before commits:

```yaml
# .pre-commit-config.yaml includes:
- Gitleaks secret scanning
- YAML linting
- Markdown linting
- EditorConfig enforcement
- Trailing whitespace removal
```

#### Branch Protection

Recommended settings for main branch:

```yaml
Branch protection rules:
- Require pull request reviews (1+)
- Require status checks to pass
- Require branches to be up to date
- Require conversation resolution
- Enforce for administrators
```

Configure: **Settings → Branches → Add rule**

#### Conventional Commits

Enforced commit format prevents:

- Accidental breaking changes without documentation
- Unclear commit history making security audits difficult
- Releases without proper changelog entries

## Security Update Process

Our process for handling security vulnerabilities:

### 1. Vulnerability Assessment (Within 48 hours)

- Verify the reported vulnerability
- Assess severity using CVSS score
- Determine affected versions
- Identify potential workarounds

### 2. Fix Development

Priority based on severity:

| Severity | Response Time | Priority |
|----------|---------------|----------|
| Critical | Immediate | P0 |
| High | 1-3 days | P1 |
| Medium | 1-2 weeks | P2 |
| Low | Next release | P3 |

### 3. Testing

All security fixes undergo:

- Unit tests
- Integration tests
- Security-specific test cases
- Manual testing
- Code review

### 4. Release

- Create patch release
- Update CHANGELOG.md
- Publish GitHub release
- Update documentation
- Notify users (if applicable)

### 5. Disclosure

After the fix is released:

- Publish security advisory
- Credit reporter (if desired)
- Document in CHANGELOG
- Update security documentation

## Responsible Disclosure

We ask security researchers to:

### Do

✅ Give us reasonable time to respond (90 days)
✅ Provide detailed vulnerability reports
✅ Keep vulnerabilities confidential until fixed
✅ Act in good faith
✅ Report vulnerabilities as soon as discovered

### Don't

❌ Publicly disclose before fix is released
❌ Access or modify other users' data
❌ Perform denial of service attacks
❌ Spam or social engineer our users
❌ Demand payment for disclosure

## Supported Versions

As a template repository, we recommend always using the latest version:

| Version | Supported |
|---------|-----------|
| Latest commit on `main` | ✅ Yes |
| Older versions | ❌ No |

Security updates are applied to the `main` branch only.

## Security Best Practices for Users

When using this template, follow these security practices:

### Immediate Setup

#### 1. Install Pre-commit Hooks

```bash
# Immediately after cloning
pre-commit install

# Verify
pre-commit run --all-files
```

#### 2. Configure Gitleaks

Review and customize `.gitleaks.toml` for your project:

```toml
[allowlist]
description = "Allowlist for false positives"

# Add project-specific patterns
paths = [
  '''.*test.*''',
  '''.*mock.*''',
  '''.*example.*'''
]

regexes = [
  '''integrity:.*'''  # npm/yarn lock files
]
```

#### 3. Enable Branch Protection

Protect your main branch:

1. **Settings → Branches → Add rule**
2. Branch name pattern: `main`
3. Enable required checks:
   - ✅ Require pull request reviews
   - ✅ Require status checks to pass
   - ✅ Require conversation resolution

#### 4. Configure Dependabot

Enable automatic security updates:

**Settings → Security & analysis**

- ✅ Dependabot alerts
- ✅ Dependabot security updates

### Ongoing Security

#### Update Dependencies Regularly

```bash
# Check for outdated dependencies (Node.js)
npm outdated

# Check for outdated dependencies (Python)
pip list --outdated

# Update pre-commit hooks
pre-commit autoupdate
```

#### Rotate Secrets Immediately

If a secret is accidentally committed:

1. **Revoke** the exposed secret immediately
2. **Generate** a new secret
3. **Update** all systems using the old secret
4. **Verify** the secret is not in git history

```bash
# Remove from git history (use with caution)
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch path/to/secret" \
  --prune-empty --tag-name-filter cat -- --all

# Force push (be careful!)
git push origin --force --all
```

#### Use Signed Commits

Enable GPG signing:

```bash
# Configure git
git config --global commit.gpgsign true
git config --global user.signingkey YOUR_GPG_KEY_ID

# All commits will be signed
git commit -m "feat: add feature"
```

#### Review Dependencies

Before adding dependencies:

- Check npm/PyPI for known vulnerabilities
- Review the package's GitHub repository
- Check the last update date
- Look for security policy
- Review open issues and PRs

#### Secure CI/CD

- Use minimal GitHub Actions permissions
- Don't store secrets in workflow files
- Use GitHub Secrets for sensitive data
- Review third-party actions before use
- Pin actions to specific SHA (not tags)

## Security Scanning Schedule

### Continuous

- ✅ Pre-commit hooks on every commit
- ✅ CI/CD checks on every push
- ✅ PR checks on every pull request

### Weekly

- ✅ CodeQL security analysis (Mondays 00:00 UTC)
- ✅ Dependabot dependency scans
- ✅ Pre-commit hook updates

### Monthly

- Manual security audit (recommended)
- Dependency review and cleanup
- Security policy review

## Security Checklist

Use this checklist when setting up a new repository from this template:

- [ ] Install pre-commit hooks
- [ ] Configure Gitleaks allowlists
- [ ] Enable branch protection
- [ ] Configure Dependabot
- [ ] Set up GitHub Secrets
- [ ] Review CodeQL configuration
- [ ] Add CODEOWNERS file
- [ ] Configure GPG signing
- [ ] Review workflow permissions
- [ ] Test security scans

## Contact

For security-related questions or concerns:

- **Security issues**: Use GitHub Security Advisories
- **General questions**: Open a [Discussion](../../discussions)
- **Urgent matters**: Contact maintainers via email

## Acknowledgments

We appreciate the security research community's efforts to responsibly disclose vulnerabilities and keep open source projects secure.

Security researchers who report valid vulnerabilities:

- Will be credited in security advisories (if desired)
- Will receive our sincere thanks
- Help make the open source ecosystem safer

## Additional Resources

- **[OWASP Top 10](https://owasp.org/www-project-top-ten/)** - Common web vulnerabilities
- **[CWE Top 25](https://cwe.mitre.org/top25/)** - Most dangerous software weaknesses
- **[GitHub Security Best Practices](https://docs.github.com/en/code-security)** - GitHub security features
- **[Gitleaks Documentation](https://github.com/gitleaks/gitleaks)** - Secret scanning
- **[CodeQL Documentation](https://codeql.github.com/)** - Code analysis

---

**Security is everyone's responsibility.** Thank you for helping keep this project and its users safe! 🔒
