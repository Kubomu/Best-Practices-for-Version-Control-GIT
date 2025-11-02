# Git Hooks Examples

Practical Git hooks you can use in your projects to automate checks and enforce best practices.

## Available Hooks

### pre-commit
Runs before creating a commit. Checks for:
- Debugging statements (console.log, print, etc.)
- TODO/FIXME without issue references
- Large files (>1MB)
- Merge conflict markers
- Trailing whitespace

### commit-msg
Validates commit messages. Checks for:
- Empty messages
- Minimum length (10 characters)
- WIP commits on main branch
- Long subject lines (warns if >50 chars)
- Optional: Conventional Commits format

### pre-push
Runs before pushing to remote. Checks for:
- Direct pushes to protected branches (main, master, production)
- Optional: Run tests before push
- Commits mentioning secrets/passwords
- Force push warnings

## Installation

### Quick Install (Single Hook)

```bash
# Copy hook to your repository
cp templates/hooks/pre-commit .git/hooks/pre-commit

# Make it executable
chmod +x .git/hooks/pre-commit
```

### Install All Hooks

```bash
# Copy all hooks
cp templates/hooks/pre-commit .git/hooks/
cp templates/hooks/commit-msg .git/hooks/
cp templates/hooks/pre-push .git/hooks/

# Make them executable
chmod +x .git/hooks/pre-commit
chmod +x .git/hooks/commit-msg
chmod +x .git/hooks/pre-push
```

### Team-Wide Setup

For sharing hooks with your team:

```bash
# Create hooks directory in your repository
mkdir -p .githooks

# Copy hooks there
cp templates/hooks/* .githooks/

# Configure Git to use this directory
git config core.hooksPath .githooks

# Make hooks executable
chmod +x .githooks/*

# Commit to repository
git add .githooks/
git commit -m "Add Git hooks for team"
```

Team members then run:
```bash
git config core.hooksPath .githooks
```

## Customization

### Modifying Checks

Edit the hook files to match your project needs:

```bash
# Edit pre-commit hook
vim .git/hooks/pre-commit

# Example: Add Python-specific checks
if git diff --cached --name-only | grep -E '\.py$' | xargs pylint 2>/dev/null; then
    echo "✅ Pylint checks passed"
else
    echo "❌ Pylint found issues"
    exit 1
fi
```

### Enable Conventional Commits

In `commit-msg` hook, uncomment the conventional commits check:

```bash
# Uncomment these lines in commit-msg
if ! echo "$commit_msg" | grep -qE '^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .{1,}$'; then
    echo "❌ Error: Commit message doesn't follow conventional commits format"
    exit 1
fi
```

### Add Test Running

In `pre-push` hook, uncomment test commands for your language:

```bash
# Node.js
npm test || { echo "❌ Tests failed"; exit 1; }

# Python
python -m pytest || { echo "❌ Tests failed"; exit 1; }

# Go
go test ./... || { echo "❌ Tests failed"; exit 1; }
```

## Bypassing Hooks

Sometimes you need to bypass hooks (use sparingly):

```bash
# Skip pre-commit hook
git commit --no-verify -m "Emergency fix"

# Skip pre-push hook
git push --no-verify
```

**⚠️ Warning**: Only bypass hooks when absolutely necessary!

## Troubleshooting

### Hook Not Running

```bash
# Check if hook exists
ls -la .git/hooks/

# Check if it's executable
ls -la .git/hooks/pre-commit

# Make it executable if needed
chmod +x .git/hooks/pre-commit

# Verify hook path (if using custom path)
git config core.hooksPath
```

### Hook Fails on Windows

Windows users may need to:

1. Use Git Bash or WSL
2. Ensure correct line endings:
   ```bash
   dos2unix .git/hooks/pre-commit
   ```
3. Check shebang line (`#!/bin/bash`)

### Permission Denied

```bash
# Make hook executable
chmod +x .git/hooks/pre-commit

# If using Windows, run in Git Bash as administrator
```

## Hook Workflow Examples

### Example 1: Python Project

```bash
# pre-commit: Run Black formatter
if ! black --check .; then
    echo "❌ Code not formatted with Black"
    echo "Run: black ."
    exit 1
fi

# pre-commit: Run tests
if ! pytest; then
    echo "❌ Tests failed"
    exit 1
fi
```

### Example 2: Node.js Project

```bash
# pre-commit: Run ESLint
if ! npm run lint; then
    echo "❌ Linting failed"
    exit 1
fi

# pre-commit: Run Prettier
if ! npm run format:check; then
    echo "❌ Code not formatted"
    exit 1
fi
```

### Example 3: Go Project

```bash
# pre-commit: Run gofmt
if [ $(gofmt -l . | wc -l) -gt 0 ]; then
    echo "❌ Code not formatted with gofmt"
    exit 1
fi

# pre-commit: Run tests
if ! go test ./...; then
    echo "❌ Tests failed"
    exit 1
fi
```

## Best Practices

1. **Keep Hooks Fast**: Long-running hooks slow down development
2. **Provide Clear Messages**: Explain what failed and how to fix it
3. **Make Them Optional**: Allow bypassing in emergencies
4. **Test Hooks**: Verify they work for all team members
5. **Document Requirements**: List any tools hooks depend on
6. **Version Control**: Store hooks in repository for team sharing
7. **Gradual Rollout**: Start with warnings before enforcing errors

## Advanced: Hooks with Dependencies

### Using pre-commit Framework

For more sophisticated hooks, use the [pre-commit framework](https://pre-commit.com/):

```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-added-large-files
```

Install:
```bash
pip install pre-commit
pre-commit install
```

## Security Considerations

Hooks can be security risks if:
- Downloaded from untrusted sources
- Execute arbitrary code
- Have write access to system

**Always review hooks before installing!**

## Related Documentation

- [Security Best Practices](../../security-best-practices.md#pre-commit-security-hooks)
- [Commit Message Best Practices](../../commit-message-best-practices.md)
- [Contributing Guidelines](../../contributing.md)

## External Resources

- [Git Hooks Documentation](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)
- [pre-commit Framework](https://pre-commit.com/)
- [Husky (Node.js hooks)](https://typicode.github.io/husky/)
