# Git Quiz & Self-Assessment

Test your Git knowledge with these questions organized by skill level.

## How to Use This Quiz

1. Answer questions honestly (don't peek at answers!)
2. Check your answers at the end of each section
3. Review topics where you struggled
4. Retake quiz after studying

## Scoring Guide

- **Beginner Level**: 80%+ correct → Ready for Intermediate
- **Intermediate Level**: 80%+ correct → Ready for Advanced
- **Advanced Level**: 80%+ correct → Git Master!

---

## Beginner Level (Fundamentals)

### Question 1
What command initializes a new Git repository?

A) `git start`
B) `git init`
C) `git begin`
D) `git create`

### Question 2
How do you stage a file for commit?

A) `git stage file.txt`
B) `git add file.txt`
C) `git commit file.txt`
D) `git prepare file.txt`

### Question 3
What does `git status` show?

A) The current Git version
B) Server status
C) Changes in working directory and staging area
D) Network connectivity

### Question 4
How do you create a new branch?

A) `git new-branch feature`
B) `git branch feature`
C) `git create feature`
D) `git make-branch feature`

### Question 5
What's the difference between `git pull` and `git fetch`?

A) No difference, they're aliases
B) `pull` fetches and merges, `fetch` only downloads
C) `fetch` is faster than `pull`
D) `pull` works offline, `fetch` needs internet

### Question 6
How do you switch to an existing branch?

A) `git switch branch-name` or `git checkout branch-name`
B) `git change branch-name`
C) `git move branch-name`
D) `git goto branch-name`

### Question 7
What does `git clone` do?

A) Copies a single file
B) Creates a new repository
C) Downloads a complete repository copy
D) Synchronizes with remote

### Question 8
How do you view commit history?

A) `git history`
B) `git log`
C) `git show`
D) `git commits`

### Question 9
What is origin in Git?

A) The first commit
B) Default name for remote repository
C) The main branch
D) The repository owner

### Question 10
How do you undo changes to a file before committing?

A) `git undo file.txt`
B) `git revert file.txt`
C) `git restore file.txt` or `git checkout -- file.txt`
D) `git reset file.txt`

---

## Intermediate Level (Collaboration & Workflows)

### Question 11
When should you use `git rebase` instead of `git merge`?

A) Always use rebase
B) Never use rebase
C) Use rebase for updating feature branches with main; avoid on shared branches
D) Use rebase only on main branch

### Question 12
What does `git stash` do?

A) Permanently deletes changes
B) Temporarily saves uncommitted changes
C) Creates a new branch
D) Stages all changes

### Question 13
How do you delete a remote branch?

A) `git branch -d origin/branch-name`
B) `git delete branch-name`
C) `git push origin --delete branch-name`
D) `git remove origin branch-name`

### Question 14
What is a merge conflict?

A) An error in Git installation
B) When same lines are modified in different branches
C) When network fails during push
D) When branch names conflict

### Question 15
What's the purpose of `.gitignore`?

A) Hide commits from log
B) Specify files Git should not track
C) Block collaborators
D) Ignore merge conflicts

### Question 16
How do you create a pull request?

A) `git pull-request`
B) `git pr create`
C) Through GitHub/GitLab web interface
D) `git request pull`

### Question 17
What does `git cherry-pick` do?

A) Selects best commits automatically
B) Applies a specific commit from one branch to another
C) Removes bad commits
D) Optimizes repository

### Question 18
How do you rename a branch?

A) `git branch --rename old-name new-name`
B) `git branch -m old-name new-name`
C) `git rename old-name new-name`
D) `git branch --move old-name new-name`

### Question 19
What is HEAD in Git?

A) The repository owner
B) The first commit
C) Pointer to current branch/commit
D) The remote repository

### Question 20
How do you squash commits?

A) `git squash HEAD~3`
B) `git rebase -i HEAD~3` and mark commits as 'squash'
C) `git merge --squash`
D) `git commit --amend`

---

## Advanced Level (Power User)

### Question 21
What's the difference between `git reset --soft`, `--mixed`, and `--hard`?

A) They're the same
B) `--soft` keeps changes staged; `--mixed` unstages; `--hard` discards
C) Speed: soft is fastest, hard is slowest
D) `--soft` is safer than `--hard`

### Question 22
When is it acceptable to force push?

A) Whenever you want
B) Never
C) On personal branches, after rebasing, or cleaning up before review
D) Only on main branch

### Question 23
What is `git reflog` used for?

A) Viewing remote logs
B) Recovering "lost" commits by showing all HEAD changes
C) Logging into remote servers
D) Refreshing repository

### Question 24
How does `git bisect` work?

A) Splits repository in half
B) Binary search through commits to find bugs
C) Compares two branches
D) Creates two commits from one

