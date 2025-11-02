# Security Best Practices for Git

Protect your code, credentials, and sensitive data with these essential security practices.

## Table of Contents
1. [.gitignore Patterns and Templates](#gitignore-patterns-and-templates)
2. [Credential Management](#credential-management)
3. [GPG Commit Signing](#gpg-commit-signing)
4. [Branch Protection Rules](#branch-protection-rules)
5. [Sensitive Data Removal](#sensitive-data-removal)
6. [Pre-commit Security Hooks](#pre-commit-security-hooks)
7. [Security Checklist](#security-checklist)

---

## .gitignore Patterns and Templates

The `.gitignore` file prevents sensitive and unnecessary files from being committed to your repository.

### Essential .gitignore Patterns

```gitignore
# Environment Variables & Secrets
.env
.env.*
*.pem
*.key
*.p12
*.pfx
secrets.yml
config/secrets.yml
credentials.json

# API Keys & Tokens
.api-keys
*secret*
*token*
*password*

# Cloud Provider Credentials
.aws/
.azure/
.gcp/

# Database
*.sql
*.sqlite
*.db
database.yml

# IDE & Editor Files
.vscode/
.idea/
*.swp
*.swo
*~
.DS_Store

# Dependencies
node_modules/
vendor/
bower_components/

# Build Outputs
dist/
build/
*.log
*.tmp
tmp/

# Operating System
Thumbs.db
desktop.ini
```

### Language-Specific Templates

#### Python
```gitignore
# Byte-compiled / optimized
__pycache__/
*.py[cod]
*$py.class

# Virtual environments
venv/
env/
ENV/
.venv/

# Testing
.pytest_cache/
.coverage
htmlcov/

# Distribution
dist/
build/
*.egg-info/
```

#### Node.js
```gitignore
# Dependencies
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Environment
.env
.env.local
.env.*.local

# Build
dist/
build/
.next/
out/

# Testing
coverage/
.nyc_output/
```

#### Java
```gitignore
# Compiled class files
*.class
target/
*.jar
*.war
*.ear

# IDE
.idea/
*.iml
.project
.classpath

# Build tools
.gradle/
build/
```

#### Go
```gitignore
# Binary
*.exe
*.exe~
*.dll
*.so
*.dylib
main

# Test binary
*.test

# Output
bin/
vendor/
```

### Global .gitignore

Set up a global gitignore for your system:

```bash
# Create global gitignore
touch ~/.gitignore_global

# Configure Git to use it
git config --global core.excludesfile ~/.gitignore_global

# Add common patterns
cat << EOF >> ~/.gitignore_global
.DS_Store
Thumbs.db
*.swp
.idea/
.vscode/
EOF
```

### Try It Yourself

```bash
# Check what would be ignored
git status --ignored

# Test .gitignore patterns
git check-ignore -v filename

# Force add ignored file (use carefully!)
git add -f file.txt
```

---

## Credential Management

Never commit credentials to your repository. Here's how to manage them securely.

### Environment Variables

```bash
# Store credentials in .env file (add to .gitignore!)
echo "API_KEY=your_secret_key" >> .env
echo "DB_PASSWORD=secret123" >> .env
echo ".env" >> .gitignore

# Use in your application
# Node.js example:
require('dotenv').config();
const apiKey = process.env.API_KEY;

# Python example:
import os
from dotenv import load_dotenv
load_dotenv()
api_key = os.getenv('API_KEY')
```

### Git Credential Helper

```bash
# Store credentials securely
git config --global credential.helper store
# Credentials stored in ~/.git-credentials (still plain text!)

# Better: Use system credential manager
# macOS
git config --global credential.helper osxkeychain

# Windows
git config --global credential.helper wincred

# Linux (with libsecret)
git config --global credential.helper libsecret

# Timeout for cached credentials (in seconds)
git config --global credential.helper 'cache --timeout=3600'
```

### SSH Keys Instead of Passwords

```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"

# Start ssh-agent
eval "$(ssh-agent -s)"

# Add SSH key
ssh-add ~/.ssh/id_ed25519

# Copy public key to clipboard (add to GitHub/GitLab)
# macOS
pbcopy < ~/.ssh/id_ed25519.pub
# Linux
xclip -sel clip < ~/.ssh/id_ed25519.pub
# Or just cat it
cat ~/.ssh/id_ed25519.pub

# Test connection
ssh -T git@github.com
```

### Secret Management Tools

```bash
# AWS Secrets Manager
aws secretsmanager get-secret-value --secret-id prod/db/password

# HashiCorp Vault
vault kv get secret/myapp/config

# Azure Key Vault
az keyvault secret show --name "DbPassword" --vault-name "MyKeyVault"

# git-secret (encrypt secrets in repo)
git secret init
git secret tell email@example.com
git secret add .env
git secret hide  # Encrypt
git secret reveal  # Decrypt
```

### Try It Yourself

```bash
# Check current credential helper
git config --list | grep credential

# Set up SSH key authentication
ssh-keygen -t ed25519 -C "test@example.com"
cat ~/.ssh/id_ed25519.pub  # Add to your Git hosting service
```

---

## GPG Commit Signing

Sign your commits to prove they came from you.

### Setting Up GPG

```bash
# Generate GPG key
gpg --full-generate-key
# Choose: RSA and RSA, 4096 bits, key doesn't expire (or set expiry)

# List GPG keys
gpg --list-secret-keys --keyid-format LONG

# Output example:
# sec   rsa4096/3AA5C34371567BD2 2024-01-01 [SC]
# uid                 Your Name <email@example.com>

# Export public key (add to GitHub/GitLab)
gpg --armor --export 3AA5C34371567BD2

# Configure Git to use GPG key
git config --global user.signingkey 3AA5C34371567BD2

# Sign commits by default
git config --global commit.gpgsign true

# Sign tags by default
git config --global tag.gpgsign true
```

### Signing Commits and Tags

```bash
# Sign a commit
git commit -S -m "Signed commit"

# Sign a tag
git tag -s v1.0 -m "Signed version 1.0"

# Verify signed commit
git verify-commit HEAD

# Verify signed tag
git verify-tag v1.0

# Show signature info
git log --show-signature
```

### Troubleshooting GPG

```bash
# GPG agent not working
export GPG_TTY=$(tty)
echo 'export GPG_TTY=$(tty)' >> ~/.bashrc

# Test GPG
echo "test" | gpg --clearsign

# Fix "gpg: signing failed: Inappropriate ioctl for device"
echo "use-agent" >> ~/.gnupg/gpg.conf
echo "pinentry-mode loopback" >> ~/.gnupg/gpg.conf
```

### Try It Yourself

```bash
# Create test repo with signed commits
mkdir signed-repo && cd signed-repo
git init
echo "test" >> file.txt
git add .
git commit -S -m "Signed commit"
git log --show-signature
```

---

## Branch Protection Rules

Protect critical branches from accidental or unauthorized changes.

### GitHub Branch Protection

```bash
# Via GitHub UI: Settings → Branches → Add rule

# Common protections:
✓ Require pull request reviews before merging
✓ Require status checks to pass
✓ Require signed commits
✓ Require linear history
✓ Include administrators
✓ Restrict who can push
✓ Require conversation resolution before merging
```

### GitLab Protected Branches

```bash
# Via GitLab UI: Settings → Repository → Protected Branches

# Protection levels:
- No one can push
- Developers can push
- Maintainers can push

# Merge access:
- No one can merge
- Developers can merge
- Maintainers can merge
```

### Pre-receive Hooks (Server-side)

```bash
# Example: Reject commits without issue reference
#!/bin/bash
while read oldrev newrev refname; do
  for commit in $(git rev-list $oldrev..$newrev); do
    message=$(git log -1 --format=%B $commit)
    if ! echo "$message" | grep -qE '#[0-9]+'; then
      echo "Error: Commit $commit missing issue reference"
      exit 1
    fi
  done
done
```

### Local Branch Protection

```bash
# Prevent accidental push to main
# Add to .git/hooks/pre-push
#!/bin/bash
protected_branch='main'
current_branch=$(git symbolic-ref HEAD | sed -e 's,.*/\(.*\),\1,')

if [ $current_branch = $protected_branch ]; then
  read -p "You're about to push to $protected_branch, are you sure? [y|n] " -n 1 -r < /dev/tty
  echo
  if [[ $REPLY =~ ^[Yy]$ ]]; then
    exit 0
  fi
  exit 1
fi
```

### Try It Yourself

```bash
# Create pre-push hook
cat << 'EOF' > .git/hooks/pre-push
#!/bin/bash
branch=$(git rev-parse --abbrev-ref HEAD)
if [ "$branch" = "main" ]; then
  echo "Direct push to main prevented!"
  exit 1
fi
EOF

chmod +x .git/hooks/pre-push

# Test it
git checkout main
git commit --allow-empty -m "test"
git push  # Should be blocked!
```

---

## Sensitive Data Removal

Accidentally committed secrets? Here's how to remove them from history.

### Using BFG Repo-Cleaner (Recommended)

```bash
# Install BFG
# Download from: https://rtyley.github.io/bfg-repo-cleaner/
# Or: brew install bfg (macOS)

# Clone a fresh copy with full history
git clone --mirror https://github.com/user/repo.git

# Remove passwords from history
bfg --replace-text passwords.txt repo.git

# Remove large files
bfg --strip-blobs-bigger-than 10M repo.git

# Remove specific file
bfg --delete-files credentials.json repo.git

# Clean up
cd repo.git
git reflog expire --expire=now --all
git gc --prune=now --aggressive

# Push cleaned history
git push --force
```

### Using git-filter-repo (Recommended)

```bash
# Install git-filter-repo
pip install git-filter-repo

# Remove file from all history
git filter-repo --path credentials.json --invert-paths

# Remove by pattern
git filter-repo --path-glob '*.pem' --invert-paths

# Replace text in all history
git filter-repo --replace-text <(echo "API_KEY=secret123==>API_KEY=REDACTED")

# After cleaning, force push
git push --force --all
git push --force --tags
```

### Using git filter-branch (Legacy)

```bash
# Remove file from all commits
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch path/to/secrets.txt" \
  --prune-empty --tag-name-filter cat -- --all

# Remove from all refs
git for-each-ref --format="%(refname)" refs/original/ | \
  xargs -n 1 git update-ref -d

# Cleanup
git reflog expire --expire=now --all
git gc --prune=now --aggressive

# Force push
git push --force --all
git push --force --tags
```

### After Removing Secrets

```bash
# 1. Rotate compromised credentials immediately!
# 2. Notify team members to rebase their work
# 3. Force all team members to re-clone

# Team members should:
git fetch origin
git reset --hard origin/main
# Or re-clone
git clone https://github.com/user/repo.git
```

### Try It Yourself (Safe Practice)

```bash
# Create test repo with "secret"
mkdir secret-test && cd secret-test
git init
echo "SECRET_KEY=abc123" >> secret.txt
git add . && git commit -m "Add secret"
echo "more work" >> other.txt
git add . && git commit -m "More work"

# Install and use BFG or git-filter-repo
# Practice removing secret.txt from history
```

---

## Pre-commit Security Hooks

Automate security checks before commits.

### Installing pre-commit Framework

```bash
# Install pre-commit
pip install pre-commit

# Create .pre-commit-config.yaml
cat << 'EOF' > .pre-commit-config.yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      - id: detect-private-key
      - id: check-added-large-files
        args: ['--maxkb=1000']
      - id: check-merge-conflict
      - id: trailing-whitespace
      - id: end-of-file-fixer

  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets
        args: ['--baseline', '.secrets.baseline']

  - repo: https://github.com/gitleaks/gitleaks
    rev: v8.16.1
    hooks:
      - id: gitleaks
EOF

# Install hooks
pre-commit install

# Run manually
pre-commit run --all-files
```

### Custom Security Hook

```bash
# Create .git/hooks/pre-commit
cat << 'EOF' > .git/hooks/pre-commit
#!/bin/bash

# Check for common secret patterns
FILES=$(git diff --cached --name-only)

for file in $FILES; do
  # Check for API keys
  if grep -qE '(api[_-]?key|apikey).{0,20}[a-zA-Z0-9]{20,}' "$file"; then
    echo "Potential API key detected in $file"
    exit 1
  fi

  # Check for passwords
  if grep -qE '(password|passwd|pwd).{0,20}["\'].*["\']' "$file"; then
    echo "Potential password detected in $file"
    exit 1
  fi

  # Check for AWS keys
  if grep -qE 'AKIA[0-9A-Z]{16}' "$file"; then
    echo "AWS Access Key detected in $file"
    exit 1
  fi

  # Check for private keys
  if grep -q "BEGIN.*PRIVATE KEY" "$file"; then
    echo "Private key detected in $file"
    exit 1
  fi
done

echo "Security check passed"
exit 0
EOF

chmod +x .git/hooks/pre-commit
```

### Tools for Secret Detection

```bash
# Gitleaks
gitleaks detect --source . --verbose

# TruffleHog
trufflehog git file://. --json

# detect-secrets
detect-secrets scan --all-files > .secrets.baseline

# git-secrets (AWS)
git secrets --install
git secrets --register-aws
git secrets --scan
```

---

## Security Checklist

### Before Starting a Project
- [ ] Create comprehensive `.gitignore` file
- [ ] Set up pre-commit hooks for secret detection
- [ ] Configure GPG commit signing
- [ ] Use SSH keys for authentication
- [ ] Set up credential helper

### During Development
- [ ] Never commit credentials or API keys
- [ ] Use environment variables for secrets
- [ ] Review diffs before committing
- [ ] Keep `.gitignore` updated
- [ ] Sign important commits/tags

### Before Pushing
- [ ] Run security scans (`gitleaks`, `trufflehog`)
- [ ] Review commit history for sensitive data
- [ ] Ensure tests pass
- [ ] Verify branch protection rules

### After Committing Secrets (Emergency)
- [ ] Rotate/invalidate compromised credentials immediately
- [ ] Remove from Git history using BFG/git-filter-repo
- [ ] Force push cleaned history
- [ ] Notify team to re-clone
- [ ] Review and improve prevention measures

### Repository Maintenance
- [ ] Regular security audits
- [ ] Update dependencies regularly
- [ ] Review and update branch protection rules
- [ ] Monitor for leaked credentials
- [ ] Keep hooks and tools updated

---

## Best Practices Summary

1. **Prevention is Better than Cure**: Use `.gitignore` and pre-commit hooks
2. **Never Commit Secrets**: Use environment variables and secret managers
3. **Use SSH Keys**: More secure than passwords
4. **Sign Your Commits**: Prove authenticity with GPG
5. **Protect Important Branches**: Use branch protection rules
6. **Regular Security Audits**: Scan for secrets regularly
7. **Rotate Credentials**: Even if not compromised, rotate periodically
8. **Educate Your Team**: Security is everyone's responsibility
9. **Have a Response Plan**: Know what to do if secrets are exposed
10. **Use Security Tools**: Automate detection and prevention

## Additional Resources

- [GitHub Security Best Practices](https://docs.github.com/en/code-security)
- [OWASP Git Security](https://owasp.org/www-community/controls/Git_Security)
- [Git SCM Security](https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work)
- [pre-commit Framework](https://pre-commit.com/)
- [Gitleaks Documentation](https://github.com/gitleaks/gitleaks)

Remember: Security is an ongoing process, not a one-time setup!
