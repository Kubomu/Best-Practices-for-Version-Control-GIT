# Git Configuration Examples

Real-world Git configuration files you can use in your projects.

## Available Examples

### Configuration Files

- **[.gitconfig](configs/.gitconfig)** - Personal Git configuration with useful aliases
- **[.gitattributes](configs/.gitattributes)** - Line ending normalization and file type handling

## How to Use

### .gitconfig (Personal Configuration)

```bash
# View current config
git config --list

# Copy sections you want
cat examples/configs/.gitconfig

# Add to your personal config
vim ~/.gitconfig
# Paste desired sections

# Or copy entire file
cp examples/configs/.gitconfig ~/.gitconfig
# Then edit with your name/email
```

### .gitattributes (Repository Configuration)

```bash
# Copy to your project root
cp examples/configs/.gitattributes .gitattributes

# Customize for your project
vim .gitattributes

# Commit to repository
git add .gitattributes
git commit -m "Add .gitattributes for consistent line endings"
```

## Configuration Breakdown

### .gitconfig Features

1. **User Information**
   - Name and email for commits

2. **Core Settings**
   - Default editor
   - Line ending handling
   - Global .gitignore

3. **Aliases**
   - Shortcuts for common commands
   - Enhanced log formatting
   - Quick status checks

4. **Push/Pull Behavior**
   - Default push strategy
   - Auto-setup remote tracking
   - Rebase vs merge preference

5. **Merge/Diff Tools**
   - Conflict resolution tool
   - Diff viewing tool
   - Conflict style

### .gitattributes Features

1. **Line Ending Normalization**
   - Auto-detect text files
   - Force LF for source code
   - Force CRLF for Windows scripts

2. **Binary File Handling**
   - Mark images as binary
   - Mark archives as binary
   - Prevent line ending changes

3. **Git LFS Setup**
   - Examples for large files
   - Commented out by default

4. **Linguist Configuration**
   - Control language statistics
   - Mark generated files
   - Mark documentation

## Customization Tips

### Personal .gitconfig

```bash
# Add your own aliases
[alias]
    deploy = !git push origin main && ssh user@server 'cd /app && git pull'

# Set preferred editor
[core]
    editor = code --wait  # VS Code
    editor = subl -w      # Sublime Text
    editor = vim          # Vim

# Configure commit signing
[user]
    signingkey = YOUR_GPG_KEY
[commit]
    gpgsign = true
```

### Project .gitattributes

```bash
# For a Python project
*.py text eol=lf
*.ipynb text eol=lf
requirements.txt text eol=lf

# For a Node.js project
*.js text eol=lf
*.json text eol=lf
package.json text eol=lf
package-lock.json text eol=lf

# For large assets with Git LFS
*.psd filter=lfs diff=lfs merge=lfs -text
*.blend filter=lfs diff=lfs merge=lfs -text
```

## Testing Configuration

### Test .gitconfig Aliases

```bash
# Test an alias
git st  # Should run git status

# List all aliases
git aliases

# Show what an alias does
git config --get-regexp alias.st
```

### Test .gitattributes

```bash
# Check how Git treats a file
git check-attr -a file.txt

# See what filter is applied
git check-attr filter file.psd

# View current attributes
git ls-files --eol
```

## Platform-Specific Settings

### macOS/Linux

```bash
[core]
    autocrlf = input  # LF only
    editor = vim

[credential]
    helper = osxkeychain  # macOS
    helper = cache        # Linux
```

### Windows

```bash
[core]
    autocrlf = true  # Convert to CRLF on checkout
    editor = notepad

[credential]
    helper = wincred
```

## Global vs Local Config

```bash
# Global (affects all repositories)
git config --global user.name "Your Name"
git config --global alias.st status

# Local (affects only current repository)
git config --local user.email "work@company.com"
git config --local core.editor "code"

# System (affects all users)
sudo git config --system core.autocrlf true
```

## Advanced Configuration

### Conditional Includes

Different config for work vs personal:

```bash
# ~/.gitconfig
[includeIf "gitdir:~/work/"]
    path = ~/.gitconfig-work

[includeIf "gitdir:~/personal/"]
    path = ~/.gitconfig-personal

# ~/.gitconfig-work
[user]
    email = you@company.com

# ~/.gitconfig-personal
[user]
    email = you@personal.com
```

### Custom Commands

```bash
[alias]
    # Run shell commands
    serve = !git daemon --reuseaddr --verbose --base-path=. --export-all ./.git

    # Use with parameters
    whoami = !git config user.name && git config user.email

    # Complex scripts
    cleanup = !git branch --merged | grep -v '\\*' | xargs -n 1 git branch -d
```

## Troubleshooting

### Config Not Working

```bash
# Check which config file is used
git config --list --show-origin

# Show specific setting
git config user.name

# Show where it's defined
git config --show-origin user.name

# Remove setting
git config --unset user.name
```

### Line Ending Issues

```bash
# Refresh all files with new .gitattributes
git rm --cached -r .
git reset --hard

# Or
git add --renormalize .
```

## Best Practices

1. **Version Control .gitattributes**: Always commit to repository
2. **Don't Version .gitconfig**: It's personal configuration
3. **Document Custom Settings**: Comment your config files
4. **Test Before Sharing**: Verify aliases work as expected
5. **Use Global Ignore**: Put IDE/OS files in ~/.gitignore_global
6. **Conditional Configs**: Separate work and personal settings

## Related Documentation

- [Security Best Practices](../security-best-practices.md)
- [Git Basics](../git-basics.md)
- [Contributing Guidelines](../contributing.md)

## External Resources

- [Git Config Documentation](https://git-scm.com/docs/git-config)
- [Git Attributes Documentation](https://git-scm.com/docs/gitattributes)
- [GitHub's .gitattributes Examples](https://github.com/alexkaratarakis/gitattributes)
