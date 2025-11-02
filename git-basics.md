# Git Basics

Git is a distributed version control system that helps you track changes in your codebase, collaborate with others, and manage your project's history.

## Key Concepts:
- **Repository**: A directory containing your project and its version history.
- **Commit**: A snapshot of changes in your project at a specific point in time.
- **Branch**: A separate line of development in your project.
- **Merge**: Combining changes from different branches.

## Common Git Commands:
- `git init`: Initialize a new Git repository.
- `git clone <repository-url>`: Clone an existing repository to your local machine.
- `git add <file>`: Stage changes to a file for commit.
- `git commit -m "<message>"`: Commit staged changes with a descriptive message.
- `git status`: Check the status of your working directory.
- `git log`: View the commit history.

## Helpful Tips:
- Always check the status with `git status` before committing.
- Keep commits small and focused on a single change or feature.

## Try It Yourself

Practice these basic Git commands in a test repository:

```bash
# Create a practice directory
mkdir git-practice
cd git-practice

# Initialize repository
git init

# Create a file and make first commit
echo "Hello Git" >> readme.txt
git status  # See untracked file
git add readme.txt
git status  # See staged file
git commit -m "Add readme file"

# Make more changes
echo "Learning Git basics" >> readme.txt
git status  # See modified file
git add readme.txt
git commit -m "Update readme"

# View your work
git log
git log --oneline

# Practice branching
git branch feature-test
git checkout feature-test
# Or in one command: git checkout -b feature-test

echo "Feature work" >> feature.txt
git add feature.txt
git commit -m "Add feature file"

# Switch back to main
git checkout main
git log --oneline --all --graph
```

## Next Steps

Once comfortable with basics, explore:
- [Branching Strategies](branching-strategies.md) - Learn effective branching
- [Commit Message Best Practices](commit-message-best-practices.md) - Write better commits
- [Git Workflows](git-workflows.md) - Team collaboration patterns
