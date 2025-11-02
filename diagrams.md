# Git Visual Diagrams

ASCII diagrams to help visualize Git concepts and workflows.

## Git Architecture

```
┌─────────────────────────────────────────────┐
│           Remote Repository                 │
│              (GitHub/GitLab)                │
└──────────────┬──────────────────────────────┘
               │
        push   │   fetch/pull
               │
┌──────────────▼──────────────────────────────┐
│          Local Repository                   │
│            (.git directory)                 │
└──────────────┬──────────────────────────────┘
               │
        commit │   checkout
               │
┌──────────────▼──────────────────────────────┐
│         Staging Area (Index)                │
└──────────────┬──────────────────────────────┘
               │
         add   │   restore
               │
┌──────────────▼──────────────────────────────┐
│        Working Directory                    │
│         (Your Files)                        │
└─────────────────────────────────────────────┘
```

## File States

```
Untracked ─────add─────> Staged ─────commit─────> Committed
   │                        │                          │
   │                        │                          │
   └────────────────────────┴────checkout────<─────────┘
                            │
                       Modified
```

## Branch Visualization

### Feature Branch Workflow

```
main:     A───B───C───────────G───H
               \             /
feature:        D───E───F───/
```

### Gitflow

```
main:       A───────────────E───────H
             \             /       /
develop:      B───C───D───/───F───G───I
               \     /           /
feature:        J───K───────────/
```

### Rebase vs Merge

**Before:**
```
main:     A───B───C
               \
feature:        D───E
```

**After Merge:**
```
main:     A───B───C───────M
               \         /
feature:        D───E───/
```

**After Rebase:**
```
main:     A───B───C
                   \
feature:            D'───E'
```

## Merge Strategies

### Fast-Forward Merge

**Before:**
```
main:     A───B
               \
feature:        C───D
```

**After:**
```
main:     A───B───C───D
```

### No Fast-Forward Merge

**Before:**
```
main:     A───B
               \
feature:        C───D
```

**After:**
```
main:     A───B───────M
               \     /
feature:        C───D
```

### Three-Way Merge

**Before:**
```
main:     A───B───C
               \
feature:        D───E
```

**After:**
```
main:     A───B───C───────M
               \         /
feature:        D───E───/
```

## Conflict Resolution

```
File with conflict:
┌─────────────────────────────┐
│ <<<<<<< HEAD                │
│ Your changes                │
│ =======                     │
│ Their changes               │
│ >>>>>>> feature-branch      │
└─────────────────────────────┘
         │
         │ resolve conflict
         ▼
┌─────────────────────────────┐
│ Resolved content            │
│ (keep yours, theirs, or     │
│  combine both)              │
└─────────────────────────────┘
         │
         │ git add
         ▼
     Resolved!
```

## Rebase Process

```
Before rebase:
main:     A───B───C
               \
feature:        D───E

Rebase steps:
1. Git finds common ancestor (B)
2. Saves D and E temporarily
3. Checks out C
4. Applies D' (rewritten)
5. Applies E' (rewritten)

After rebase:
main:     A───B───C
                   \
feature:            D'───E'
```

## Cherry-Pick

```
Before:
main:     A───B───C
               \
feature:        D───E───F

Cherry-pick E to main:

After:
main:     A───B───C───E'
               \
feature:        D───E───F
```

## Git Stash

```
Working Directory (modified):
┌─────────────────────┐
│ file1.txt (changed) │
│ file2.txt (changed) │
└─────────────────────┘
         │
         │ git stash
         ▼
┌─────────────────────┐
│   Stash Storage     │
│   stash@{0}         │
└─────────────────────┘
         │
         │ git stash pop
         ▼
Working Directory (restored):
┌─────────────────────┐
│ file1.txt (changed) │
│ file2.txt (changed) │
└─────────────────────┘
```

## Reset Types

### --soft

```
Before:
HEAD → C commits: A←B←C

After reset --soft HEAD~1:
HEAD → B commits: A←B  C(orphaned)
Staging: changes from C
Working: no changes
```

### --mixed (default)

```
Before:
HEAD → C commits: A←B←C

After reset HEAD~1:
HEAD → B commits: A←B  C(orphaned)
Staging: empty
Working: changes from C
```

### --hard

```
Before:
HEAD → C commits: A←B←C

After reset --hard HEAD~1:
HEAD → B commits: A←B  C(orphaned)
Staging: empty
Working: empty (DANGER: changes lost!)
```

