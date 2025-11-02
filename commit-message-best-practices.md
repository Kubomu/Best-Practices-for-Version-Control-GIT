# Commit Message Best Practices

Writing clear and concise commit messages is essential for collaboration. Here are some guidelines for writing effective commit messages.

## Structure:
1. **Use the present tense**: Describe what the commit does, not what it did. (e.g., "Add new feature" instead of "Added new feature").
2. **Limit the subject line to 50 characters**: Keep it short and to the point.
3. **Capitalize the first letter** of the subject line.
4. **Use a body**: Provide further explanation, especially if the commit is complex.
5. **Use a footer**: Reference issue numbers or any related information.

## Example:

Add user authentication feature

Implemented user login and registration forms.
Used JWT for token-based authentication.
Updated database schema to store user information.


### Why It Matters:
- Well-written commit messages make it easier to understand project history.
- They help team members quickly find relevant changes and issues.

## Additional Examples

### Good Commit Messages:
```
feat: Add user authentication with JWT

Implemented login and registration endpoints.
Added JWT token generation and validation.
Updated middleware to check authentication.

Relates to #123
```

```
fix: Resolve null pointer exception in payment processing

The payment handler was not checking for null customer
objects before accessing properties.

Added null checks and error handling.

Fixes #456
```

```
docs: Update API documentation for v2.0

Added new endpoints introduced in v2.0.
Clarified authentication requirements.
Fixed typos in examples.
```

### Bad Commit Messages:
```
❌ "fixed stuff"
❌ "changes"
❌ "WIP"
❌ "asdfasdf"
❌ "Updated file.js"
```

## Commit Message Format (Conventional Commits)

Many teams use the Conventional Commits specification:

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Formatting (no code change)
- `refactor`: Code restructuring
- `test`: Adding tests
- `chore`: Maintenance tasks

**Examples:**
```bash
git commit -m "feat(auth): add password reset functionality"
git commit -m "fix(api): handle timeout errors gracefully"
git commit -m "docs(readme): add installation instructions"
git commit -m "refactor(utils): extract validation functions"
```

## Try It Yourself

Practice writing good commit messages:

```bash
# Create test repository
mkdir commit-practice && cd commit-practice
git init

# Bad commit message
echo "line 1" >> file.txt
git add .
git commit -m "update"  # Too vague!

# Good commit message
echo "line 2" >> file.txt
git add .
git commit -m "docs: Add introduction to file

Added introductory line explaining file purpose.
This will help new contributors understand context."

# View your commits
git log

# Compare readability
git log --oneline
```

## Tools to Help

**Pre-commit hooks** to enforce message format:

```bash
# .git/hooks/commit-msg
#!/bin/bash
commit_msg=$(cat "$1")

# Check if message follows convention
if ! echo "$commit_msg" | grep -qE "^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .{1,50}"; then
  echo "Error: Commit message doesn't follow convention"
  echo "Format: <type>(scope): <description>"
  echo "Example: feat(auth): add login functionality"
  exit 1
fi
```

**Git aliases** for easier commits:

```bash
# Add to ~/.gitconfig
[alias]
  cm = commit -m
  cma = commit -a -m
  amend = commit --amend --no-edit
```

## Next Steps

- Practice writing commits in this format
- Set up commit message templates
- Review [Git Workflows](git-workflows.md) for team practices
- Explore [Advanced Git Techniques](advanced-git-techniques.md)
