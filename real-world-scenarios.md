# Real-World Git Scenarios

Practical case studies and examples from real team collaboration scenarios.

## Table of Contents
1. [Feature Development Workflow](#feature-development-workflow)
2. [Emergency Hotfix Production](#emergency-hotfix-production)
3. [Managing Multiple Release Versions](#managing-multiple-release-versions)
4. [Open Source Contribution](#open-source-contribution)
5. [Code Review Process](#code-review-process)
6. [Resolving Complex Merge Conflicts](#resolving-complex-merge-conflicts)
7. [Recovering from Mistakes](#recovering-from-mistakes)
8. [Team Collaboration Best Practices](#team-collaboration-best-practices)

---

## Feature Development Workflow

### Scenario
Sarah is developing a new user authentication feature that will take 2 weeks. Meanwhile, other team members continue working on main.

### The Challenge
- Keep feature branch updated with main
- Avoid conflicts at merge time
- Maintain clean commit history
- Enable parallel testing

### Solution: Feature Branch Workflow

```bash
# Day 1: Start feature
git checkout main
git pull origin main
git checkout -b feature/user-authentication

# Make initial commits
git add src/auth/
git commit -m "Add authentication models"
git push -u origin feature/user-authentication

# Day 3: Update from main
git fetch origin
git rebase origin/main
# Or: git merge origin/main (if rebasing is risky)
git push --force-with-lease origin feature/user-authentication

# Day 7: Midpoint - keep syncing
git fetch origin
git rebase origin/main
# Resolve any conflicts
git add .
git rebase --continue
git push --force-with-lease origin feature/user-authentication

# Day 14: Ready for review
git fetch origin
git rebase origin/main  # Final sync
git push --force-with-lease origin feature/user-authentication

# Create Pull Request in GitHub/GitLab UI

# After approval
git checkout main
git pull origin main
git merge --no-ff feature/user-authentication
git push origin main

# Clean up
git branch -d feature/user-authentication
git push origin --delete feature/user-authentication
```

### Key Takeaways
- Update feature branch regularly (every 2-3 days)
- Use rebase for clean history before PR
- Use merge commit for PR to main
- Delete branches after merging

### Try It Yourself

```bash
# Simulate this workflow
mkdir feature-workflow && cd feature-workflow
git init

# Create main branch history
git checkout -b main
for i in {1..5}; do
  echo "Main update $i" >> main.txt
  git add . && git commit -m "Main update $i"
done

# Create feature branch from earlier point
git checkout HEAD~3
git checkout -b feature/auth
echo "Auth feature" >> auth.txt
git add . && git commit -m "Add auth"

# Update feature branch
git rebase main
git log --oneline --graph --all
```

---

## Emergency Hotfix Production

### Scenario
Production has a critical bug affecting all users. It's Friday 4 PM. The bug is in release v2.3.1. Main branch has moved ahead with v2.4.0 features.

### The Challenge
- Fix production immediately
- Don't include unreleased features
- Apply fix to both production and development
- Track fix in version history

### Solution: Hotfix Workflow

```bash
# 1. Confirm production version
git checkout v2.3.1
git tag  # Verify tag exists

# 2. Create hotfix branch from production tag
git checkout -b hotfix/critical-login-bug v2.3.1

# 3. Fix the bug
# Edit files...
vim src/auth/login.js
# Test locally
npm test

# 4. Commit the fix
git add src/auth/login.js
git commit -m "Fix critical login validation bug

Bug was causing null pointer exception when email field empty.
Added validation check before processing.

Fixes #1234"

# 5. Create hotfix release
git tag v2.3.2 -m "Hotfix: Critical login bug"
git push origin hotfix/critical-login-bug
git push origin v2.3.2

# 6. Deploy v2.3.2 to production
# (CI/CD pipeline deploys tagged version)

# 7. Merge hotfix to main
git checkout main
git pull origin main
git merge --no-ff hotfix/critical-login-bug
git push origin main

# 8. Merge hotfix to develop (if using Gitflow)
git checkout develop
git pull origin develop
git merge --no-ff hotfix/critical-login-bug
git push origin develop

# 9. Clean up
git branch -d hotfix/critical-login-bug
git push origin --delete hotfix/critical-login-bug

# 10. Notify team
# Send message: "Hotfix v2.3.2 deployed, bug #1234 resolved"
```

### Timeline
```
Production (v2.3.1):  A---B---C
                               \
Hotfix:                         H (fix)
                               /
Production (v2.3.2):  A---B---C---H (tagged v2.3.2)

Main (unreleased):    A---B---C---D---E---F
                                          \
After merge:          A---B---C---D---E---F---M---H
```

### Key Takeaways
- Always branch from production tag
- Test thoroughly before tagging
- Merge back to all active branches
- Document in commit message
- Communicate with team

---

## Managing Multiple Release Versions

### Scenario
Company maintains three versions: v1.x (legacy), v2.x (current), v3.x (next gen). Customers on different versions need security updates.

### The Challenge
- Maintain multiple active releases
- Apply security patches to all versions
- Avoid feature leak between versions
- Track which fix goes where

### Solution: Release Branch Strategy

```bash
# Initial setup
# main - bleeding edge development
# release/v1 - legacy support
# release/v2 - current stable
# release/v3 - next generation

# Security vulnerability discovered!
# Must patch all versions

# 1. Create fix on latest first
git checkout main
git checkout -b fix/security-vulnerability

# Develop fix
vim src/security/validator.js
git add .
git commit -m "Fix XSS vulnerability in user input"
git push origin fix/security-vulnerability

# Create PR, get reviewed, merge to main
# Assume merged to main as commit abc123

# 2. Cherry-pick to v3 release
git checkout release/v3
git cherry-pick abc123
git push origin release/v3
git tag v3.1.5
git push origin v3.1.5

# 3. Cherry-pick to v2 release
git checkout release/v2
git cherry-pick abc123
# Might need adjustments for older codebase
git push origin release/v2
git tag v2.8.3
git push origin v2.8.3

# 4. Cherry-pick to v1 release
git checkout release/v1
git cherry-pick abc123
# Likely needs manual adjustments
# Resolve conflicts, test thoroughly
git add .
git cherry-pick --continue
git push origin release/v1
git tag v1.15.9
git push origin v1.15.9

# 5. Document in release notes
echo "## Security Update
- Fixed XSS vulnerability (CVE-2024-1234)
- Affects: All versions
- Patched in: v1.15.9, v2.8.3, v3.1.5" >> CHANGELOG.md

# 6. Coordinate releases
# All versions released simultaneously
```

### Release Matrix

| Version | Branch | Latest | Support Level | Update Frequency |
|---------|--------|--------|---------------|------------------|
| v1.x | release/v1 | v1.15.9 | Security only | As needed |
| v2.x | release/v2 | v2.8.3 | Full support | Monthly |
| v3.x | release/v3 | v3.1.5 | Active dev | Weekly |
| main | main | N/A | Unstable | Continuous |

### Key Takeaways
- Use release branches for long-term support
- Cherry-pick security fixes to all versions
- Test each version independently
- Maintain clear documentation
- Coordinate release timing

---

## Open Source Contribution

### Scenario
Alex wants to contribute a bug fix to a popular open-source project with 500+ contributors.

### The Challenge
- Follow project's contribution guidelines
- Keep fork synchronized
- Create clean, reviewable PR
- Respond to reviewer feedback
- Handle conflicts during review

### Solution: Fork and Pull Request Workflow

```bash
# 1. Fork repository on GitHub
# Click "Fork" button on github.com/original/project

# 2. Clone your fork
git clone https://github.com/alex/project.git
cd project

# 3. Add upstream remote
git remote add upstream https://github.com/original/project.git
git fetch upstream

# 4. Create feature branch from upstream
git checkout -b fix/issue-1234 upstream/main

# 5. Make changes
vim src/components/header.js
npm test  # Run tests

# 6. Commit following project's convention
# Read CONTRIBUTING.md first!
git add src/components/header.js
git commit -m "fix(header): resolve navbar overflow on mobile

- Add max-width constraint to navbar items
- Implement horizontal scroll for overflow
- Add unit tests for responsive behavior

Fixes #1234"

# 7. Push to your fork
git push -u origin fix/issue-1234

# 8. Create Pull Request on GitHub
# Go to your fork, click "Create Pull Request"
# Fill out PR template completely

# 9. Reviewer requests changes
# Make changes locally
vim src/components/header.js
git add .
git commit -m "refactor: use flexbox instead of grid per review"
git push origin fix/issue-1234
# PR automatically updates

# 10. Maintainer requests squashing commits
git rebase -i upstream/main
# Mark commits as "squash" except first
git push --force-with-lease origin fix/issue-1234

# 11. Keep PR updated while waiting for review
git fetch upstream
git rebase upstream/main
git push --force-with-lease origin fix/issue-1234

# 12. PR is merged!
# Clean up local branch
git checkout main
git pull upstream main
git push origin main  # Update your fork's main
git branch -d fix/issue-1234
git push origin --delete fix/issue-1234

# 13. Stay synced for future contributions
git fetch upstream
git merge upstream/main
git push origin main
```

### PR Best Practices Checklist

- [ ] Read CONTRIBUTING.md thoroughly
- [ ] Search existing issues/PRs first
- [ ] Create issue before PR (if required)
- [ ] Branch from latest upstream/main
- [ ] Follow project's commit message format
- [ ] Add tests for new functionality
- [ ] Update documentation
- [ ] Keep PR focused on single issue
- [ ] Respond to reviews promptly
- [ ] Be respectful and professional

### Key Takeaways
- Always work on forks, not upstream directly
- Keep commits clean and well-documented
- Be responsive to maintainer feedback
- Follow project conventions strictly
- Sync regularly with upstream

---

## Code Review Process

### Scenario
Team of 5 developers using Pull Request workflow. Every PR needs 2 approvals before merging.

### The Challenge
- Ensure thorough reviews
- Provide constructive feedback
- Handle requested changes
- Maintain momentum

### Solution: Structured Review Process

#### As PR Author

```bash
# 1. Prepare PR
git checkout -b feature/payment-integration
# Develop feature...
git add .
git commit -m "Add payment integration"

# 2. Self-review before creating PR
git diff main...feature/payment-integration
# Check for:
# - Debug code
# - TODOs
# - Console.logs
# - Secrets
# - Formatting issues

# 3. Create descriptive PR
git push -u origin feature/payment-integration

# PR Description Template:
"""
## Summary
Integrates Stripe payment processing into checkout flow.

## Changes
- Add Stripe API client configuration
- Implement payment processing endpoint
- Add payment confirmation UI
- Update checkout workflow

## Testing
- [ ] Unit tests pass (npm test)
- [ ] Integration tests pass
- [ ] Tested with Stripe test mode
- [ ] Verified error handling
- [ ] UI tested on Chrome, Firefox, Safari

## Screenshots
[Add screenshots of UI changes]

## Related Issues
Closes #456

## Checklist
- [x] Code follows project style guidelines
- [x] Added tests
- [x] Updated documentation
- [x] No console.logs or debug code
- [x] No merge conflicts
"""

# 4. Address review comments
# Reviewer requests changes...
git checkout feature/payment-integration
# Make changes...
git add .
git commit -m "refactor: extract payment validation logic"
git push origin feature/payment-integration

# 5. Request re-review
# Comment on PR: "@reviewer changes addressed, PTAL"
```

#### As Reviewer

```bash
# 1. Fetch PR branch
git fetch origin pull/123/head:pr-123
git checkout pr-123

# 2. Review checklist
# - [ ] Code is understandable
# - [ ] Tests are adequate
# - [ ] No security issues
# - [ ] Follows style guidelines
# - [ ] Documentation updated
# - [ ] No performance issues

# 3. Test locally
npm install
npm test
npm run dev  # Manual testing

# 4. Leave constructive comments
# Good comment:
# "Consider extracting this logic into a separate function
# for better testability. Example:
# function validatePayment(data) { ... }"

# Bad comment:
# "This code is bad."

# 5. Approve or request changes
# In GitHub UI, click "Review Changes"
```

### Review Comment Examples

**Good Comments:**
```
✅ "Great error handling here! Consider also catching network timeouts."
✅ "This function is getting large. Could we extract the validation logic?"
✅ "Nice work! One suggestion: add a comment explaining the regex pattern."
✅ "Security concern: This endpoint should require authentication."
```

**Bad Comments:**
```
❌ "This is wrong."
❌ "Why did you do it this way?"
❌ "I don't like this."
❌ "This won't work."
```

### Key Takeaways
- Review code within 24 hours
- Be specific and constructive
- Test locally when possible
- Approve when ready (don't nitpick)
- Author should respond within 24 hours

---

## Resolving Complex Merge Conflicts

### Scenario
Two developers worked on same module for 2 weeks. 50 files changed on both branches. Merge conflict in 15 files.

### The Challenge
- Understand both sets of changes
- Preserve functionality from both branches
- Test combined result
- Avoid breaking changes

### Solution: Systematic Conflict Resolution

```bash
# 1. Prepare for merge
git checkout feature/user-dashboard
git fetch origin
git log origin/feature/admin-dashboard --oneline

# 2. Attempt merge
git merge origin/feature/admin-dashboard
# CONFLICT (content): Merge conflict in 15 files

# 3. Assess conflicts
git status
# Shows all conflicted files

# 4. Prioritize by impact
# Critical: authentication, database
# High: core business logic
# Medium: UI components
# Low: tests, documentation

# 5. Resolve systematically
# Start with critical files
vim src/auth/permissions.js

# Understand both versions
git show HEAD:src/auth/permissions.js > /tmp/mine.js
git show MERGE_HEAD:src/auth/permissions.js > /tmp/theirs.js
diff /tmp/mine.js /tmp/theirs.js

# Resolve conflict markers
# <<<<<<< HEAD
# Your version
# =======
# Their version
# >>>>>>> feature/admin-dashboard

# 6. Test after each file
git add src/auth/permissions.js
npm test src/auth/permissions.test.js

# 7. For complex conflicts, use merge tool
git mergetool
# Opens visual diff tool

# 8. Communicate with other developer
# "Hey, I'm resolving conflicts in permissions.js.
# Your changes add role-based access, mine add
# resource-level permissions. I'm combining both
# approaches. Can you review?"

# 9. Complete merge
git add .
git merge --continue

# 10. Comprehensive testing
npm test          # Unit tests
npm run e2e       # End-to-end tests
npm run lint      # Linting
npm run build     # Build check

# 11. Get review before pushing
git push origin feature/user-dashboard
# Request review from both original developers
```

### Conflict Resolution Strategy

```
1. UNDERSTAND
   ├─ Read both versions
   ├─ Understand intent
   └─ Identify overlap vs. independent changes

2. COMMUNICATE
   ├─ Talk to other developer(s)
   ├─ Discuss approach
   └─ Agree on solution

3. RESOLVE
   ├─ Keep both changes when possible
   ├─ Refactor if needed
   └─ Test incrementally

4. VERIFY
   ├─ All tests pass
   ├─ Manual testing
   └─ Code review
```

### Key Takeaways
- Don't rush complex merges
- Communicate with affected developers
- Test thoroughly after resolution
- Consider pair programming for complex conflicts
- Document major resolution decisions

---

## Recovering from Mistakes

### Scenario
Junior developer accidentally force-pushed to main, losing 2 days of team work. 5 developers affected.

### The Challenge
- Recover lost commits
- Restore team's work
- Prevent future occurrences
- Maintain trust

### Solution: Emergency Recovery

```bash
# 1. DON'T PANIC - Git rarely loses data

# 2. Get information from affected developer
# "What was the last commit you remember?"
# "Do you have uncommitted work?"

# 3. Check reflog on server (if accessible)
# On GitHub: check "restore branch" feature
# On GitLab: check protected branch settings

# 4. Find lost commits via reflog
# Each affected developer:
git reflog
# Look for commits before force push

# 5. Recover from reflog
git log --all --reflog --oneline | grep "commit message pattern"
# Find commit hash: abc123

# 6. Restore main branch
git checkout main
git reset --hard abc123  # The lost commit
git push --force-with-lease origin main

# 7. Alternative: Recover from team member's clone
# Team member with lost commits:
cd their-repo
git log --oneline
# Last good commit: def456

# Push from their machine
git push --force-with-lease origin main

# 8. Verify recovery
git log --oneline -10
# Confirm all commits present

# 9. All team members: Update local repos
git fetch origin
git reset --hard origin/main

# 10. Implement protection
# On GitHub/GitLab:
# Settings → Branches → Protect main branch
# ✓ Require pull request reviews
# ✓ Dismiss stale reviews
# ✓ Require review from code owners
# ✓ Restrict who can push
# ✓ Do not allow force pushes
# ✓ Require linear history

# 11. Update local git hooks for team
cat << 'EOF' > .git/hooks/pre-push
#!/bin/bash
protected_branch='main'
current_branch=$(git rev-parse --abbrev-ref HEAD)

if [ "$current_branch" = "$protected_branch" ]; then
  echo "ERROR: Direct push to $protected_branch is not allowed!"
  echo "Please create a feature branch and submit a PR."
  exit 1
fi
EOF

chmod +x .git/hooks/pre-push

# 12. Team training session
# - Review what happened
# - Explain git safety
# - Practice recovery procedures
# - No blame, focus on learning
```

### Post-Incident Review

**What Happened:**
- Junior dev ran `git push --force` on main
- Lost 15 commits from 5 developers

**Why It Happened:**
- Main branch not protected
- No pre-push hooks
- Insufficient training

**How It Was Fixed:**
- Recovered via reflog
- All commits restored
- No data lost

**Prevention Measures:**
1. Enable branch protection
2. Install pre-push hooks
3. Team training on git safety
4. Pair programming for juniors
5. Document recovery procedures

### Key Takeaways
- Git reflog saves the day
- Protect critical branches
- Use hooks for safety
- Train team thoroughly
- Have disaster recovery plan

---

## Team Collaboration Best Practices

### Daily Workflow

```bash
# Morning: Start day
git checkout main
git pull origin main
git checkout -b feature/today-work

# Mid-day: Save progress
git add .
git commit -m "WIP: half-done feature"
git push origin feature/today-work

# Afternoon: Update from main
git fetch origin
git rebase origin/main

# End of day: Clean up
git rebase -i HEAD~3  # Squash WIP commits
git push --force-with-lease origin feature/today-work
```

### Communication Practices

1. **Commit Messages**: Write for teammates
   ```bash
   # Good
   git commit -m "Fix race condition in user login

   The session token was being created before user
   validation completed, causing intermittent failures.

   Added await to ensure sequential processing.

   Fixes #1234"

   # Bad
   git commit -m "fix bug"
   ```

2. **Branch Names**: Self-documenting
   ```bash
   feature/USER-123-payment-integration
   bugfix/ISSUE-456-login-timeout
   hotfix/critical-security-patch
   ```

3. **PR Descriptions**: Thorough explanations
   - What changed
   - Why it changed
   - How to test
   - Screenshots/videos
   - Related issues

### Team Agreements

Document in README or CONTRIBUTING.md:

```markdown
## Git Workflow

1. **Branching**
   - Branch from latest main
   - Name: `<type>/<ticket>-<description>`
   - Update daily from main

2. **Commits**
   - Small, focused commits
   - Meaningful messages
   - Reference ticket numbers

3. **Pull Requests**
   - Require 2 approvals
   - All tests must pass
   - No force-push after review starts
   - Delete branch after merge

4. **Code Review**
   - Review within 24 hours
   - Be constructive
   - Test locally for complex changes
   - Approve when ready

5. **Protected Branches**
   - main: No direct push
   - develop: No force push
   - release/*: Maintainers only
```

### Key Takeaways
- Establish clear workflows
- Document team agreements
- Communicate proactively
- Review promptly
- Respect teammate's time

---

## Conclusion

These real-world scenarios demonstrate that successful Git usage requires:
- **Technical skills**: Commands and workflows
- **Communication**: Clear messages and discussions
- **Process**: Established procedures
- **Recovery**: Plans for when things go wrong
- **Continuous learning**: Share knowledge and improve

Practice these scenarios in safe environments before applying them to production codebases!

---

## Additional Scenarios to Explore

- Migrating from SVN to Git
- Managing monorepo with multiple teams
- Implementing Continuous Integration
- Handling large binary assets
- Coordinating across time zones
- Teaching Git to new team members

See [FAQ](faq.md) and [Troubleshooting Guide](troubleshooting-guide.md) for more scenarios.
