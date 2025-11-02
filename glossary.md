# Git Glossary

A comprehensive reference of Git terminology and concepts.

## A

### Branch
A parallel version of your repository. Branches allow you to work on different features or fixes independently without affecting the main codebase.

```bash
git branch feature-login
git checkout feature-login
```

### Bare Repository
A repository without a working directory, typically used as a central repository on servers.

```bash
git init --bare repo.git
```

### Bisect
A binary search tool to find which commit introduced a bug.

```bash
git bisect start
git bisect bad HEAD
git bisect good v1.0
```

### Blob
Git's representation of file content. Each version of a file is stored as a blob object.

---

## C

### Clone
Creating a copy of a remote repository on your local machine, including all history and branches.

```bash
git clone https://github.com/user/repo.git
```

### Commit
A snapshot of your repository at a specific point in time. Contains changes, author, timestamp, and message.

```bash
git commit -m "Add user authentication"
```

### Checkout
Switching between branches or restoring files from commits.

```bash
git checkout main
git checkout -b new-branch
git checkout -- file.txt
```

### Cherry-pick
Applying a specific commit from one branch to another.

```bash
git cherry-pick abc123
```

### Conflict
Occurs when Git cannot automatically merge changes because the same lines were modified in different branches.

```
<<<<<<< HEAD
Your changes
=======
Their changes
>>>>>>> branch-name
```

### Commit Hash (SHA)
Unique 40-character identifier for each commit.

```bash
# Full hash
abc123def456789012345678901234567890abcd

# Short hash (first 7 characters)
abc123d
```

---

## D

### Detached HEAD
State where HEAD points directly to a commit instead of a branch.

```bash
git checkout abc123  # Causes detached HEAD
git checkout -b new-branch  # Fix it
```

### Diff
Shows differences between commits, branches, or files.

```bash
git diff                    # Working directory vs staging
git diff --staged           # Staging vs last commit
git diff main feature       # Between branches
git diff abc123 def456      # Between commits
```

### Distributed Version Control
System where every contributor has a complete copy of the repository history. Opposite of centralized systems like SVN.

---

## F

### Fetch
Downloads changes from remote repository without merging them.

```bash
git fetch origin
git fetch origin main
```

### Fast-forward
A merge where Git simply moves the branch pointer forward because there are no divergent changes.

```bash
# Before
main:    A---B
              \
feature:       C---D

# After fast-forward merge
main:    A---B---C---D
```

### Fork
Creating a personal copy of someone else's repository on hosting services like GitHub. Used for contributing to open-source projects.

---

## G

### Gitignore
File (`.gitignore`) specifying which files Git should not track.

```gitignore
# Example .gitignore
node_modules/
*.log
.env
```

### GUI (Git GUI)
Graphical interface for Git operations. Examples: GitKraken, SourceTree, GitHub Desktop.

---

## H

### HEAD
Pointer to the current branch/commit you're working on.

```bash
git log HEAD      # Current commit
git reset HEAD~1  # Previous commit
git show HEAD     # Show current commit
```

### Hash
See Commit Hash (SHA)

### Hook
Script that runs automatically at certain Git events (pre-commit, post-commit, etc.).

```bash
# Example: .git/hooks/pre-commit
#!/bin/bash
npm test || exit 1
```

### Hotfix
Quick fix for a production issue, usually branched from main.

```bash
git checkout -b hotfix/critical-bug main
```

---

## I

### Index
See Staging Area

### Init
Initialize a new Git repository.

```bash
git init
git init --bare  # Server repository
```

---

## L

### Log
View commit history.

```bash
git log
git log --oneline
git log --graph --all
```

### LFS (Large File Storage)
Extension for storing large binary files outside the Git repository.

```bash
git lfs install
git lfs track "*.psd"
```

---

## M

### Main (Master)
The default primary branch. Historically called `master`, now commonly `main`.

```bash
git checkout main
```

### Merge
Combining changes from different branches.

```bash
git merge feature-branch
```

### Merge Commit
Special commit with two parent commits, created when merging branches (unless fast-forward).

```
     A---B---C main
          \   \
           D---E feature
                \
                 M (merge commit)
```

### Merge Conflict
See Conflict

---

## O

### Origin
Default name for the primary remote repository.

```bash
git remote -v
# origin  https://github.com/user/repo.git
```

---

## P

### Pull
Fetch changes from remote and merge them into current branch.

```bash
git pull origin main
# Same as:
# git fetch origin
# git merge origin/main
```

### Push
Upload local commits to remote repository.

```bash
git push origin main
git push -u origin feature-branch
```

### Pull Request (PR)
Request to merge changes from one branch to another, with code review. Called "Merge Request" in GitLab.

### Patch
Text file containing changes that can be applied to a repository.

```bash
git format-patch HEAD~3  # Create patches for last 3 commits
git apply patch.diff     # Apply patch
```

---

## R

### Rebase
Moving or combining commits to a new base commit. Creates a linear history.

```bash
git rebase main
git rebase -i HEAD~3  # Interactive rebase
```

### Reflog
Reference log tracking all changes to HEAD, including "deleted" commits.

```bash
git reflog
git reset --hard HEAD@{2}
```

### Remote
Version of your repository hosted elsewhere (GitHub, GitLab, etc.).

```bash
git remote add origin https://github.com/user/repo.git
git remote -v
```

### Repository (Repo)
Directory containing your project and its entire version history.

### Reset
Move branch pointer to different commit, potentially changing working directory.

```bash
git reset --soft HEAD~1   # Keep changes staged
git reset HEAD~1          # Keep changes unstaged
git reset --hard HEAD~1   # Discard changes
```