### Question 25
What's the difference between submodules and subtrees?

A) No difference
B) Submodules link external repos; subtrees embed them
C) Subtrees are deprecated
D) Submodules are faster

### Question 26
What does `git revert` do?

A) Same as `git reset`
B) Creates new commit that undoes a previous commit
C) Deletes commits
D) Restores deleted files

### Question 27
How do you sign commits with GPG?

A) `git commit --sign`
B) `git commit -S` and configure GPG key
C) Automatically enabled
D) Only possible on GitHub

### Question 28
What is a detached HEAD state?

A) An error state
B) When HEAD points to commit instead of branch
C) When repository is corrupted
D) When remote is unreachable

### Question 29
How do you find which commit introduced a bug?

A) `git find-bug`
B) `git bisect`
C) `git blame`
D) Manual review only

### Question 30
What's the purpose of `git filter-repo`?

A) Filter log output
B) Rewrite history to remove sensitive data
C) Filter branches
D) Clean working directory

---

## Scenario-Based Questions

### Scenario 1
You committed to wrong branch. What do you do?

A) Delete the commit
B) Create new branch at current commit, then reset original branch
C) Force push to correct branch
D) Start over

### Scenario 2
Your PR has conflicts with main. How do you resolve?

A) Close PR and create new one
B) Update branch with main, resolve conflicts, push
C) Ask maintainer to resolve
D) Force push your branch

### Scenario 3
You accidentally pushed sensitive data. What's the first step?

A) Delete repository
B) Rotate/revoke compromised credentials immediately
C) Use git filter-branch
D) Make commits private

### Scenario 4
Your branch is 50 commits behind main. Best approach?

A) Delete branch and start over
B) `git rebase main` or `git merge main`
C) Cherry-pick all commits
D) Leave it behind

### Scenario 5
You need to work on urgent bug while having uncommitted changes. What do you do?

A) Commit incomplete work
B) `git stash`, fix bug, `git stash pop`
C) Delete changes
D) Create new repository

---

## True or False

### Question 31
True or False: You should always rebase instead of merge.

### Question 32
True or False: `git pull` is equivalent to `git fetch` + `git merge`.

### Question 33
True or False: Once you push a commit, you can never change it.

### Question 34
True or False: `.gitkeep` is a built-in Git feature.

### Question 35
True or False: Deleting a branch deletes its commits.

### Question 36
True or False: You can recover commits after `git reset --hard` using reflog.

### Question 37
True or False: Git stores complete copies of files in each commit.

### Question 38
True or False: You need internet connection to commit in Git.

### Question 39
True or False: Force pushing to shared branches is safe.

### Question 40
True or False: `git blame` is used to assign responsibility for bugs.

---

## Command Matching

Match the command to its purpose:

### Commands:
A) `git rebase -i HEAD~3`
B) `git reflog`
C) `git cherry-pick abc123`
D) `git bisect start`
E) `git stash pop`
F) `git reset --soft HEAD~1`

### Purposes:
1. Apply specific commit to current branch
2. Undo last commit but keep changes staged
3. Start binary search for buggy commit
4. View history of HEAD changes
5. Apply and remove most recent stash
6. Interactively edit last 3 commits

---

## Fill in the Blank

### Question 41
To create and switch to a new branch in one command: `git checkout ___ branch-name`

### Question 42
To see who last modified each line of a file: `git ___ file.txt`

### Question 43
To undo a commit safely on a shared branch: `git ___ commit-hash`

### Question 44
To show changes not yet staged: `git ___`

### Question 45
To list all branches including remote: `git branch ___`

---

## Answers

### Beginner Level Answers
1. B - `git init`
2. B - `git add file.txt`
3. C - Changes in working directory and staging area
4. B - `git branch feature`
5. B - `pull` fetches and merges, `fetch` only downloads
6. A - `git switch branch-name` or `git checkout branch-name`
7. C - Downloads a complete repository copy
8. B - `git log`
9. B - Default name for remote repository
10. C - `git restore file.txt` or `git checkout -- file.txt`

**Scoring**: ___ / 10

### Intermediate Level Answers
11. C - Use rebase for updating feature branches with main; avoid on shared branches
12. B - Temporarily saves uncommitted changes
13. C - `git push origin --delete branch-name`
14. B - When same lines are modified in different branches
15. B - Specify files Git should not track
16. C - Through GitHub/GitLab web interface
17. B - Applies a specific commit from one branch to another
18. B - `git branch -m old-name new-name`
19. C - Pointer to current branch/commit
20. B - `git rebase -i HEAD~3` and mark commits as 'squash'

**Scoring**: ___ / 10

