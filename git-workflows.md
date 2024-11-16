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
