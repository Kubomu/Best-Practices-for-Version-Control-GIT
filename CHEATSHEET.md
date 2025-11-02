# Git Quick Reference Cheat Sheet

One-page reference for the most common Git commands.

## Setup & Configuration

```bash
# Set user info
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# View all config
git config --list

# Set default editor
git config --global core.editor "vim"

# Set default branch name
git config --global init.defaultBranch main
```

## Creating Repositories

```bash
# Initialize new repo
git init

# Clone existing repo
git clone <url>

# Clone specific branch
git clone -b <branch> <url>

# Shallow clone (faster, less history)
git clone --depth 1 <url>
```

## Basic Commands

```bash
# Check status
git status

# Add files to staging
git add <file>
git add .                # All files
git add *.js             # Pattern match

# Commit changes
git commit -m "message"
git commit -am "message" # Add & commit tracked files

# View commit history
git log
git log --oneline
git log --graph --all
```

## Branching

```bash
# List branches
git branch               # Local
git branch -r            # Remote
git branch -a            # All

# Create branch
git branch <name>

# Switch branch
git checkout <name>
git switch <name>        # Newer syntax

# Create and switch
git checkout -b <name>
git switch -c <name>

# Delete branch
git branch -d <name>     # Safe delete
git branch -D <name>     # Force delete

# Rename branch
git branch -m <old> <new>
```

## Merging

```bash
# Merge branch into current
git merge <branch>

# Merge with no fast-forward
git merge --no-ff <branch>

# Abort merge
git merge --abort

# View merged branches
git branch --merged
```

## Remote Operations

```bash
# List remotes
git remote -v

# Add remote
git remote add <name> <url>

# Fetch from remote
git fetch origin

# Pull (fetch + merge)
git pull origin <branch>

# Push to remote
git push origin <branch>
git push -u origin <branch>  # Set upstream

# Delete remote branch
git push origin --delete <branch>
```

## Undoing Changes

```bash
# Unstage file
git restore --staged <file>
git reset HEAD <file>

# Discard changes in working directory
git restore <file>
git checkout -- <file>

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (unstage changes)
git reset HEAD~1
git reset --mixed HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Revert commit (creates new commit)
git revert <commit>
```

## Stashing

```bash
# Stash changes
git stash
git stash save "message"

# List stashes
git stash list

# Apply stash
git stash apply          # Keep in list
git stash pop            # Apply & remove

# Apply specific stash
git stash apply stash@{2}

# Drop stash
git stash drop stash@{1}

# Clear all stashes
git stash clear
```

## Viewing Changes

```bash
# View unstaged changes
git diff

# View staged changes
git diff --staged
git diff --cached

# View changes between branches
git diff branch1..branch2

# View changes in specific commit
git show <commit>

# View who changed what
git blame <file>
```

## Rebase

```bash
# Rebase current branch onto another
git rebase <branch>

# Interactive rebase (last 3 commits)
git rebase -i HEAD~3

# Continue rebase after resolving conflicts
git rebase --continue

# Abort rebase
git rebase --abort

# Skip current commit during rebase
git rebase --skip
```

## Cherry-Pick

```bash
# Apply specific commit to current branch
git cherry-pick <commit>

# Cherry-pick multiple commits
git cherry-pick <commit1> <commit2>

# Cherry-pick without committing
git cherry-pick -n <commit>

# Abort cherry-pick
git cherry-pick --abort
```

## Tags

```bash
# List tags
git tag

# Create lightweight tag
git tag v1.0.0

# Create annotated tag
git tag -a v1.0.0 -m "Version 1.0.0"

# Tag specific commit
git tag v1.0.0 <commit>

# Push tags to remote
git push origin v1.0.0   # Single tag
git push origin --tags   # All tags

# Delete tag
git tag -d v1.0.0                    # Local
git push origin --delete v1.0.0     # Remote
```

## Reflog

```bash
# View reflog
git reflog

# View reflog for specific branch
git reflog show <branch>

# Recover lost commit
git reset --hard HEAD@{2}
git checkout <commit-from-reflog>
```

## Bisect (Find Bug)

```bash
# Start bisect
git bisect start

# Mark current commit as bad
git bisect bad

# Mark known good commit
git bisect good <commit>

# Test & mark as good/bad
git bisect good
git bisect bad

# End bisect
git bisect reset
```

## Submodules

