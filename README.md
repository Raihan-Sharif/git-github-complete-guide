# Git & GitHub: The Complete Guide for Developers

## Table of Contents

- [Introduction](#introduction)
- [Git Basics](#git-basics)
  - [What is Git?](#what-is-git)
  - [Git vs GitHub](#git-vs-github)
  - [Installation & Configuration](#installation--configuration)
- [Git Essentials](#git-essentials)
  - [Repository Initialization](#repository-initialization)
  - [Git Workflow](#git-workflow)
  - [Basic Commands](#basic-commands)
  - [Commit Best Practices](#commit-best-practices)
- [Branching & Merging](#branching--merging)
  - [Understanding Branches](#understanding-branches)
  - [Branching Strategies](#branching-strategies)
  - [Merging Techniques](#merging-techniques)
  - [Resolving Conflicts](#resolving-conflicts)
- [GitHub Collaboration](#github-collaboration)
  - [Setting Up Authentication](#setting-up-authentication)
    - [SSH Authentication](#ssh-authentication)
    - [Personal Access Tokens (PAT)](#personal-access-tokens-pat)
  - [Remote Repository Management](#remote-repository-management)
  - [Pull Requests](#pull-requests)
  - [Code Reviews](#code-reviews)
- [Real-world Workflows](#real-world-workflows)
  - [Feature Development](#feature-development)
  - [Bug Fixing](#bug-fixing)
  - [Release Management](#release-management)
- [Advanced Git Techniques](#advanced-git-techniques)
  - [Rebasing](#rebasing)
  - [Stashing](#stashing)
  - [Cherry-picking](#cherry-picking)
  - [Undoing Changes](#undoing-changes)
- [GitHub Best Practices](#github-best-practices)
  - [Repository Structure](#repository-structure)
  - [README and Documentation](#readme-and-documentation)
  - [Issue Management](#issue-management)
  - [GitHub Actions Basics](#github-actions-basics)
- [Troubleshooting Common Issues](#troubleshooting-common-issues)
- [Cheat Sheet Reference](#cheat-sheet-reference)

## Introduction

Version control is a fundamental skill for any software developer. Git, the most widely used version control system, enables developers to track changes, collaborate effectively, and maintain a complete history of their projects. When paired with GitHub, Git becomes even more powerful, providing a platform for storing code, collaborating with others, and showcasing your work.

This guide is designed to help you understand Git and GitHub from the ground up, covering everything from basic commands to real-world development workflows and best practices.

## Git Basics

### What is Git?

Git is a distributed version control system designed to track changes in source code during software development. Created by Linus Torvalds in 2005, Git allows multiple developers to work together on non-linear development through its branching and merging features.

Key features of Git:

- **Distributed architecture**: Each developer has a complete copy of the repository
- **Branching and merging**: Support for non-linear development
- **Data integrity**: Changes are tracked with cryptographic hashes
- **Speed**: Operations are performed locally, making Git fast compared to centralized systems

### Git vs GitHub

People often confuse Git and GitHub, but they serve different purposes:

| Git                                | GitHub                                                    |
| ---------------------------------- | --------------------------------------------------------- |
| Distributed version control system | Web-based hosting service for Git repositories            |
| Installed locally on your computer | Cloud-based platform for collaboration                    |
| Manages version history            | Adds collaboration features like PRs, issues, discussions |
| Works offline                      | Requires internet connection                              |
| Free and open-source               | Free with paid tiers for advanced features                |

### Installation & Configuration

**Installing Git:**

```bash
# Download Git from https://git-scm.com and install
# Verify installation
git --version
```

**Initial Configuration:**

```bash
# Set your identity
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Optional - Set password (not recommended, use SSH or PAT instead)
git config --global user.password "******"

# Set default editor (VS Code in this example)
git config --global core.editor "code --wait"

# Configure line ending behaviors (different for OS)
# For Windows:
git config --global core.autocrlf true
# For macOS/Linux:
git config --global core.autocrlf input

# View all configurations
git config --list
```

**Understanding Configuration Levels:**

- `--global`: Settings apply to all repositories for current user
- `--system`: Settings apply to all users on the system
- `--local`: Settings apply only to current repository (default if no flag specified)

## Git Essentials

### Repository Initialization

**Creating a New Repository:**

```bash
# Create a new directory
mkdir my-project
cd my-project

# Initialize as a Git repository
git init

# Check repository status
git status
```

**What happens when you initialize a repository?**

- Git creates a hidden `.git` directory that contains all the data and configuration for version control
- Your project directory becomes a working directory where you can make changes
- No changes are tracked until you explicitly add and commit them

### Git Workflow

Understanding the basic Git workflow is crucial:

1. **Working Directory**: Where you modify files
2. **Staging Area (Index)**: Where you prepare changes for a commit
3. **Repository**: Where Git stores the history of your project as commits

![Git Workflow](https://git-scm.com/book/en/v2/images/areas.png)

### Basic Commands

**Staging Changes:**

```bash
# Stage specific files
git add file1.js file2.js

# Stage all files in a directory
git add .

# Stage files matching a pattern
git add *.html

# Stage parts of a file
git add -p file.js
```

**Checking Status:**

```bash
# Full status
git status

# Short status
git status -s
```

**Committing Changes:**

```bash
# Commit with message
git commit -m "Add login functionality"

# Commit with detailed message (opens editor)
git commit

# Stage and commit all tracked files in one command
git commit -am "Fix login bug"
```

**Viewing History:**

```bash
# View commit history
git log

# Compact one-line view
git log --oneline

# Reverse order (oldest first)
git log --reverse

# Graph view with all branches
git log --oneline --all --graph
```

**Inspecting Commits:**

```bash
# View specific commit details
git show 921a2ff

# View the most recent commit
git show HEAD

# View two commits before HEAD
git show HEAD~2

# View a file from a specific commit
git show HEAD:file.html
```

### Commit Best Practices

Writing good commit messages is crucial for maintaining a clear and useful project history:

**Commit Message Structure:**

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Example:**

```
feat(auth): implement OAuth2 login

- Add OAuth2 client configuration
- Create login button component
- Set up token storage in local storage

Resolves: #123
```

**Types:**

- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code changes that neither fix bugs nor add features
- `perf`: Performance improvements
- `test`: Adding or updating tests
- `chore`: Changes to build process, dependencies, etc.

**Guidelines:**

1. **Atomic commits**: Each commit should represent a single logical change
2. **Present tense**: Use "add feature" rather than "added feature"
3. **Be descriptive**: The first line should be a clear summary
4. **Reference issues**: Mention related issues or tickets
5. **Keep it short**: First line should be under 50 characters, body lines under 72

**Real-world Example:**

```
fix(payment): correct credit card validation error

Previously, cards with dashes would fail validation even though
the numbers were correct. This updates the validation regex
to strip non-numeric characters before validating.

Fixes #237
```

## Branching & Merging

### Understanding Branches

Branches allow you to develop features, fix bugs, or experiment without affecting the main codebase.

**Basic Branch Operations:**

```bash
# List all branches
git branch

# Create a new branch
git branch feature-login

# Switch to a branch
git switch feature-login
# or (older syntax)
git checkout feature-login

# Create and switch to a new branch
git switch -c feature-login
# or (older syntax)
git checkout -b feature-login

# Delete a branch
git branch -d feature-login  # Safe delete (prevents if unmerged)
git branch -D feature-login  # Force delete
```

### Branching Strategies

Different teams use different branching strategies based on their needs:

**1. GitHub Flow**

- Simple, lightweight approach
- Main branch (master/main) should always be deployable
- Feature development happens in feature branches
- Pull requests for review before merging to main

**2. Git Flow**

- More structured approach
- Two main branches: `master` (production) and `develop` (integration)
- Feature branches branch off from `develop`
- Release branches prepare for production
- Hotfix branches handle emergency fixes

**3. Trunk-Based Development**

- Everyone commits to a single branch (trunk)
- Short-lived feature branches
- Heavy use of feature flags
- Frequent integration

**Example of GitHub Flow (most common for small-medium teams):**

```bash
# Start from main branch
git switch main
git pull origin main

# Create a feature branch
git switch -c feature-user-authentication

# Make changes, commit, and push
git add .
git commit -m "feat(auth): implement user login form"
git push -u origin feature-user-authentication

# Create pull request on GitHub (through the web interface)

# After approval and merge, update local main
git switch main
git pull origin main

# Delete the feature branch (locally and remotely)
git branch -d feature-user-authentication
git push origin --delete feature-user-authentication
```

### Merging Techniques

There are multiple ways to integrate changes from one branch to another:

**1. Basic Merge:**

```bash
# Switch to the target branch (where changes will go)
git switch main

# Merge the source branch
git merge feature-login

# This creates a merge commit if needed
```

**2. Fast-forward Merge:**

```bash
# Fast-forward merge (no merge commit, linear history)
git merge --ff-only feature-simple-fix

# Prevent fast-forward to create a merge commit always
git merge --no-ff feature-login
```

**3. Squash Merge:**

```bash
# Combine all commits from the branch into one
git merge --squash feature-login
git commit -m "feat: add login functionality"
```

### Resolving Conflicts

Conflicts occur when Git can't automatically merge changes:

```bash
# Merge a branch
git merge feature-sidebar

# If conflict occurs, Git will show which files have conflicts
git status

# Open conflicted files and resolve conflicts
# Look for markers like:
<<<<<<< HEAD
current branch code
=======
incoming branch code
>>>>>>> feature-sidebar

# After resolving conflicts
git add resolved-file.js
git commit  # Completes the merge
```

**Best Practices for Conflict Resolution:**

1. Understand both changes before resolving
2. Communicate with the team member whose code conflicts with yours
3. Consider using visual merge tools (`git mergetool`)
4. Test thoroughly after resolving conflicts
5. Keep merges small and frequent to minimize conflicts

## GitHub Collaboration

### Setting Up Authentication

To securely connect your local Git to GitHub, you'll need to set up authentication.

#### SSH Authentication

SSH (Secure Shell) is the recommended authentication method for Git:

**Setting up SSH:**

1. **Check for existing SSH keys:**

```bash
ls -la ~/.ssh
```

2. **Generate a new SSH key (if needed):**

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
# or for older systems:
ssh-keygen -t rsa -b 4096 -C "your.email@example.com"
```

3. **Start the SSH agent:**

```bash
eval "$(ssh-agent -s)"
```

4. **Add your SSH key to the agent:**

```bash
ssh-add ~/.ssh/id_ed25519
```

5. **Copy your public key to clipboard:**

```bash
# macOS
cat ~/.ssh/id_ed25519.pub | pbcopy

# Windows (git bash)
cat ~/.ssh/id_ed25519.pub | clip

# Linux
cat ~/.ssh/id_ed25519.pub | xclip -selection clipboard
```

6. **Add the SSH key to your GitHub account:**

   - Go to GitHub → Settings → SSH and GPG keys → New SSH key
   - Paste your key and save

7. **Test your connection:**

```bash
ssh -T git@github.com
```

#### Personal Access Tokens (PAT)

For HTTPS authentication or for use with GitHub API:

**Creating a PAT:**

1. Go to GitHub → Settings → Developer settings → Personal access tokens → Generate new token
2. Select the appropriate scopes (permissions):
   - For basic repository access: `repo` and `gist`
   - For read-only access: `repo:status`, `repo_deployment`, `public_repo`, `read:packages`
3. Generate token and copy it immediately (it won't be shown again)

**Using PAT with Git:**

```bash
# When you clone a repository, use HTTPS URL
git clone https://github.com/username/repository.git

# When prompted for password, use your PAT instead
# Or set it up in your credential manager
git config --global credential.helper store
# This saves credentials (not recommended on shared computers)

# On macOS, use keychain
git config --global credential.helper osxkeychain

# On Windows, use Windows Credential Manager
git config --global credential.helper wincred
```

### Remote Repository Management

**Adding a Remote Repository:**

```bash
# Add a remote (origin is the conventional name for the primary remote)
git remote add origin git@github.com:username/repository.git

# View remote repositories
git remote -v

# Add another remote (e.g., for a fork)
git remote add upstream git@github.com:original-owner/repository.git
```

**Syncing with Remote:**

```bash
# Download changes without integrating (fetch)
git fetch origin
git fetch upstream

# Download and integrate changes (pull)
git pull origin main

# Push local changes to remote
git push origin main

# Push and set upstream branch
git push -u origin feature-branch
```

**Real-world Example (Working with a Fork):**

```bash
# Clone your fork
git clone git@github.com:yourusername/project.git
cd project

# Add the original repository as upstream
git remote add upstream git@github.com:original-owner/project.git

# Keep your fork updated
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```

### Pull Requests

Pull Requests (PRs) are a GitHub feature that allow you to propose changes before merging them into the main codebase:

**Creating a Pull Request:**

1. **Push your branch to GitHub:**

```bash
git push -u origin feature-branch
```

2. **On GitHub:**
   - Navigate to your repository
   - Click "Pull requests" → "New pull request"
   - Select the base branch (where changes will go) and compare branch (your changes)
   - Click "Create pull request"
   - Add title and description

**Writing a good PR description:**

```markdown
## Description

Brief description of the changes made.

## Motivation and Context

Why these changes are needed and what problem they solve.

## How Has This Been Tested?

What tests did you run to verify your changes?

## Screenshots (if applicable)

Visual changes or UI updates.

## Types of changes

- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Checklist:

- [ ] My code follows the style guidelines
- [ ] I have performed a self-review
- [ ] I have added tests that prove my fix/feature works
- [ ] New and existing tests pass locally
```

### Code Reviews

Code reviews are crucial for maintaining code quality and sharing knowledge:

**Reviewing a Pull Request:**

1. **On GitHub:**
   - Navigate to the PR
   - Click "Files changed" to see all changes
   - Review the code and add inline comments by clicking the "+" button
   - Finish review with "Review changes" button

**Tips for effective code reviews:**

As a reviewer:

1. Be respectful and constructive
2. Look for:
   - Code correctness
   - Test coverage
   - Security issues
   - Performance concerns
   - Adherence to coding standards
3. Suggest alternatives when pointing out problems
4. Ask questions rather than making assumptions
5. Approve once all issues are addressed

As an author:

1. Make PRs manageable in size (< 300 lines when possible)
2. Describe your changes clearly
3. Be responsive to feedback
4. Address all comments before asking for re-review
5. Thank reviewers for their input

**Example PR Review Comment:**

```
In this function, we might have a race condition if multiple users try to update the same record simultaneously.
Consider using a transaction or optimistic locking pattern here.

```

## Real-world Workflows

### Feature Development

A typical workflow for developing a new feature:

```bash
# Start from an updated main branch
git switch main
git pull origin main

# Create feature branch
git switch -c feature/user-authentication

# Make changes and commit frequently
git add .
git commit -m "feat(auth): add login form component"
# ... more changes and commits

# Keep your feature branch updated with main
git switch main
git pull origin main
git switch feature/user-authentication
git merge main  # Resolve any conflicts

# Push your branch to GitHub
git push -u origin feature/user-authentication

# Create a pull request on GitHub

# After the PR is approved and merged:
git switch main
git pull origin main
git branch -d feature/user-authentication
```

### Bug Fixing

Workflow for fixing a bug:

```bash
# Create a bug fix branch from main
git switch main
git pull origin main
git switch -c fix/login-validation-issue

# Fix the bug and commit
git add .
git commit -m "fix(auth): correct email validation regex"

# Push and create PR
git push -u origin fix/login-validation-issue

# Create PR on GitHub
```

**Hotfix Workflow (for production bugs):**

```bash
# Create hotfix branch from production tag/branch
git switch production
git pull origin production
git switch -c hotfix/critical-security-issue

# Fix the bug and commit
git add .
git commit -m "fix(security): address SQL injection vulnerability"

# Push and create PR against production branch
git push -u origin hotfix/critical-security-issue

# After merge and deployment to production:
# Also merge the fix to main to keep branches in sync
git switch main
git pull origin main
git merge origin/hotfix/critical-security-issue
git push origin main
```

### Release Management

Managing releases in a professional environment:

```bash
# Create a release branch from develop (if using Git Flow)
git switch develop
git pull origin develop
git switch -c release/v1.2.0

# Make final adjustments, bug fixes, version updates
git add .
git commit -m "chore(release): bump version to 1.2.0"

# Push release branch
git push -u origin release/v1.2.0

# When ready to release:
# Merge to main/master
git switch main
git merge release/v1.2.0 --no-ff
git tag -a v1.2.0 -m "Version 1.2.0"
git push origin main --tags

# Also merge back to develop
git switch develop
git merge release/v1.2.0 --no-ff
git push origin develop

# Delete release branch
git branch -d release/v1.2.0
git push origin --delete release/v1.2.0
```

## Advanced Git Techniques

### Rebasing

Rebasing is a way to rewrite commit history by changing the base of your branch:

```bash
# Rebase your branch on top of main
git switch feature-branch
git rebase main

# Interactive rebase to clean up commits
git rebase -i HEAD~3  # Rebase last 3 commits
```

**When to use rebase:**

- When you want to maintain a clean, linear history
- Before merging a feature branch to update with the latest changes
- To clean up your commits before pushing to a remote

**When to avoid rebase:**

- After pushing to a shared branch (it rewrites history)
- When you're collaborating with others on the same branch

### Stashing

Stashing allows you to temporarily store changes without committing:

```bash
# Stash current changes
git stash

# List stashes
git stash list

# Apply and drop the latest stash
git stash pop

# Apply without dropping
git stash apply

# Stash with a message
git stash save "Work in progress on login page"

# Drop a specific stash
git stash drop stash@{1}

# Create a branch from a stash
git stash branch new-branch stash@{0}
```

**Real-world Example:**

```bash
# You're working on feature, but need to fix an urgent bug
git stash save "WIP: login feature"

# Switch to main and create bug fix branch
git switch main
git switch -c urgent-fix

# Fix the bug, commit, push, and create PR
git add .
git commit -m "fix: urgent database connection issue"
git push -u origin urgent-fix

# Once done, go back to your feature work
git switch feature-branch
git stash pop  # Resume your work
```

### Cherry-picking

Cherry-picking allows you to apply specific commits from one branch to another:

```bash
# Cherry-pick a single commit
git cherry-pick a1b2c3d

# Cherry-pick multiple commits
git cherry-pick a1b2c3d e4f5g6h

# Cherry-pick without committing
git cherry-pick -n a1b2c3d
```

**Real-world Example:**

```bash
# Scenario: A bug fix on one branch needs to be applied to another

# Identify the commit hash
git log --oneline feature-branch

# Switch to the target branch
git switch main

# Cherry-pick the bug fix commit
git cherry-pick 8e7d6c5  # The bug fix commit hash
```

### Undoing Changes

Different ways to undo changes in Git:

**Unstaging Files:**

```bash
# Unstage a file
git restore --staged file.js
```

**Discarding Working Directory Changes:**

```bash
# Discard changes in a file
git restore file.js

# Discard all changes
git restore .
```

**Undoing Commits:**

```bash
# Undo last commit but keep changes staged
git reset --soft HEAD^

# Undo last commit and unstage changes
git reset HEAD^  # or git reset --mixed HEAD^

# Undo last commit and discard changes (caution!)
git reset --hard HEAD^
```

**Creating a "Revert" Commit:**

```bash
# Create a new commit that undoes changes
git revert 72856ea

# Revert multiple commits
git revert HEAD~3..HEAD
```

**Amending the Last Commit:**

```bash
# Change the last commit message
git commit --amend -m "New commit message"

# Add more changes to the last commit
git add forgotten-file.js
git commit --amend --no-edit  # Keep the same message
```

## GitHub Best Practices

### Repository Structure

A well-organized repository improves collaboration and maintainability:

```
project-name/
├── .github/              # GitHub specific files
│   ├── ISSUE_TEMPLATE/   # Issue templates
│   └── workflows/        # GitHub Actions
├── docs/                 # Documentation
├── src/                  # Source code
├── tests/                # Test files
├── .gitignore            # Ignored files and directories
├── .editorconfig         # Editor configuration
├── LICENSE               # License information
└── README.md             # Project overview
```

**Best Practices:**

1. Keep related files together
2. Use consistent naming conventions
3. Include configuration files at the root level
4. Separate source code from documentation and tests
5. Create a comprehensive `.gitignore` file for your tech stack

### README and Documentation

A good README is essential for any project:

**README Structure:**

````markdown
# Project Name

Brief description of the project.

## Features

- Key feature 1
- Key feature 2
- Key feature 3

## Installation

```bash
npm install my-package
```
````

## Usage

```javascript
const myPackage = require("my-package");
myPackage.doSomething();
```

## Contributing

Guidelines for contributing to the project.

## License

MIT License. See `LICENSE` for more information.

````

**Additional Documentation:**
- `CONTRIBUTING.md`: Contribution guidelines
- `CODE_OF_CONDUCT.md`: Community standards
- `CHANGELOG.md`: Version history and changes
- `SECURITY.md`: Security policies and reporting vulnerabilities

### Issue Management

Effective issue templates help standardize bug reports and feature requests:

**Bug Report Template:**
```markdown
---
name: Bug Report
about: Create a report to help us improve
---

## Description
A clear and concise description of the bug.

## Steps to Reproduce
1. Go to '...'
2. Click on '....'
3. Scroll down to '....'
4. See error

## Expected Behavior
What you expected to happen.

## Actual Behavior
What actually happened.

## Screenshots
If applicable, add screenshots.

## Environment
- OS: [e.g. Windows 10]
- Browser: [e.g. Chrome 91]
- Version: [e.g. 1.2.3]
````

**Feature Request Template:**

```markdown
---
name: Feature Request
about: Suggest an idea for this project
---

## Problem Statement

A clear description of the problem this feature would solve.

## Proposed Solution

A clear description of what you want to happen.

## Alternatives Considered

Any alternative solutions or features you've considered.

## Additional Context

Add any other context or screenshots about the feature request here.
```

### GitHub Actions Basics

GitHub Actions automate workflows directly from your repository:

**Simple CI Workflow Example:**

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: "16"

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test
```

**Common GitHub Actions Use Cases:**

1. Running tests on pull requests
2. Building and deploying applications
3. Linting and code quality checks
4. Automated dependency updates
5. Release management

## Troubleshooting Common Issues

**Issue: I committed to the wrong branch**

```bash
# Save your changes
git stash

# Switch to the correct branch
git switch correct-branch

# Apply your changes
git stash pop

# Commit to the correct branch
git add .
git commit -m "Your commit message"
```

**Issue: I need to undo a pushed commit**

```bash
# Create a new commit that undoes the changes
git revert HEAD

# Push the revert commit
git push origin main
```

**Issue: I pushed sensitive information**

```bash
# Remove the sensitive info
git rm --cached sensitive-file
# or edit the file to remove sensitive content

# Commit the change
git commit -m "Remove sensitive information"

# Rewrite history to remove the sensitive data
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch sensitive-file" \
  HEAD

# Force push the changes
git push origin --force
```

**Issue: Git says 'pull isn't possible because you have unmerged files'**

```bash
# See which files are conflicting
git status

# Resolve conflicts in each file

# Stage resolved files
git add resolved-file.js

# Complete the merge
git commit
```

**Issue: I can't push because remote has changes**

```bash
# Fetch and merge remote changes
git pull origin main

# Resolve any conflicts

# Try pushing again
git push origin main
```

## Cheat Sheet Reference

### Creating Snapshots

```bash
# Initialize a repository
git init

# Stage files
git add file1.html            # Stage a single file
git add file1.js file2.js     # Stage multiple files
git add *.html                # Stage files with a pattern
git add .                     # Stage all files

# Status
git status                    # Full status
git status -s                 # Short status

# Committing
git commit -m "Message"       # Commit with a one-line message
git commit                    # Opens editor for message
git commit -am "Message"      # Stage tracked files and commit

# File operations
git rm file1.js               # Remove from working directory and staging
git rm --cached file1.js      # Remove from staging only
git mv file1.js file1.txt     # Rename/move files
```

### History and Inspection

```bash
# View log
git log                       # Full history
git log --oneline             # Compact view
git log --reverse             # Oldest to newest
git log --oneline --all --graph # Visual branch history

# Inspect commits
git show 921a2ff              # Show specific commit
git show HEAD                 # Show last commit
git show HEAD~2               # Two steps before last commit
git show HEAD:file.html       # Show file from last commit
```

### Undoing Changes

```bash
# Unstage files
git restore --staged file.js  # Unstage a file

# Discard changes
git restore file.css          # Discard changes in a file
git restore .                 # Discard all changes

# Restore previous versions
git restore --source=HEAD~2 file.html # Restore file from 2 commits ago

# Checkout commits
git checkout dad47ed          # Check out a specific commit
git checkout master           # Return to master branch
```

### Branching and Merging

```bash
# Branch management
git branch hotfix             # Create a branch
git switch hotfix             # Switch to the branch
git switch -c hotfix          # Create and switch
git branch -D hotfix          # Delete a branch

# Merging
git merge bugfix              # Merge bugfix into current branch
git merge --abort             # Abort a merge with conflicts

# View branches
git branch --merged           # Show merged branches
git branch --no-merged        # Show unmerged branches

# Rebasing
git rebase master             # Rebase current branch on master
```

### Collaboration

```bash
# Clone repositories
git clone url                 # Clone a repository

# Remote operations
git fetch origin master       # Fetch master from origin
git fetch origin              # Fetch all from origin
git fetch                     # Fetch from default remote
git pull                      # Fetch and merge
git push origin master        # Push master to origin
git push                      # Push current branch
git push -u origin hotfix     # Push and set upstream

# Remote management
git remote                    # Show remote repositories
git remote add upstream url   # Add a new remote
git remote rm upstream        # Remove a remote
```

Remember that Git is a powerful tool with many commands and options. This guide covers the most common operations, but there's always more to learn as you deepen your expertise.

Happy coding and collaborating!