### Advanced Level Answers
21. B - `--soft` keeps changes staged; `--mixed` unstages; `--hard` discards
22. C - On personal branches, after rebasing, or cleaning up before review
23. B - Recovering "lost" commits by showing all HEAD changes
24. B - Binary search through commits to find bugs
25. B - Submodules link external repos; subtrees embed them
26. B - Creates new commit that undoes a previous commit
27. B - `git commit -S` and configure GPG key
28. B - When HEAD points to commit instead of branch
29. B - `git bisect`
30. B - Rewrite history to remove sensitive data

**Scoring**: ___ / 10

### Scenario-Based Answers
Scenario 1: B - Create new branch at current commit, then reset original branch
Scenario 2: B - Update branch with main, resolve conflicts, push
Scenario 3: B - Rotate/revoke compromised credentials immediately
Scenario 4: B - `git rebase main` or `git merge main`
Scenario 5: B - `git stash`, fix bug, `git stash pop`

**Scoring**: ___ / 5

### True or False Answers
31. False - Use merge for shared branches
32. True - `pull` = `fetch` + `merge`
33. False - Can amend, rebase, etc. (though not recommended for shared branches)
34. False - It's just a convention, not a Git feature
35. False - Only the branch pointer is deleted
36. True - Reflog keeps history of HEAD changes
37. False - Git uses snapshots and compression
38. False - Git works fully offline
39. False - Dangerous and can lose team's work
40. False - `git blame` shows who last modified each line

**Scoring**: ___ / 10

### Command Matching Answers
A-6, B-4, C-1, D-3, E-5, F-2

**Scoring**: ___ / 6

### Fill in the Blank Answers
41. `-b`
42. `blame`
43. `revert`
44. `diff`
45. `-a` or `--all`

**Scoring**: ___ / 5

---

## Total Score

**Total Points**: ___ / 56

### Skill Level Assessment

- **0-20**: Beginner - Review [Git Basics](git-basics.md)
- **21-35**: Intermediate - Study [Git Workflows](git-workflows.md) and [Branching Strategies](branching-strategies.md)
- **36-45**: Advanced - Explore [Advanced Git Techniques](advanced-git-techniques.md)
- **46-56**: Expert - Consider contributing to this repository!

---

## Areas for Improvement

Based on your score, focus on:

**If you struggled with Beginner questions:**
- Review [Git Basics](git-basics.md)
- Practice basic commands daily
- Set up a test repository to experiment

**If you struggled with Intermediate questions:**
- Study [Git Workflows](git-workflows.md)
- Read [Branching Strategies](branching-strategies.md)
- Practice with team collaboration scenarios

**If you struggled with Advanced questions:**
- Deep dive into [Advanced Git Techniques](advanced-git-techniques.md)
- Study [Security Best Practices](security-best-practices.md)
- Review [Troubleshooting Guide](troubleshooting-guide.md)

**If you struggled with Scenarios:**
- Read [Real-World Scenarios](real-world-scenarios.md)
- Practice in safe test environments
- Pair program with experienced developers

---

## Practice Exercises

After reviewing weak areas, try these:

### Exercise 1: Branch Management
```bash
# Create test repo
mkdir git-practice && cd git-practice
git init

# Create commits
for i in {1..5}; do
  echo "Line $i" >> file.txt
  git add . && git commit -m "Commit $i"
done

# Practice:
# 1. Create 3 branches
# 2. Make different commits on each
# 3. Merge them into main
# 4. Handle conflicts
# 5. Clean up branches
```

### Exercise 2: Time Travel
```bash
# Using previous repo

# Practice:
# 1. Reset to 2 commits ago
# 2. Use reflog to find "lost" commits
# 3. Recover them
# 4. Create branch at old commit
# 5. Cherry-pick specific commit
```

### Exercise 3: Collaboration Simulation
```bash
# Create two clones (simulating two developers)
git clone original simulate-dev1
git clone original simulate-dev2

# In simulate-dev1:
# 1. Create feature branch
# 2. Make commits
# 3. Push to origin

# In simulate-dev2:
# 1. Fetch changes
# 2. Create conflicting changes
# 3. Resolve conflicts
# 4. Merge
```

---

## Retake Quiz

After studying, retake the quiz to measure improvement!

**First Attempt Score**: ___ / 56 (___%))
**Second Attempt Score**: ___ / 56 (___%))
**Improvement**: ___ points

---

## Additional Resources

- [Git Documentation](https://git-scm.com/doc)
- [Pro Git Book](https://git-scm.com/book)
- [Git Exercises](https://gitexercises.fracz.com/)
- [Learn Git Branching](https://learngitbranching.js.org/)
- [Git Immersion](http://gitimmersion.com/)

---

## Share Your Score

Took the quiz? Share your improvement journey:
- Open an issue with your before/after scores
- Contribute questions you wished were included
- Help others by explaining tricky concepts

Happy learning! 🚀
