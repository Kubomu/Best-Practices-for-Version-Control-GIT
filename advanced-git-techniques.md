# Advanced Git Techniques

Master these advanced Git techniques to become a power user and handle complex version control scenarios with confidence.

## Table of Contents
1. [Git Rebase](#git-rebase)
2. [Cherry-Picking Commits](#cherry-picking-commits)
3. [Git Stash](#git-stash)
4. [Reset vs Revert vs Checkout](#reset-vs-revert-vs-checkout)
5. [Git Reflog](#git-reflog)
6. [Git Bisect](#git-bisect)
7. [Submodules and Subtrees](#submodules-and-subtrees)

---

## Git Rebase

Rebase is a powerful tool for maintaining a clean, linear commit history by moving or combining commits.

### Basic Rebase

```bash
# Rebase current branch onto main
git checkout feature-branch
git rebase main

# Rebase flow
# 1. Git finds common ancestor
# 2. Saves your commits temporarily
# 3. Applies main's commits
# 4. Replays your commits on top
```

### Interactive Rebase

Interactive rebase lets you edit, reorder, squash, or drop commits.

```bash
# Rebase last 3 commits interactively
git rebase -i HEAD~3

# Rebase from a specific commit
git rebase -i abc123

# Common commands in interactive mode:
# pick   = use commit as-is
# reword = use commit, but edit message
# edit   = use commit, but stop for amending
# squash = combine with previous commit
# fixup  = like squash, but discard message
# drop   = remove commit
```

### Try It Yourself

```bash
# Create a practice branch
git checkout -b rebase-practice

# Make several small commits
echo "Line 1" >> test.txt && git add . && git commit -m "Add line 1"
echo "Line 2" >> test.txt && git add . && git commit -m "Add line 2"
echo "Line 3" >> test.txt && git add . && git commit -m "Add line 3"

# Squash last 3 commits into one
git rebase -i HEAD~3
# Change the last 2 picks to 'squash'
```

### Rebase vs Merge: When to Use Which?

**Use Rebase When:**
- Updating a feature branch with main
- Cleaning up local commits before pushing
- Maintaining a linear project history
- Working on a private branch

**Use Merge When:**
- Integrating completed features into main
- Working on public/shared branches
- Preserving exact history of changes
- Collaborating with multiple developers

**Golden Rule:** Never rebase commits that have been pushed to a public/shared branch!

---

## Cherry-Picking Commits

Cherry-pick allows you to apply specific commits from one branch to another.

### Basic Cherry-Pick

```bash
# Apply a specific commit to current branch
git cherry-pick abc123

# Cherry-pick multiple commits
git cherry-pick abc123 def456 ghi789

# Cherry-pick a range of commits
git cherry-pick abc123..xyz789
```

### Resolving Cherry-Pick Conflicts

```bash
# If conflicts occur during cherry-pick
git status  # See conflicting files
# Resolve conflicts manually
git add <resolved-files>
git cherry-pick --continue

# Abort cherry-pick
git cherry-pick --abort
```

### Try It Yourself

```bash
# Create two branches
git checkout -b feature-1
echo "Feature 1" >> feature1.txt && git add . && git commit -m "Add feature 1"

git checkout -b feature-2 main
echo "Feature 2" >> feature2.txt && git add . && git commit -m "Add feature 2"

# Cherry-pick commit from feature-1 to feature-2
git log feature-1  # Copy commit hash
git cherry-pick <commit-hash>
```

### Common Use Cases

- **Hotfix to Multiple Branches:** Apply same bug fix to multiple release branches
- **Selective Feature Integration:** Pull specific improvements without merging entire branch
- **Mistake Recovery:** Move commit from wrong branch to correct one

---

## Git Stash

Stash temporarily shelves changes so you can work on something else, then come back and restore them.

### Basic Stash Operations

```bash
# Stash current changes
git stash

# Stash with a descriptive message
git stash save "WIP: refactoring user module"

# List all stashes
git stash list

# Apply most recent stash (keeps it in stash list)
git stash apply

# Apply and remove most recent stash
git stash pop

# Apply specific stash
git stash apply stash@{2}

# Remove specific stash
git stash drop stash@{1}

# Clear all stashes
git stash clear
```

### Advanced Stash

```bash
# Stash including untracked files
git stash -u

# Stash including ignored files
git stash -a

# Create branch from stash
git stash branch new-feature-branch

# Show stash contents
git stash show
git stash show -p  # Show full diff
```

### Try It Yourself

```bash
# Make some changes
echo "Work in progress" >> file.txt
git add file.txt

# Urgent bug fix needed!
git stash save "WIP: updating file"

# Work on bug fix
git checkout -b bugfix
# ... make fixes ...

# Return to original work
git checkout original-branch
git stash pop
```

### Common Scenarios

- **Context Switching:** Quick branch switches without committing half-done work
- **Pulling Updates:** Stash local changes, pull remote updates, reapply changes
- **Experimenting:** Try something risky without committing

---

## Reset vs Revert vs Checkout

Understanding the differences between these commands is crucial for managing your repository.

### Visual Comparison

```
                Working Directory    Staging Area    Commit History
git reset --soft         ✓                ✓               ✗
git reset --mixed        ✓                ✗               ✗
git reset --hard         ✗                ✗               ✗
git revert               ✓                ✓               ✓ (new commit)
git checkout             ✗                ✗               ✓ (no change)
```

### Git Reset

Moves the branch pointer to a different commit.

```bash
# Soft reset: Keep changes staged
git reset --soft HEAD~1

# Mixed reset (default): Keep changes unstaged
git reset HEAD~1
git reset --mixed HEAD~1

# Hard reset: Discard all changes (DANGEROUS!)
git reset --hard HEAD~1

# Reset specific file
git reset HEAD file.txt
```

**Use Cases:**
- `--soft`: Undo commit but keep changes for re-committing
- `--mixed`: Undo commit and unstage changes
- `--hard`: Completely undo commits and changes (use with caution!)

### Git Revert

Creates a new commit that undoes changes from a previous commit.

```bash
# Revert specific commit
git revert abc123

# Revert without auto-commit
git revert -n abc123

# Revert merge commit
git revert -m 1 abc123
```

**Use Cases:**
- Undo commits on public/shared branches
- Maintain complete history
- Safe undo that doesn't rewrite history

### Git Checkout

Switches branches or restores files.

```bash
# Checkout specific file from HEAD
git checkout -- file.txt

# Checkout file from specific commit
git checkout abc123 -- file.txt

# Checkout branch
git checkout branch-name
```

### Try It Yourself

```bash
# Practice reset
git checkout -b reset-practice
echo "A" >> test.txt && git add . && git commit -m "A"
echo "B" >> test.txt && git add . && git commit -m "B"
echo "C" >> test.txt && git add . && git commit -m "C"

# Soft reset - commits gone, changes staged
git reset --soft HEAD~2
git status

# Practice revert
git checkout -b revert-practice
echo "Bad change" >> test.txt && git add . && git commit -m "Bad commit"
git revert HEAD  # Creates new commit undoing the change
```

### Decision Guide

**Use Reset When:**
- Working on local/private branches
- Want to remove commits entirely
- Consolidating multiple local commits

**Use Revert When:**
- Working on public/shared branches
- Need to maintain history
- Undoing merged changes

**Use Checkout When:**
- Restoring specific files
- Switching branches
- Not modifying commit history

---

## Git Reflog

Reflog is your safety net - it records every change to HEAD, allowing you to recover "lost" commits.

### Understanding Reflog

```bash
# View reflog
git reflog

# View reflog for specific branch
git reflog show branch-name

# Reflog with dates
git reflog --date=relative
```

### Recovering Lost Commits

```bash
# Scenario: Accidentally hard reset
git reset --hard HEAD~3  # Oops!

# Find the lost commit
git reflog
# Look for the commit before the reset

# Recover it
git reset --hard HEAD@{1}
# or
git reset --hard abc123
```

### Try It Yourself

```bash
# Create commits
git checkout -b reflog-practice
echo "Important work" >> important.txt && git add . && git commit -m "Important"
git log --oneline  # Note the commit hash

# "Lose" the commit
git reset --hard HEAD~1
git log --oneline  # Commit appears gone!

# Recover it with reflog
git reflog
git reset --hard HEAD@{1}  # Or use the commit hash
git log --oneline  # Your commit is back!
```

### Common Recovery Scenarios

```bash
# Recover deleted branch
git reflog
git checkout -b recovered-branch abc123

# Recover from bad rebase
git reflog
git reset --hard HEAD@{5}  # Before the rebase

# Find when branch was created
git reflog show branch-name
```

### Important Notes

- Reflog is local only (not shared with remotes)
- Entries expire after 90 days by default
- Can't recover uncommitted changes
- Reflog saved you from `git reset --hard`!

---

## Git Bisect

Bisect uses binary search to find which commit introduced a bug.

### Basic Bisect Workflow

```bash
# Start bisect
git bisect start

# Mark current commit as bad
git bisect bad

# Mark a known good commit
git bisect good abc123

# Git will checkout middle commit
# Test if bug exists

# Mark as good or bad
git bisect good   # Bug not present
git bisect bad    # Bug present

# Repeat until Git identifies the culprit commit

# End bisect session
git bisect reset
```

### Automated Bisect

```bash
# Run automated test script
git bisect start HEAD abc123
git bisect run ./test.sh

# test.sh should exit with:
# 0 = good commit
# 1-127 (except 125) = bad commit
# 125 = skip this commit
```

### Try It Yourself

```bash
# Create a history with a bug
git checkout -b bisect-practice
for i in {1..10}; do
  echo "Commit $i" >> file.txt
  git add . && git commit -m "Commit $i"
  # Introduce bug at commit 7
  if [ $i -eq 7 ]; then
    echo "BUG" >> file.txt
    git add . && git commit --amend --no-edit
  fi
done

# Find the bug
git bisect start
git bisect bad HEAD
git bisect good HEAD~10
# Check for bug at each step with: grep "BUG" file.txt
# Mark with git bisect good/bad
```

### Bisect with Automated Tests

```bash
# Example test script (test.sh)
#!/bin/bash
make test
exit $?

# Run bisect
git bisect start HEAD v1.0
git bisect run ./test.sh
```

### Best Practices

- Have a reliable test case
- Use automated testing when possible
- Mark commits carefully
- Use `git bisect skip` for broken commits
- Bisect works best with small, focused commits

---

## Submodules and Subtrees

Manage external dependencies and shared code across repositories.

### Git Submodules

Submodules allow you to keep a Git repository as a subdirectory of another Git repository.

#### Adding a Submodule

```bash
# Add submodule
git submodule add https://github.com/user/repo.git path/to/submodule

# Add submodule to specific branch
git submodule add -b develop https://github.com/user/repo.git libs/mylib

# Commit the submodule
git commit -m "Add submodule"
```

#### Working with Submodules

```bash
# Clone repo with submodules
git clone --recursive https://github.com/user/main-repo.git

# Or after cloning
git submodule init
git submodule update

# Update submodules to latest
git submodule update --remote

# Update specific submodule
git submodule update --remote path/to/submodule

# Execute command in all submodules
git submodule foreach 'git pull origin main'
```

#### Removing a Submodule

```bash
# Remove submodule
git submodule deinit path/to/submodule
git rm path/to/submodule
rm -rf .git/modules/path/to/submodule
git commit -m "Remove submodule"
```

### Git Subtrees

Subtrees embed external repository contents directly into your repo.

#### Adding a Subtree

```bash
# Add remote
git remote add library-remote https://github.com/user/library.git

# Add subtree
git subtree add --prefix=libs/library library-remote main --squash

# The --squash flag keeps your history clean
```

#### Working with Subtrees

```bash
# Pull updates from subtree
git subtree pull --prefix=libs/library library-remote main --squash

# Push changes back to subtree
git subtree push --prefix=libs/library library-remote main

# Split out subtree into separate branch
git subtree split --prefix=libs/library -b library-only
```

### Submodules vs Subtrees

| Feature | Submodules | Subtrees |
|---------|-----------|----------|
| Complexity | More complex | Simpler |
| History | Separate | Merged |
| Clone | Requires --recursive | Normal clone |
| File size | Small (references) | Larger (full copy) |
| Updates | Manual | Manual |
| Best for | External dependencies | Vendoring libraries |

### Try It Yourself: Submodules

```bash
# Create main project
mkdir main-project && cd main-project
git init

# Create and add submodule (using a public repo)
git submodule add https://github.com/octocat/Hello-World.git external/hello-world

# View submodule status
git submodule status

# Update submodule
cd external/hello-world
git pull origin main
cd ../..
git add external/hello-world
git commit -m "Update submodule"
```

### Try It Yourself: Subtrees

```bash
# Create main project
mkdir subtree-project && cd subtree-project
git init

# Add subtree
git remote add hello-remote https://github.com/octocat/Hello-World.git
git fetch hello-remote
git subtree add --prefix=vendor/hello hello-remote main --squash

# Pull updates
git subtree pull --prefix=vendor/hello hello-remote main --squash
```

### Best Practices

**Submodules:**
- Pin to specific commits for stability
- Document update procedures
- Use automation for updates
- Consider using sparse checkout

**Subtrees:**
- Use --squash to keep history clean
- Document original repository
- Be careful with push operations
- Consider for vendoring dependencies

---

## Summary

These advanced techniques give you powerful control over your Git workflow:

- **Rebase**: Clean, linear history
- **Cherry-pick**: Selective commit application
- **Stash**: Temporary change storage
- **Reset/Revert**: Undo changes safely
- **Reflog**: Recover lost commits
- **Bisect**: Find bugs efficiently
- **Submodules/Subtrees**: Manage dependencies

Master these tools, but always remember:
- With great power comes great responsibility
- Never rewrite public history
- Always have backups (reflog is your friend)
- Test in a safe branch first
- When in doubt, ask for help!

## Next Steps

- Practice these techniques in a test repository
- Try the exercises marked "Try It Yourself"
- Review [Troubleshooting Guide](troubleshooting-guide.md) for common issues
- Explore [Real-World Scenarios](real-world-scenarios.md) for practical applications