## Pull Request Flow

```
1. Fork/Clone:
Original ─────fork────> Your Fork
   │                        │
   │                        │ clone
   │                        ▼
   │                   Local Copy

2. Create Feature:
Your Fork
   │
   │ create branch
   ▼
Feature Branch
   │
   │ commits
   ▼
Push to Fork

3. Pull Request:
Your Fork ────PR────> Original
               │
               │ review
               │ approve
               ▼
            Merged!
```

## Reflog Recovery

```
Main timeline:
A───B───C───D (HEAD)
         │
         │ git reset --hard B
         ▼
A───B (HEAD)   C───D (orphaned)
         │
         │ git reflog
         │ find C's hash
         │ git reset --hard <hash>
         ▼
A───B───C───D (HEAD recovered!)
```

## Bisect Process

```
Git bisect binary search:

bad                                     good
 │                                       │
 v                                       v
 A───B───C───D───E───F───G───H───I───J

Step 1: Test E
├─── bad ───┤───── ? ─────────────────>

Step 2: Test B or C
├─ ? ─┤──?──┤───── known bad ─────────>

Step 3: Found!
├─ ok ─┼─BAD─┤──── known bad ─────────>
        ^
    Culprit commit!
```

## Submodule Structure

```
Main Repository:
┌─────────────────────────────────┐
│  src/                           │
│  ├── main.js                    │
│  └── submodules/                │
│      ├── library-a/ ───> Remote A
│      │   └── (separate repo)   │
│      └── library-b/ ───> Remote B
│          └── (separate repo)   │
└─────────────────────────────────┘
```

## Workflow Decision Tree

```
                Start
                  │
                  ▼
        Solo developer? ───yes──> Centralized
                  │
                 no
                  │
                  ▼
        Small team (3-10)? ───yes──> Feature Branch
                  │
                 no
                  │
                  ▼
        Need releases? ───yes──> Gitflow
                  │
                 no
                  │
                  ▼
        Open source? ───yes──> Forking Workflow
                  │
                 no
                  │
                  ▼
           Trunk-Based Dev
```

## Commit Graph Symbols

```
*   Commit
│   Branch line
┼   Branch split
╮   Branch merge
●   Current HEAD
▲   Tag
⚡  Stash
```

## Timeline Example

```
* commit d4e5f6g (HEAD -> main, origin/main)
│ Author: Jane Doe
│ Date: Mon Nov 2 14:30:00 2025
│
│   feat: Add user authentication
│
* commit a1b2c3d
│ Author: John Smith
│ Date: Mon Nov 2 10:15:00 2025
│
│   fix: Resolve login bug
│
* commit 7f8g9h0
  Author: Jane Doe
  Date: Sun Nov 1 16:45:00 2025

    docs: Update README
```

## Remote Sync States

```
Up to date:
Local:  A───B───C (HEAD)
Remote: A───B───C (origin/main)

Behind:
Local:  A───B (HEAD)
Remote: A───B───C───D (origin/main)
        Need: git pull

Ahead:
Local:  A───B───C───D (HEAD)
Remote: A───B (origin/main)
        Need: git push

Diverged:
Local:  A───B───C (HEAD)
             \
Remote: A───B───D (origin/main)
        Need: git pull (then merge or rebase)
```

## Git Object Model

```
Repository:
┌─────────────────────────────────┐
│  Commits (nodes)                │
│    │                            │
│    ├─> Trees (directories)      │
│    │     │                      │
│    │     ├─> Blobs (files)      │
│    │     └─> Trees (subdirs)    │
│    │                            │
│    └─> Parent commit(s)         │
│                                 │
│  References:                    │
│    ├─> HEAD (current)           │
│    ├─> main (branch)            │
│    └─> origin/main (remote)     │
└─────────────────────────────────┘
```

---

## Using These Diagrams

These ASCII diagrams can be:
- Copied into documentation
- Used in presentations
- Included in issue comments
- Shared with team members
- Printed as quick references

For more complex visualizations, try:
- [Git Graph VS Code Extension](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph)
- [GitKraken](https://www.gitkraken.com/)
- [SourceTree](https://www.sourcetreeapp.com/)

## Related Documentation

- [Git Basics](git-basics.md) - Fundamental concepts explained
- [Branching Strategies](branching-strategies.md) - When to use each pattern
- [Advanced Git Techniques](advanced-git-techniques.md) - Deep dives
- [Git Workflows](git-workflows.md) - Team collaboration
