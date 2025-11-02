# Branching Strategies

Effective branching strategies can help keep your project organized and enable smooth collaboration. Here are some common strategies:

## 1. Feature Branches
Each new feature should be developed in its own branch, separate from `main` or `develop`.

### Steps:
1. Create a feature branch: `git checkout -b feature/my-feature`
2. Commit your changes regularly.
3. Merge the feature branch back into `main` or `develop`.

## 2. Release Branches
A release branch allows you to prepare for a new version of your software without interrupting ongoing development.

### Steps:
1. Create a release branch from `develop`: `git checkout -b release/v1.0 develop`
2. Perform final testing and bug fixes.
3. Merge the release branch into `main` and tag the release.

## 3. Hotfix Branches
Hotfixes are quick fixes for production issues that should be applied immediately.

### Steps:
1. Create a hotfix branch from `main`: `git checkout -b hotfix/v1.1 main`
2. Apply the fix and commit.
3. Merge the hotfix branch into both `main` and `develop`.

## Visual Diagram

```
main:     A---B---C-------------------G---H
               \                     /   /
develop:        D---E---F-----------/---I
                     \             /
feature:              J---K-------/
```

## Try It Yourself

Practice branching strategies in a test repository:

```bash
# Set up repository with main and develop
mkdir branching-practice && cd branching-practice
git init -b main

# Create initial commit
echo "v1.0" >> version.txt
git add . && git commit -m "Initial commit v1.0"

# Create develop branch
git checkout -b develop

# Practice Feature Branch
echo "Starting feature" >> feature.txt
git add . && git commit -m "Start feature work"

git checkout -b feature/new-ui develop
echo "UI updates" >> ui.txt
git add . && git commit -m "Add UI improvements"

# Merge feature to develop
git checkout develop
git merge feature/new-ui
git branch -d feature/new-ui

# Practice Release Branch
git checkout -b release/v2.0 develop
echo "v2.0" >> version.txt
git add . && git commit -m "Bump version to 2.0"

# Merge release to main and tag
git checkout main
git merge release/v2.0
git tag v2.0

# Merge release back to develop
git checkout develop
git merge release/v2.0
git branch -d release/v2.0

# Practice Hotfix
git checkout -b hotfix/v2.0.1 main
echo "Critical fix" >> hotfix.txt
git add . && git commit -m "Fix critical bug"

# Merge to both main and develop
git checkout main
git merge hotfix/v2.0.1
git tag v2.0.1

git checkout develop
git merge hotfix/v2.0.1
git branch -d hotfix/v2.0.1

# View the branch history
git log --oneline --graph --all
```

## Branch Naming Conventions

Use clear, descriptive branch names:

- **Feature**: `feature/user-authentication`, `feature/payment-integration`
- **Bugfix**: `bugfix/login-error`, `fix/null-pointer-exception`
- **Hotfix**: `hotfix/security-patch`, `hotfix/critical-crash`
- **Release**: `release/v1.0.0`, `release/2024-q1`
- **Experimental**: `experiment/new-algorithm`, `spike/performance-test`

## Best Practices

1. **Keep branches short-lived**: Merge within days, not weeks
2. **Update regularly**: Sync with main/develop frequently
3. **Delete after merge**: Clean up merged branches
4. **Use descriptive names**: Include ticket numbers when applicable
5. **One purpose per branch**: Don't mix features and fixes

## Next Steps

- Learn about [Git Workflows](git-workflows.md) for team collaboration
- Explore [Real-World Scenarios](real-world-scenarios.md) for practical examples
- Review [Advanced Git Techniques](advanced-git-techniques.md) for rebase strategies
