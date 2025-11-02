# Git Workflows

Git workflows define how teams use Git to collaborate and manage their development process. Here are some common Git workflows:

## 1. Centralized Workflow
In the centralized workflow, there is one central repository. Developers push changes directly to the `main` branch.

### Steps:
1. Pull the latest changes: `git pull origin main`
2. Make changes and commit them.
3. Push your changes: `git push origin main`

## 2. Feature Branch Workflow
Each feature is developed in its own branch. After development, the feature branch is merged into `main` or `develop`.

### Steps:
1. Create a new branch: `git checkout -b feature/my-feature`
2. Work on the feature and commit regularly.
3. Merge the feature branch back to `main` using a pull request.

## 3. Gitflow Workflow
Gitflow is a branching model that uses specific branches for different purposes (e.g., `main`, `develop`, `feature`, `release`, `hotfix`).

### Steps:
1. Create a new feature branch from `develop`: `git checkout -b feature/my-feature develop`
2. Merge the feature branch into `develop`.
3. When the release is ready, merge `develop` into `main` and create a release branch.
4. After deployment, hotfix branches are merged into both `main` and `develop`.

## 4. Forking Workflow
This workflow is ideal for open-source projects. Contributors fork the repository and submit pull requests to propose changes.

### Steps:
1. Fork the repository.
2. Create a feature branch.
3. Push changes to your fork.
4. Open a pull request to the original repository.

## Choosing the Right Workflow:
- Use **centralized workflow** for small teams or personal projects.
- Use **feature branch** or **Gitflow workflow** for larger teams.
- Use **forking workflow** for open-source projects.

## Workflow Comparison

| Workflow | Team Size | Complexity | Best For | Branch Count |
|----------|-----------|------------|----------|--------------|
| Centralized | 1-3 | Low | Simple projects | 1 (main) |
| Feature Branch | 3-10 | Medium | Most teams | 2-5 active |
| Gitflow | 10+ | High | Release management | 5+ active |
| Forking | Any | Medium | Open source | Many forks |

## Try It Yourself

### Practice Centralized Workflow

```bash
mkdir centralized-practice && cd centralized-practice
git init

# Simulate main branch development
echo "Feature 1" >> file.txt
git add . && git commit -m "Add feature 1"
git tag central-v1

# All work happens on main
echo "Feature 2" >> file.txt
git add . && git commit -m "Add feature 2"
```

### Practice Feature Branch Workflow

```bash
mkdir feature-branch-practice && cd feature-branch-practice
git init -b main

# Set up main
echo "Initial" >> main.txt
git add . && git commit -m "Initial commit"

# Developer 1: Create feature
git checkout -b feature/login
echo "Login code" >> login.txt
git add . && git commit -m "Add login feature"
git push -u origin feature/login  # In real scenario

# Developer 2: Create another feature
git checkout main
git checkout -b feature/signup
echo "Signup code" >> signup.txt
git add . && git commit -m "Add signup feature"

# Merge features back (via pull requests in real workflow)
git checkout main
git merge feature/login
git merge feature/signup

# Clean up
git branch -d feature/login feature/signup
```

### Practice Gitflow Workflow

```bash
mkdir gitflow-practice && cd gitflow-practice
git init -b main

# Initial production release
echo "v1.0" >> version.txt
git add . && git commit -m "Release v1.0"
git tag v1.0

# Create develop branch
git checkout -b develop

# Feature development
git checkout -b feature/payments develop
echo "Payment system" >> payments.txt
git add . && git commit -m "Add payments"
git checkout develop
git merge feature/payments

# Release preparation
git checkout -b release/v1.1 develop
echo "v1.1" >> version.txt
git add . && git commit -m "Prepare v1.1"

# Deploy to production
git checkout main
git merge release/v1.1
git tag v1.1

# Merge back to develop
git checkout develop
git merge release/v1.1

# Hotfix for production
git checkout -b hotfix/v1.1.1 main
echo "Critical fix" >> hotfix.txt
git add . && git commit -m "Fix critical bug"
git checkout main
git merge hotfix/v1.1.1
git tag v1.1.1
git checkout develop
git merge hotfix/v1.1.1

# View history
git log --oneline --graph --all
```

### Practice Forking Workflow

```bash
# Simulating fork workflow (normally done on GitHub/GitLab)

# Original repository
mkdir original-repo && cd original-repo
git init
echo "Original project" >> README.md
git add . && git commit -m "Initial commit"

# Simulate fork (clone represents fork)
cd ..
git clone original-repo fork-repo
cd fork-repo

# Create feature branch in fork
git checkout -b feature/improvement
echo "My contribution" >> contribution.txt
git add . && git commit -m "Add improvement"

# In real workflow:
# git push origin feature/improvement
# Then create Pull Request on GitHub/GitLab

# Maintainer would review and merge
cd ../original-repo
git remote add contributor ../fork-repo
git fetch contributor
git merge contributor/feature/improvement
```

## Decision Tree: Choosing Your Workflow

```
Start
  |
  ├─ Solo developer or tiny team (1-2)?
  │  └─ Use Centralized Workflow
  │
  ├─ Small-medium team (3-10)?
  │  └─ Use Feature Branch Workflow
  │
  ├─ Large team with releases?
  │  └─ Use Gitflow Workflow
  │
  └─ Open source project?
     └─ Use Forking Workflow
```

## Best Practices for All Workflows

1. **Commit Often**: Small, frequent commits are better than large ones
2. **Pull/Fetch Regularly**: Stay updated with latest changes
3. **Write Good Commit Messages**: Follow [Commit Message Best Practices](commit-message-best-practices.md)
4. **Use Branches**: Even in centralized workflow, use branches for experiments
5. **Code Review**: Always review before merging to main
6. **Automate Testing**: Use CI/CD to run tests automatically
7. **Document Workflow**: Make sure team knows which workflow to follow

## Common Pitfalls

### ❌ Don't:
- Push directly to main in multi-person teams
- Keep branches alive for months
- Mix multiple unrelated changes in one branch
- Forget to delete merged branches
- Skip code review process

### ✅ Do:
- Use pull requests for code review
- Keep branches short-lived (days, not weeks)
- Focus one branch on one feature/fix
- Clean up regularly
- Communicate with team about changes

## Hybrid Workflows

Many teams combine workflows:

**Example: Gitflow + Feature Branches**
- Use Gitflow structure (main, develop, release, hotfix)
- Add feature branches for all development
- Require pull requests for all merges
- Use CI/CD for automated testing

**Example: Trunk-Based Development**
- Short-lived feature branches (1-2 days)
- Merge to main frequently
- Use feature flags for incomplete features
- Heavy reliance on automated testing

## Next Steps

- Practice your team's workflow in test repository
- Set up branch protection rules
- Configure CI/CD pipelines
- Review [Branching Strategies](branching-strategies.md)
- Explore [Real-World Scenarios](real-world-scenarios.md)