```bash
# Add submodule
git submodule add <url> <path>

# Initialize submodules
git submodule init

# Update submodules
git submodule update

# Clone with submodules
git clone --recursive <url>

# Update all submodules
git submodule update --remote
```

## Aliases (Add to ~/.gitconfig)

```bash
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    last = log -1 HEAD
    unstage = reset HEAD --
    visual = log --graph --oneline --all
    undo = reset --soft HEAD~1
```

## Git Workflow Cheat

### Feature Branch Workflow

```bash
# 1. Update main
git checkout main
git pull origin main

# 2. Create feature branch
git checkout -b feature/my-feature

# 3. Work & commit
git add .
git commit -m "Add feature"

# 4. Push to remote
git push -u origin feature/my-feature

# 5. Create Pull Request on GitHub/GitLab

# 6. After approval, merge & delete
git checkout main
git pull origin main
git branch -d feature/my-feature
```

### Gitflow Workflow

```bash
# Start feature
git checkout develop
git checkout -b feature/new-feature

# Finish feature
git checkout develop
git merge feature/new-feature

# Start release
git checkout -b release/1.0 develop

# Finish release
git checkout main
git merge release/1.0
git tag v1.0.0
git checkout develop
git merge release/1.0

# Hotfix
git checkout -b hotfix/1.0.1 main
# Fix, commit
git checkout main
git merge hotfix/1.0.1
git checkout develop
git merge hotfix/1.0.1
```

## Common Scenarios

### Fix Last Commit Message

```bash
git commit --amend -m "Corrected message"
```

### Add More to Last Commit

```bash
# Make changes
git add .
git commit --amend --no-edit
```

### Committed to Wrong Branch

```bash
# Move commit to new branch
git branch feature-branch
git reset --hard HEAD~1
git checkout feature-branch
```

### Discard All Local Changes

```bash
git restore .              # Discard unstaged
git restore --staged .     # Unstage all
git clean -fd              # Remove untracked files
```

### Update Feature Branch with Main

```bash
# Method 1: Merge
git checkout feature-branch
git merge main

# Method 2: Rebase (cleaner)
git checkout feature-branch
git rebase main
```

### Resolve Merge Conflicts

```bash
# 1. See conflicted files
git status

# 2. Edit files, resolve conflicts

# 3. Mark as resolved
git add <resolved-files>

# 4. Continue
git merge --continue
# or
git rebase --continue
```

## Special Symbols

| Symbol | Meaning |
|--------|---------|
| `HEAD` | Current commit |
| `HEAD~1` | One commit before HEAD |
| `HEAD~2` | Two commits before HEAD |
| `HEAD^` | Parent of HEAD |
| `@` | Shortcut for HEAD |
| `@{-1}` | Previous branch |
| `@{u}` | Upstream branch |

## Useful Flags

| Flag | Effect |
|------|--------|
| `-f` | Force |
| `-d` | Delete |
| `-a` | All |
| `-v` | Verbose |
| `-n` | Dry run |
| `-u` | Set upstream |
| `--hard` | Discard changes |
| `--soft` | Keep changes staged |
| `--mixed` | Keep changes unstaged |

## Emergency Commands

```bash
# Oh no, I messed up!
git reflog                    # Find lost commits

# Undo hard reset
git reset --hard HEAD@{1}

# Recover deleted branch
git checkout -b recovered <commit>

# Abort everything
git merge --abort
git rebase --abort
git cherry-pick --abort

# Start fresh (DANGER!)
git reset --hard origin/main
git clean -fd
```

## Tips & Tricks

```bash
# View commits by author
git log --author="John"

# View commits last week
git log --since="1 week ago"

# View file history
git log --follow <file>

# Search commits
git log --grep="fix"

# Count commits
git rev-list --count HEAD

# View repository size
git count-objects -vH

# Optimize repository
git gc --aggressive

# Check what would be ignored
git check-ignore -v <file>
```

## Getting Help

```bash
# Help for specific command
git help <command>
git <command> --help

# Quick help
git <command> -h
```

---

**Pro Tip**: Print this page and keep it at your desk! Or bookmark it for quick reference.

For detailed explanations, see the full documentation:
- [Git Basics](git-basics.md)
- [Advanced Git Techniques](advanced-git-techniques.md)
- [Troubleshooting Guide](troubleshooting-guide.md)
