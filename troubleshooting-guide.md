# Git Troubleshooting Guide

Stuck with a Git problem? This guide covers common issues and their solutions.

## Table of Contents
1. [Oops, I Committed to the Wrong Branch](#oops-i-committed-to-the-wrong-branch)
2. [How to Undo My Last Commit](#how-to-undo-my-last-commit)
3. [I Accidentally Pushed Sensitive Data](#i-accidentally-pushed-sensitive-data)
4. [My Branch is Behind Main](#my-branch-is-behind-main)
5. [Force Push Dangers and When It's Acceptable](#force-push-dangers-and-when-its-acceptable)
6. [Merge Conflicts Won't Resolve](#merge-conflicts-wont-resolve)
7. [Detached HEAD State](#detached-head-state)
8. [Lost Commits After Reset](#lost-commits-after-reset)
9. [Push Rejected (Non-Fast-Forward)](#push-rejected-non-fast-forward)
10. [Authentication Failures](#authentication-failures)

---

## Oops, I Committed to the Wrong Branch

One of the most common mistakes. Here are several solutions depending on your situation.

### Scenario 1: Commit on Main Instead of Feature Branch

**Problem**: You committed directly to `main` but meant to commit to a feature branch.

**Solution**:

```bash
# Check current status
git log --oneline -n 3

# Method 1: Move commit to new branch
git branch feature-branch          # Create new branch at current commit
git reset --hard HEAD~1            # Move main back one commit
git checkout feature-branch        # Switch to feature branch
# Your commit is now on feature-branch!

# Method 2: If branch already exists
git checkout feature-branch        # Switch to existing branch
git cherry-pick main               # Copy commit from main
git checkout main                  # Go back to main
git reset --hard HEAD~1            # Remove commit from main
```

### Scenario 2: Multiple Commits on Wrong Branch

```bash
# If you made 3 commits on wrong branch
git log --oneline -n 5  # Verify which commits to move

# Create new branch with these commits
git branch correct-branch

# Reset current branch (removes last 3 commits)
git reset --hard HEAD~3

# Switch to new branch
git checkout correct-branch
```

### Scenario 3: Already Pushed to Remote

```bash
# If you've already pushed to main (and others haven't pulled)
git checkout main
git reset --hard HEAD~1
git push origin main --force-with-lease  # Use with caution!

# Create feature branch with the commit
git checkout -b feature-branch HEAD@{1}
git push origin feature-branch
```

### Try It Yourself

```bash
# Create test scenario
git checkout main
echo "wrong branch" >> file.txt
git add . && git commit -m "Oops, wrong branch"

# Fix it
git branch correct-branch
git reset --hard HEAD~1
git checkout correct-branch
git log --oneline  # Verify commit is here
```

---

## How to Undo My Last Commit

Different scenarios require different approaches.

### Scenario 1: Undo Commit, Keep Changes (Most Common)

```bash
# Keep changes staged
git reset --soft HEAD~1

# Keep changes unstaged
git reset HEAD~1
# or
git reset --mixed HEAD~1

# Now you can modify and re-commit
```

### Scenario 2: Undo Commit, Discard Changes

```bash
# WARNING: This permanently deletes your changes!
git reset --hard HEAD~1
```

### Scenario 3: Undo Commit That Was Already Pushed

```bash
# Create a new commit that undoes the changes
git revert HEAD

# This is safe for shared branches!
```

### Scenario 4: Fix Commit Message Only

```bash
# If you haven't pushed yet
git commit --amend -m "Corrected commit message"

# If you've already pushed (requires force push)
git commit --amend -m "Corrected commit message"
git push --force-with-lease
```

### Scenario 5: Add More Changes to Last Commit

```bash
# Make additional changes
echo "more changes" >> file.txt
git add file.txt

# Amend the last commit
git commit --amend --no-edit
```

### Visual Decision Tree

```
Need to undo last commit?
│
├─ Already pushed to shared branch?
│  └─ YES → Use git revert HEAD
│  └─ NO → Continue below
│
├─ Want to keep the changes?
│  ├─ YES, keep staged → git reset --soft HEAD~1
│  ├─ YES, keep unstaged → git reset HEAD~1
│  └─ NO, discard all → git reset --hard HEAD~1
│
└─ Just fix commit message?
   └─ git commit --amend -m "New message"
```

### Try It Yourself

```bash
# Practice soft reset
echo "test" >> file.txt
git add . && git commit -m "Test commit"
git reset --soft HEAD~1
git status  # Changes are still staged

# Practice hard reset
git commit -m "Test commit again"
git reset --hard HEAD~1
git status  # Changes are gone
```

---

## I Accidentally Pushed Sensitive Data

**CRITICAL**: Act fast! Assume the data is compromised immediately.

### Immediate Actions (Do These First!)

```bash
# 1. ROTATE CREDENTIALS IMMEDIATELY
# - Change passwords
# - Regenerate API keys
# - Revoke tokens
# - Update all systems using these credentials

# 2. Notify relevant parties
# - Security team
# - Team members
# - Affected services
```

### Remove from Git History

#### Method 1: Using BFG Repo-Cleaner (Fastest)

```bash
# Clone fresh copy
git clone --mirror https://github.com/user/repo.git

# Remove passwords file
bfg --delete-files secrets.env repo.git

# Or replace text patterns
bfg --replace-text passwords.txt repo.git
# passwords.txt contains:
# API_KEY=abc123 ==> API_KEY=REMOVED
# password=secret ==> password=REMOVED

# Clean up
cd repo.git
git reflog expire --expire=now --all
git gc --prune=now --aggressive

# Force push
git push
```

#### Method 2: Using git-filter-repo

```bash
# Install
pip install git-filter-repo

# Remove specific file from all history
git filter-repo --invert-paths --path config/secrets.yml

# Remove by pattern
git filter-repo --path-glob '*.pem' --invert-paths

# Replace sensitive strings
git filter-repo --replace-text <(echo "
api_key=sk-1234===>api_key=REDACTED
password=secret===>password=REDACTED
")

# Force push all branches and tags
git push origin --force --all
git push origin --force --tags
```

#### Method 3: Using git filter-branch (Legacy)

```bash
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch path/to/secret.txt" \
  --prune-empty --tag-name-filter cat -- --all

git reflog expire --expire=now --all
git gc --prune=now --aggressive
git push origin --force --all
git push origin --force --tags
```

### After Removal Steps

```bash
# 1. Verify removal
git log --all --full-history -- path/to/secret.txt
# Should return nothing

# 2. All team members must:
rm -rf repo
git clone https://github.com/user/repo.git
# Or rebase their work on new history

# 3. Update .gitignore
echo "secrets.env" >> .gitignore
git add .gitignore
git commit -m "Update gitignore to prevent future leaks"

# 4. Set up pre-commit hooks to prevent recurrence
# See security-best-practices.md
```

### Prevention for Next Time

```bash
# Install secret detection
pip install detect-secrets
detect-secrets scan --all-files > .secrets.baseline

# Install pre-commit hooks
cat << 'EOF' > .pre-commit-config.yaml
repos:
  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets
        args: ['--baseline', '.secrets.baseline']
EOF

pre-commit install
```

---

## My Branch is Behind Main

Your feature branch is missing commits that were added to main. Here's how to update it.

### Scenario 1: 10-50 Commits Behind (Simple Update)

```bash
# Method 1: Merge (preserves branch history)
git checkout feature-branch
git merge main

# Resolve any conflicts
git add .
git commit

# Method 2: Rebase (cleaner history)
git checkout feature-branch
git rebase main

# If conflicts occur:
# - Resolve conflicts in files
git add .
git rebase --continue
# Or abort: git rebase --abort
```

### Scenario 2: 50+ Commits Behind

```bash
# Check how far behind
git checkout feature-branch
git log main..feature-branch --oneline  # Your commits
git log feature-branch..main --oneline  # Missing commits

# Option 1: Interactive rebase (review each commit)
git rebase -i main

# Option 2: Merge (safer if many conflicts expected)
git merge main

# Option 3: Fresh start (if feature branch is outdated)
git checkout main
git pull
git checkout -b feature-branch-v2
git cherry-pick <commit1> <commit2> <commit3>
```

### Scenario 3: Already Pushed Feature Branch

```bash
# Update local branch
git checkout feature-branch
git rebase main

# Update remote (requires force push)
git push --force-with-lease origin feature-branch

# WARNING: Notify team members to reset their local copies!
# They should:
git checkout main
git branch -D feature-branch
git fetch origin
git checkout feature-branch
```

### Keeping Feature Branch Updated

```bash
# Best practice: Update frequently
git checkout feature-branch
git fetch origin
git rebase origin/main

# Or use merge
git merge origin/main

# Automate with git hooks or CI/CD
```

### Visual Guide

```
Before (50 commits behind):
main:     A---B---C---D---E---F---G---...---Z
                   \
feature:            H---I---J

After rebase:
main:     A---B---C---D---E---F---G---...---Z
                                             \
feature:                                      H'---I'---J'

After merge:
main:     A---B---C---D---E---F---G---...---Z
                   \                         \
feature:            H---I---J-----------------M
```

### Try It Yourself

```bash
# Create diverged branches
git checkout -b main-simulate
echo "main1" >> file.txt && git add . && git commit -m "Main 1"
echo "main2" >> file.txt && git add . && git commit -m "Main 2"

git checkout HEAD~2
git checkout -b feature-simulate
echo "feature1" >> feature.txt && git add . && git commit -m "Feature 1"

# Now feature is 2 commits behind
git log --oneline --graph --all

# Update feature branch
git rebase main-simulate
```

---

## Force Push Dangers and When It's Acceptable

Force push rewrites history and can cause serious problems if misused.

### Why Force Push is Dangerous

```bash
# Alice's timeline
A---B---C---D  (main)

# Bob's timeline (after Alice force pushes)
A---B---E---F  (main)
    |
    C---D  (Bob's local copy - now orphaned!)
```

Bob's commits C and D are now orphaned and he'll face conflicts.

### When Force Push is ACCEPTABLE

1. **On Your Personal/Private Branches**
   ```bash
   git push --force origin my-personal-feature-branch
   ```

2. **Cleaning Up Before PR Review** (if no one else is working on it)
   ```bash
   # Clean up commit history
   git rebase -i main
   git push --force-with-lease origin feature-branch
   ```

3. **Removing Sensitive Data** (emergency!)
   ```bash
   # After using BFG or filter-repo
   git push --force --all
   ```

4. **Fixing Broken Commits** (coordinated with team)
   ```bash
   # Team agrees to reset
   git push --force-with-lease origin branch-name
   ```

### When Force Push is DANGEROUS

1. **Never on Main/Master/Production branches**
2. **Never on shared feature branches** (without coordination)
3. **Never without checking with team first**
4. **Never when others have based work on your commits**

### Safe Force Push: --force-with-lease

```bash
# Bad: Overwrites remote regardless of state
git push --force

# Good: Only pushes if remote hasn't changed
git push --force-with-lease

# Best: Specify expected remote state
git push --force-with-lease origin feature-branch:refs/heads/feature-branch
```

### Recovering from Force Push

```bash
# If someone force pushed and broke your branch

# Method 1: Find the lost commits with reflog
git reflog
# Find commit before force push
git reset --hard abc123

# Method 2: Check if GitHub/GitLab still has it
# Go to web UI → branch → history → find old commit
git reset --hard <old-commit-hash>

# Method 3: Ask team member who has old version
git remote add coworker https://github.com/coworker/repo.git
git fetch coworker
git cherry-pick <their-commits>
```

### Alternatives to Force Push

```bash
# Instead of force push, create new commits
git revert HEAD~3..HEAD

# Or create a new branch
git branch backup-branch
git checkout main
git reset --hard origin/main
git checkout -b new-feature-branch
git cherry-pick backup-branch
```

### Force Push Checklist

Before force pushing, ask yourself:
- [ ] Is this a personal/private branch?
- [ ] Have I coordinated with my team?
- [ ] Will anyone else's work be affected?
- [ ] Have I tried `--force-with-lease` instead of `--force`?
- [ ] Is there a safer alternative?
- [ ] Do I have a backup plan?

### Try It Yourself (Safe Practice)

```bash
# Create test repo
mkdir force-push-test && cd force-push-test
git init

# Create commits
echo "A" >> file.txt && git add . && git commit -m "A"
echo "B" >> file.txt && git add . && git commit -m "B"
echo "C" >> file.txt && git add . && git commit -m "C"

# Simulate force push scenario
git reset --hard HEAD~2
git reflog  # See what happened

# Practice recovery
git reset --hard HEAD@{1}
```

---

## Merge Conflicts Won't Resolve

Stuck in merge/rebase conflict hell? Here's how to escape.

### Understanding Conflict Markers

```
<<<<<<< HEAD (current branch)
This is your current code
=======
This is the incoming code
>>>>>>> feature-branch (incoming branch)
```

### Step-by-Step Resolution

```bash
# 1. See all conflicting files
git status

# 2. Open each file and look for <<<<<<< markers

# 3. Resolve conflicts:
#    - Keep yours: remove incoming code and markers
#    - Keep theirs: remove your code and markers
#    - Keep both: combine and remove markers
#    - Rewrite: create new solution and remove markers

# 4. Mark as resolved
git add conflicted-file.txt

# 5. Continue merge/rebase
git merge --continue
# or
git rebase --continue
```

### Tools to Help

```bash
# Use merge tool
git mergetool

# See diff with both versions
git diff

# See just conflicted parts
git diff --name-only --diff-filter=U

# Checkout versions
git checkout --ours file.txt    # Keep your version
git checkout --theirs file.txt  # Keep their version
```

### Nuclear Options (When All Else Fails)

```bash
# Abort merge
git merge --abort

# Abort rebase
git rebase --abort

# Abort cherry-pick
git cherry-pick --abort

# Start fresh (DANGEROUS!)
git reset --hard HEAD
```

### Complex Merge Conflicts

```bash
# Use theirs for all conflicts
git merge -X theirs branch-name

# Use ours for all conflicts
git merge -X ours branch-name

# Use specific strategy for specific files
git checkout --ours path/to/file.txt
git checkout --theirs path/to/other-file.txt
git add path/to/file.txt path/to/other-file.txt
git merge --continue
```

### Prevention

```bash
# Keep branches updated frequently
git fetch origin
git rebase origin/main  # or merge

# Make smaller, focused commits
# Communicate with team about overlapping work
# Use feature flags instead of long-lived branches
```

---

## Detached HEAD State

### What is Detached HEAD?

You're not on any branch - you're directly on a commit.

```bash
# This causes detached HEAD
git checkout abc123  # Checking out specific commit
```

### How to Tell You're in Detached HEAD

```bash
git status
# Output: HEAD detached at abc123
```

### Fix: Create a Branch

```bash
# If you've made commits and want to keep them
git branch new-branch-name
git checkout new-branch-name

# Or in one command
git checkout -b new-branch-name
```

### Fix: Return to Branch

```bash
# If you haven't made commits
git checkout main

# If you made commits but don't want them
git checkout main  # Git will warn about losing commits
```

### Recover Lost Commits from Detached HEAD

```bash
# You left detached HEAD and lost commits
git reflog
# Find your lost commit
git checkout -b recovered-branch abc123
```

---

## Lost Commits After Reset

Accidentally did `git reset --hard`? Don't panic!

### Using Reflog

```bash
# View reflog
git reflog

# Find commit before reset
git reset --hard HEAD@{2}

# Or use commit hash
git reset --hard abc123

# Create branch at lost commit
git branch recovered abc123
```

### If Reflog Doesn't Help

```bash
# Check all dangling commits
git fsck --lost-found

# View dangling commits
git log --graph --oneline --decorate $(git fsck --no-reflogs | grep commit | cut -d ' ' -f 3)

# Recover specific commit
git cherry-pick <commit-hash>
```

---

## Push Rejected (Non-Fast-Forward)

### Error Message

```
! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs
hint: Updates were rejected because the tip of your current branch is behind
```

### Solution

```bash
# 1. Fetch remote changes
git fetch origin

# 2. Merge or rebase
git merge origin/main
# or
git rebase origin/main

# 3. Push
git push origin main
```

### If You're Sure You Want to Overwrite

```bash
# USE WITH EXTREME CAUTION!
git push --force-with-lease origin main
```

---

## Authentication Failures

### SSH Key Issues

```bash
# Test SSH connection
ssh -T git@github.com

# Generate new SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"

# Add to ssh-agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Add public key to GitHub/GitLab
cat ~/.ssh/id_ed25519.pub
```

### Token Authentication

```bash
# Use personal access token instead of password
git remote set-url origin https://TOKEN@github.com/user/repo.git

# Or configure credential helper
git config --global credential.helper store
```

### 403 Forbidden

```bash
# Check remote URL
git remote -v

# Update remote URL
git remote set-url origin git@github.com:user/repo.git
```

---

## Quick Reference: Emergency Commands

```bash
# Undo anything
git reflog  # Find commit to restore
git reset --hard abc123

# Abort current operation
git merge --abort
git rebase --abort
git cherry-pick --abort

# Unstage everything
git reset HEAD

# Discard all local changes
git restore .
# or
git checkout -- .

# Return to clean state (DANGEROUS!)
git reset --hard HEAD
git clean -fd

# See what's different
git diff
git diff --staged

# Check current state
git status
git log --oneline -10
```

---

## When All Else Fails

1. **Check reflog**: `git reflog` is your friend
2. **Ask for help**: Post on Stack Overflow with `git log`, `git status` output
3. **Clone fresh**: Sometimes starting over is faster
4. **Use GUI tools**: GitKraken, SourceTree, Fork can visualize issues
5. **Backup first**: `git branch backup` before trying risky operations

Remember: Git rarely loses data permanently. Most "disasters" are recoverable with reflog!
