# Resolving Merge Conflicts

Merge conflicts occur when Git cannot automatically merge changes from different branches. This typically happens when the same part of the file is modified in both branches.

### Steps to Resolve Merge Conflicts:
1. **Identify the conflict**: Git will mark the conflicting files with `<<<<<`, `=====` and `>>>>>`.
2. **Open the file**: Review the changes and decide which version to keep.
3. **Resolve the conflict**: Manually edit the file to combine or choose the changes.
4. **Stage the resolved file**: `git add <file>`
5. **Commit the merge**: `git commit`
6. **Push changes**: `git push origin <branch-name>`

### Tips:
- Keep your branches updated with `git pull` to avoid conflicts.
- Communicate with team members to avoid simultaneous edits to the same sections of the code.
