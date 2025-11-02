# .gitignore Templates

Ready-to-use `.gitignore` templates for common programming languages and frameworks.

## Available Templates

- **[Python.gitignore](Python.gitignore)** - Python projects (Django, Flask, pytest, venv)
- **[Node.gitignore](Node.gitignore)** - Node.js/JavaScript (npm, yarn, Next.js, React)
- **[Java.gitignore](Java.gitignore)** - Java projects (Maven, Gradle, IntelliJ, Eclipse)
- **[Go.gitignore](Go.gitignore)** - Go projects (modules, binaries, vendor)

## How to Use

### Method 1: Copy Template

```bash
# Navigate to your project
cd your-project

# Copy template (replace 'Python' with your language)
cp path/to/templates/gitignore/Python.gitignore .gitignore

# Customize as needed
vim .gitignore

# Add to git
git add .gitignore
git commit -m "Add .gitignore"
```

### Method 2: Download Directly

```bash
# From this repository
curl -O https://raw.githubusercontent.com/Kubomu/Best-Practices-for-Version-Control-GIT/main/templates/gitignore/Python.gitignore

# Rename to .gitignore
mv Python.gitignore .gitignore
```

### Method 3: Use GitHub's Templates

When creating a repository on GitHub, select a `.gitignore` template from the dropdown.

## Customizing Templates

These templates are comprehensive starting points. Customize based on your needs:

### Add Project-Specific Patterns

```gitignore
# Your custom ignores
config/local.yml
data/
*.backup
```

### Keep Certain Files

Use `!` to negate a pattern:

```gitignore
# Ignore all .env files
.env*

# But keep .env.example
!.env.example
```

### Directory vs File

```gitignore
logs/        # Ignore logs directory
*.log        # Ignore all .log files
debug.log    # Ignore specific file
```

## Testing Your .gitignore

```bash
# Check if file would be ignored
git check-ignore -v filename

# See what's currently ignored
git status --ignored
```

## Common Patterns Explained

| Pattern | Matches |
|---------|---------|
| `*.log` | All files ending in .log |
| `logs/` | Directory named logs |
| `**/temp` | Any directory named temp |
| `*.py[cod]` | .pyc, .pyo, .pyd files |
| `!important.log` | Exception: don't ignore this |

## Best Practices

1. **Add .gitignore early** - Before your first commit
2. **Don't ignore committed files** - Use `git rm --cached` first
3. **Keep it organized** - Group related patterns with comments
4. **Use global .gitignore** - For IDE/OS files across all projects
5. **Don't ignore too much** - Be specific to avoid losing important files

## Global .gitignore Setup

For IDE and OS files that apply to all projects:

```bash
# Create global gitignore
touch ~/.gitignore_global

# Configure Git to use it
git config --global core.excludesfile ~/.gitignore_global

# Add common patterns
cat << EOF >> ~/.gitignore_global
# IDEs
.vscode/
.idea/
*.swp

# OS
.DS_Store
Thumbs.db
EOF
```

## When to NOT Use .gitignore

**DO track these files:**
- Source code
- Configuration templates (.env.example)
- Documentation
- Build scripts
- Dependencies configuration (package.json, requirements.txt)

**DON'T track these:**
- Compiled code
- Dependencies (node_modules/, venv/)
- Secrets (.env with actual values)
- Build outputs (dist/, build/)
- IDE settings
- OS files (.DS_Store)

## More Templates

Need templates for other languages? Check:
- [github.com/github/gitignore](https://github.com/github/gitignore) - Official GitHub collection
- [gitignore.io](https://www.gitignore.io/) - Generate custom .gitignore files
- [toptal.com/developers/gitignore](https://www.toptal.com/developers/gitignore) - Interactive generator

## Contributing

Have improvements for these templates? See [../../contributing.md](../../contributing.md) for guidelines.

## Related Resources

- [Security Best Practices](../../security-best-practices.md#gitignore-patterns-and-templates)
- [Git Basics](../../git-basics.md)
- [FAQ - .gitignore questions](../../faq.md)
