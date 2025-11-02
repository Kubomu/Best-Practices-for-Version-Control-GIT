# Frequently Asked Questions (FAQ)

Common Git questions answered clearly and concisely.

## Table of Contents
- [General Git Questions](#general-git-questions)
- [Branching and Merging](#branching-and-merging)
- [Commits and History](#commits-and-history)
- [Remote Repositories](#remote-repositories)
- [Collaboration](#collaboration)
- [Advanced Topics](#advanced-topics)
- [Tools and Configuration](#tools-and-configuration)

---

## General Git Questions

### What's the difference between Git and GitHub?

**Git** is a version control system (software) that runs on your computer to track changes in files.

**GitHub** is a hosting service (website) for Git repositories that adds collaboration features like pull requests, issues, and project management.

**Analogy**: Git is like Microsoft Word, GitHub is like Google Docs.

Other hosting services: GitLab, Bitbucket, Gitea, etc.

### Do I need to be online to use Git?

**No!** Git is distributed, meaning:
- You can commit, branch, merge, and view history offline
- You only need internet to push/pull from remote repositories
- Full repository history is on your local machine

### How is Git different from SVN or other version control systems?

| Feature | Git | SVN |
|---------|-----|-----|
| Type | Distributed | Centralized |
| Offline work | Yes | Limited |
| Speed | Fast | Slower |
| Branching | Cheap & easy | Expensive |
| Commits | Local first | Direct to server |
| Storage | Full history | Working copy only |

### Can I use Git for non-code files?

**Yes**, but with limitations:
- **Good for**: Documentation, configuration, small text files
- **Not ideal for**: Large binaries, videos, compiled files, databases
- **For large files**: Use Git LFS (Large File Storage)

```bash
# Install Git LFS
git lfs install

# Track large files
git lfs track "*.psd"
git lfs track "*.mp4"

# Commit as normal
git add .gitattributes
git commit -m "Track large files with LFS"
```

---

## Branching and Merging

### When should I create a new branch?

Create a branch for:
- **Each new feature**: `feature/user-authentication`
- **Bug fixes**: `bugfix/login-error`
- **Experiments**: `experiment/new-algorithm`
- **Releases**: `release/v1.0`
- **Hotfixes**: `hotfix/security-patch`

**Rule of thumb**: If you're changing code, work in a branch!

### How do I name my branches?

Common naming conventions:

```bash
# Feature branches
feature/user-login
feature/payment-integration

# Bug fixes
bugfix/null-pointer-error
fix/broken-links

# Hotfixes
hotfix/security-vulnerability
hotfix/critical-crash

# Releases
release/v1.0.0
release/2024-q1

# Personal/experimental
username/experiment-name
spike/performance-testing
```

**Best practices**:
- Use lowercase with hyphens: `my-branch-name`
- Be descriptive but concise
- Include ticket/issue number: `feature/JIRA-123-user-login`
- Avoid special characters except `/`, `-`, `_`

### Should I use merge or rebase?

**Use Merge when**:
- Integrating feature branches into main
- You want to preserve complete history
- Working on public/shared branches
- Multiple people working on same branch

**Use Rebase when**:
- Updating feature branch with main
- Cleaning up local commits
- Want a linear history
- Working alone on a branch

```bash
# Merge (creates merge commit)
git checkout main
git merge feature-branch

# Rebase (linear history)
git checkout feature-branch
git rebase main
```

**Golden Rule**: Never rebase commits that have been pushed to shared branches!

### How do I delete a branch?

```bash
# Delete local branch (must not be checked out)
git branch -d branch-name       # Safe delete (prevents if not merged)
git branch -D branch-name       # Force delete

# Delete remote branch
git push origin --delete branch-name
```

### What's a merge commit?

A merge commit is a special commit with two parent commits, created when merging branches.

```bash
# This creates a merge commit
git merge feature-branch

# Avoid merge commit with fast-forward
git merge --ff-only feature-branch

# Avoid merge commit with rebase
git rebase main
```

---

## Commits and History

### How do I write a good commit message?

Follow this structure:

```
<type>: <short summary> (50 chars max)

<detailed description> (optional, 72 chars per line)

<footer> (optional: issue references, breaking changes)
```

**Examples**:

```bash
# Good
git commit -m "Add user authentication with JWT"
git commit -m "Fix null pointer error in payment service"
git commit -m "Update documentation for API endpoints"

# Bad
git commit -m "fixed stuff"
git commit -m "changes"
git commit -m "asdfasdf"
```

**Types**: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

See [Commit Message Best Practices](commit-message-best-practices.md) for more.

### How often should I commit?

**Commit frequently!**

- After completing a logical unit of work
- When tests pass
- Before switching contexts
- At least once per day of work

**Too small**: `git commit -m "added semicolon"`
**Too large**: `git commit -m "implemented entire user system"`
**Just right**: `git commit -m "Add user registration form validation"`

### Can I change a commit after pushing?

**If you haven't pushed**: Yes, easily with `git commit --amend`

**If you've pushed**:
- To personal branch: Yes, but requires force push
- To shared branch: Avoid! Use `git revert` instead

```bash
# Before push
git commit --amend -m "Corrected message"

# After push (personal branch only)
git commit --amend -m "Corrected message"
git push --force-with-lease

# After push (shared branch)
git revert HEAD
```

### What's the difference between HEAD, working directory, and staging area?

```
Working Directory: Your actual files
        ↓ git add
Staging Area: Files ready to commit
        ↓ git commit
HEAD: Last commit on current branch
```

**Example**:

```bash
# Working directory
echo "hello" >> file.txt

# Staging area
git add file.txt

# HEAD (committed)
git commit -m "Add hello"
```

### How do I view commit history?

```bash
# Basic log
git log

# One line per commit
git log --oneline

# With graph
git log --graph --oneline --all

# Last N commits
git log -n 5

# Commits by author
git log --author="John"

# Commits in date range
git log --since="2024-01-01" --until="2024-12-31"

# Changes in commit
git show abc123
```

---

## Remote Repositories

### What's the difference between origin and main?

- **origin**: Default name for remote repository
- **main** (or master): Default name for primary branch

```bash
# origin is where the remote repo is
git remote -v
# origin  https://github.com/user/repo.git

# main is a branch
git branch
# * main
```

You can have multiple remotes:

```bash
git remote add upstream https://github.com/original/repo.git
git remote add coworker https://github.com/coworker/repo.git
```

### What's the difference between fetch and pull?

**Fetch**: Downloads changes but doesn't apply them

```bash
git fetch origin
# Downloads changes
# You can review before merging
git merge origin/main
```

**Pull**: Fetch + Merge in one command

```bash
git pull origin main
# Same as:
# git fetch origin
# git merge origin/main
```

**Best practice**: Use `fetch` first to review changes, then `merge` or `rebase`.

### How do I connect my local repo to GitHub?

```bash
# Create repo on GitHub first, then:

# Add remote
git remote add origin https://github.com/username/repo.git

# Push first time
git push -u origin main

# Future pushes
git push
```

### What does -u mean in git push -u?

`-u` sets upstream tracking, so future `git push` and `git pull` know where to push/pull.

```bash
# First push with -u
git push -u origin feature-branch

# Now you can just use
git push
git pull
```

### How do I sync a fork with the original repository?

```bash
# Add original repo as upstream
git remote add upstream https://github.com/original/repo.git

# Fetch upstream changes
git fetch upstream

# Merge into your main
git checkout main
git merge upstream/main

# Push to your fork
git push origin main
```

---

## Collaboration

### How do I handle pull requests?

**Creating a PR**:

```bash
# Create feature branch
git checkout -b feature/my-feature

# Make changes and commit
git add .
git commit -m "Add new feature"

# Push to remote
git push -u origin feature/my-feature

# Create PR on GitHub/GitLab UI
```

**Reviewing a PR**:

```bash
# Checkout PR locally
git fetch origin pull/123/head:pr-123
git checkout pr-123

# Test changes
# Leave review comments in UI
```

### What is a conflict and how do I avoid them?

**Conflict** happens when two people modify the same part of a file.

**Avoid conflicts by**:
- Pulling frequently: `git pull origin main`
- Making small, focused changes
- Communicating with team
- Using feature flags for long-running features
- Keeping branches short-lived

### How do I collaborate on a branch with a teammate?

```bash
# Both work on same branch
# Person A pushes
git push origin feature-branch

# Person B pulls
git pull origin feature-branch

# If conflicts, resolve and push
git add .
git commit
git push origin feature-branch
```

**Better approach**: Create sub-branches

```bash
# Main feature branch
feature/payment-system

# Individual work
feature/payment-system-frontend
feature/payment-system-backend

# Merge to feature branch when done
```

---

## Advanced Topics

### What is a detached HEAD?

You're not on a branch, but directly on a commit.

```bash
# This causes detached HEAD
git checkout abc123

# Fix: create a branch
git checkout -b new-branch
```

See [Troubleshooting Guide](troubleshooting-guide.md#detached-head-state) for details.

### What is rebasing and when should I use it?

**Rebase** moves commits to a new base commit.

```bash
# Feature branch diverged from main
main:    A---B---C
              \
feature:       D---E

# After rebase
main:    A---B---C
                  \
feature:           D'---E'
```

**Use for**:
- Updating feature branch with main
- Cleaning up commit history
- Creating linear history

**Never rebase** commits pushed to shared branches!

See [Advanced Git Techniques](advanced-git-techniques.md#git-rebase) for details.

### What's the difference between reset, revert, and checkout?

- **reset**: Moves branch pointer (changes history)
- **revert**: Creates new commit that undoes changes (preserves history)
- **checkout**: Switches branches or restores files (doesn't change commits)

```bash
# Reset (removes commits)
git reset --hard HEAD~1

# Revert (adds commit that undoes)
git revert HEAD

# Checkout (switches branch)
git checkout main
```

See [Advanced Git Techniques](advanced-git-techniques.md#reset-vs-revert-vs-checkout) for details.

### What is reflog and how can it save me?

**Reflog** records every change to HEAD, acting as a safety net.

```bash
# View reflog
git reflog

# Recover "lost" commits
git reset --hard HEAD@{3}
```

**Use cases**:
- Undoing `git reset --hard`
- Recovering deleted branches
- Finding lost commits

See [Advanced Git Techniques](advanced-git-techniques.md#git-reflog) for details.

### Should I use submodules or subtrees?

**Submodules**: External repos linked as dependencies
- Better for: Clear separation, multiple projects sharing code
- Pros: Small repo size, independent versioning
- Cons: More complex workflow

**Subtrees**: External repos copied into your repo
- Better for: Vendoring dependencies, simpler workflow
- Pros: Easier to use, everything in one repo
- Cons: Larger repo size

See [Advanced Git Techniques](advanced-git-techniques.md#submodules-and-subtrees) for details.

---

## Tools and Configuration

### What's the best Git GUI?

Popular options:
- **GitHub Desktop**: Beginner-friendly, GitHub integration
- **GitKraken**: Powerful, beautiful UI, cross-platform
- **SourceTree**: Free, full-featured
- **Fork**: Fast, elegant (Mac/Windows)
- **Tower**: Professional, powerful (paid)
- **Built-in IDE**: VS Code, IntelliJ, etc.

**Recommendation**: Learn CLI first, then use GUI as supplement.

### How do I configure Git?

```bash
# User info (required)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Default editor
git config --global core.editor "vim"
git config --global core.editor "code --wait"  # VS Code

# Default branch name
git config --global init.defaultBranch main

# Colorful output
git config --global color.ui auto

# View all config
git config --list
```

### What are Git aliases and how do I create them?

**Aliases** are shortcuts for Git commands.

```bash
# Create aliases
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual 'log --graph --oneline --all'

# Use them
git st          # Instead of git status
git co main     # Instead of git checkout main
git visual      # Show graphical log
```

**Popular aliases**:

```bash
# Undo last commit
git config --global alias.undo 'reset --soft HEAD~1'

# View contributors
git config --global alias.contributors 'shortlog -sn'

# Show branches sorted by last modified
git config --global alias.recent 'branch --sort=-committerdate'

# Detailed log
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

### How do I ignore files that are already tracked?

```bash
# Stop tracking but keep file
git rm --cached filename
echo "filename" >> .gitignore
git add .gitignore
git commit -m "Stop tracking filename"

# Ignore changes to tracked file (local only)
git update-index --assume-unchanged filename

# Undo assume-unchanged
git update-index --no-assume-unchanged filename
```

### What's .gitkeep and why use it?

Git doesn't track empty directories. `.gitkeep` is a workaround.

```bash
# Create empty directory
mkdir logs
touch logs/.gitkeep
git add logs/.gitkeep
git commit -m "Add logs directory"
```

Note: `.gitkeep` is not a Git feature, just a convention. Any filename works.

### How do I speed up Git operations?

```bash
# Enable parallel operations
git config --global pack.threads 0

# Reduce network usage
git config --global core.compression 9

# Speed up status
git config --global core.untrackedCache true
git config --global core.fsmonitor true

# Shallow clone (faster, less history)
git clone --depth 1 https://github.com/user/repo.git

# Partial clone (large repos)
git clone --filter=blob:none https://github.com/user/repo.git
```

---

## Still Have Questions?

- Check the [Glossary](glossary.md) for Git terminology
- Read the [Troubleshooting Guide](troubleshooting-guide.md) for common issues
- Review [Advanced Git Techniques](advanced-git-techniques.md) for power user features
- Browse [External Resources](external-resources.md) for more learning materials
- Open an issue or discussion in this repository
- Search Stack Overflow with the `git` tag
- Read the official [Git Documentation](https://git-scm.com/doc)

---

## Contributing to this FAQ

Found an error? Have a question not covered here? Please:
1. Open an issue describing the question/error
2. Submit a pull request with the fix/addition
3. Follow the [Contributing Guidelines](contributing.md)

We welcome contributions that make Git more accessible to everyone!