### Revert
Create new commit that undoes changes from a previous commit.

```bash
git revert abc123
```

---

## S

### SHA
See Commit Hash

### Stash
Temporarily save uncommitted changes without committing.

```bash
git stash
git stash save "WIP: feature work"
git stash list
git stash pop
git stash apply
```

### Staging Area (Index)
Intermediate area where changes are prepared before committing.

```bash
git add file.txt     # Add to staging
git reset HEAD file.txt  # Remove from staging
git diff --staged    # View staged changes
```

### Submodule
Git repository embedded within another repository.

```bash
git submodule add https://github.com/user/lib.git libs/lib
git submodule update --init --recursive
```

### Subtree
Alternative to submodules, merging external repository directly into yours.

```bash
git subtree add --prefix=libs/lib https://github.com/user/lib.git main
```

---

## T

### Tag
Named reference to a specific commit, typically used for releases.

```bash
git tag v1.0.0
git tag -a v1.0.0 -m "Version 1.0.0 release"
git push origin v1.0.0
git push origin --tags
```

### Tracking Branch
Local branch linked to a remote branch.

```bash
git push -u origin feature-branch  # Set upstream tracking
git branch -vv  # View tracking branches
```

### Tree
Git's representation of directory structure.

---

## U

### Untracked Files
Files in working directory that Git is not tracking.

```bash
git status  # Shows untracked files
git add file.txt  # Start tracking
```

### Upstream
The main repository you forked from, or the remote branch your local branch tracks.

```bash
git remote add upstream https://github.com/original/repo.git
git fetch upstream
git merge upstream/main
```

---

## W

### Working Directory (Working Tree)
Your actual project files (as opposed to staged or committed files).

```bash
# States of a file
Working Directory → (git add) → Staging Area → (git commit) → Repository
```

### Worktree
Multiple working directories for the same repository.

```bash
git worktree add ../feature-work feature-branch
git worktree list
git worktree remove ../feature-work
```

---

## Common Git Commands Quick Reference

### Basic Commands
```bash
git init                # Initialize repository
git clone <url>         # Clone repository
git status              # Check status
git add <file>          # Stage changes
git commit -m "msg"     # Commit changes
git push                # Push to remote
git pull                # Pull from remote
```

### Branching
```bash
git branch              # List branches
git branch <name>       # Create branch
git checkout <name>     # Switch branch
git checkout -b <name>  # Create and switch
git merge <branch>      # Merge branch
git branch -d <name>    # Delete branch
```

### Inspection
```bash
git log                 # View history
git log --oneline       # Compact history
git diff                # View changes
git show <commit>       # Show commit details
git blame <file>        # Show who changed what
```

### Undoing
```bash
git reset HEAD <file>   # Unstage file
git checkout -- <file>  # Discard changes
git revert <commit>     # Revert commit
git reset --hard <c>    # Reset to commit
```

### Remote
```bash
git remote -v           # List remotes
git remote add <n> <u>  # Add remote
git fetch <remote>      # Fetch changes
git push <r> <b>        # Push branch
git pull <r> <b>        # Pull branch
```

---

## Git Object Types

Git stores data as four types of objects:

1. **Blob**: File content
2. **Tree**: Directory structure
3. **Commit**: Snapshot with metadata
4. **Tag**: Named reference to commit

```bash
# View object type
git cat-file -t abc123

# View object content
git cat-file -p abc123
```

---

## Git Workflow States

```
┌─────────────────┐
│ Remote Repo     │
│ (GitHub/GitLab) │
└────────┬────────┘
         │
    push │ │ pull/fetch
         │ │
┌────────▼────────┐
│ Local Repo      │
│ (.git dir)      │
└────────┬────────┘
         │
  commit │ │ checkout
         │ │
┌────────▼────────┐
│ Staging Area    │
│ (Index)         │
└────────┬────────┘
         │
     add │ │ reset
         │ │
┌────────▼────────┐
│ Working Dir     │
│ (Your files)    │
└─────────────────┘
```

---

## File States in Git

1. **Untracked**: New file Git doesn't know about
2. **Unmodified**: File unchanged since last commit
3. **Modified**: File changed but not staged
4. **Staged**: File marked for next commit

```bash
# Untracked → Staged
git add new-file.txt

# Modified → Staged
git add modified-file.txt

# Staged → Committed
git commit -m "Add files"
```

---

## Git Configuration Levels

1. **System**: Applies to all users (`/etc/gitconfig`)
2. **Global**: Applies to current user (`~/.gitconfig`)
3. **Local**: Applies to current repo (`.git/config`)

```bash
git config --system <key> <value>
git config --global <key> <value>
git config --local <key> <value>

# View all config
git config --list --show-origin
```

---

## Special References

- `HEAD`: Current commit/branch
- `HEAD~1`: Parent of HEAD (same as `HEAD^`)
- `HEAD~2`: Grandparent of HEAD
- `HEAD^2`: Second parent of merge commit
- `@`: Shortcut for HEAD
- `@{-1}`: Previous branch
- `@{u}`: Upstream branch

```bash
git show HEAD~1
git diff HEAD~2
git checkout @{-1}  # Go to previous branch
```

---

## See Also

- [FAQ](faq.md) - Frequently asked questions
- [Git Documentation](https://git-scm.com/docs) - Official Git reference
- [Pro Git Book](https://git-scm.com/book) - Free comprehensive guide
- [Git Glossary (Official)](https://git-scm.com/docs/gitglossary) - Official glossary

---

## Contributing

Find a term that should be added? See a definition that could be clearer? Please contribute!

1. Fork this repository
2. Add/update definitions
3. Submit a pull request

See [Contributing Guidelines](contributing.md) for more information.
