# Git and GitHub collaboration guide

This repository provides a practical guide for collaborating using GitHub. It walks through repository setup, branching, committing, syncing, pull requests, code review, protected branches, and merging workflows using Git CLI and GitHub web interfaces.

## Table of contents

- [Requirements](#requirements)
- [Summary of core Git concepts](#summary-of-core-git-concepts)
- [Collaboration workflow summary](#collaboration-workflow-summary)
- [Initial setup](#initial-setup)
- [Making and tracking code changes](#making-and-tracking-code-changes)
- [Syncing with main](#syncing-with-main)
- [Opening a pull request](#opening-a-pull-request)
- [Code review: approve or request changes](#code-review-approve-or-request-changes)
- [Merging a pull request](#merging-a-pull-request)
- [Enable branch protection rules](#enable-branch-protection-rules)
- [Keeping your local repo updated](#keeping-your-local-repo-updated)
- [References](#references)
- [Contributions](#contributions)
- [License](#license)

## Requirements

### 1. GitHub account

You’ll need a GitHub account to host repositories, collaborate, and use GitHub Desktop.

Create an account at [github.com/join](https://github.com/join).

### 2. Git CLI (Command Line)

If you don't already have Git installed, download it from [git-scm.com/downloads](https://git-scm.com/downloads).

To verify installation, run in your Terminal app:

```bash
git --version
```

### 3. Text editor

You’ll use an editor to write and modify your code. We'll use VS Code here.

- Download [VS Code](https://code.visualstudio.com/download)

## Git and GitHub core concepts summary

| Action | Git CLI | Description |
|-|-|-|
| Create a repo | GitHub Web | Create a project workspace |
| Clone a repo | `git clone <URL>` | Copy a remote repo to your computer |
| Check current status | `git status` | View file changes, branch, and staging state |
| Create branch | `git checkout -b feature/xyz` | Start isolated work |
| Stage changes | `git add .` | Mark files for commit |
| Commit changes | `git commit -m "desc"` | Save your work snapshot |
| Push branch | `git push -u origin feature/xyz` | Upload branch to GitHub |
| Pull main updates | `git pull origin main` | Sync with latest code |
| Merge main into branch | `git merge main` | Update feature branch |
| Submit pull request | GitHub Web | Ask for review |
| Review pull request | GitHub Web | Approve or request changes |
| Merge pull request | GitHub Web | Add changes to main |

## Collaboration workflow summary

```
dev-A         dev-B
  |             |
  |-> create branch
  |             |
  |-> make changes
  |-> push + PR |
  |             |-> review PR
  |             |-> request changes
  |<- rework    |
  |-> update PR |
  |             |-> approve + merge
```

## Initial setup

### 1. Create a repository

1. In your web browser go to [github.com](https://github.com)
2. Click **New Repository**
3. Set:
   - Name: `project-repo`
   - Visibility: Public or Private
   - Check “Add a README file”
4. Click **Create Repository**

### 2. Clone the repository to your computer

On your repo page:
1. Click **Code**
2. Select the **Local** > **HTTPS** tab
3. Copy the URL

In your Terminal app, change to a folder to hold your GitHub repos. Then run:

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
```

### 3. Configure Git identity

> [!NOTE]
> This step is required once per computer to identify your code commits to local and remote repos.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

## Making and tracking code changes

### 1. Create a new branch

Create a new branch to add features or make changes.

```
git checkout -b feature/my-feature
```
…or for a change…

```
git checkout -b change/my-change
```

### 2. Make code changes

Use your editor to make and save code changes.

> [!TIP]
> Making, staging, and commiting frequent and small changes provides easier change tracking and rollback.

### 3. Check status with Git

> [!TIP]
> Use `git status` frequently to understand which files are modified, staged, or untracked.

```
git status
```

Example Output:
```
On branch feature/my-change
Changes not staged for commit:
  modified: script.py

Untracked files:
  newfile.txt
```

### 4. Stage and commit changes

**Staging** is the process of selecting which changed files you want to include in your next commit. You use `git add` to move changes from your working directory to the staging area.

- `git add .` stages all files in the current directory.
- `git add <filename>` stages a specific file.

**Committing** saves the staged changes as a new snapshot in your repository’s history. You use `git commit` to record these changes with a message describing what you did.

- Write commit messages with present tense. For example: Add, not Added.

```
git add .
git status
git commit -m "Add: implemented feature X"
```

Example Output:
```
[feature/my-feature 123abcd] Add: implemented feature X
 1 file changed, 20 insertions(+)
```

### 5. Push your branch

```
git push -u origin feature/my-change
```

## Sync your feature branch with the main branch

Before you push your feature branch and open a pull request, it's best practice to sync your branch with the latest changes from `main`. This helps prevent merge conflicts and ensures your work is compatible with the most recent codebase. Pull the latest changes from `main` into your feature branch, resolve any conflicts, and test your updates.

```
git checkout main
git pull origin main
git checkout feature/my-change
git merge main
git status
```

## Opening a pull request

A pull request, or PR, is a way to propose changes you've made in your branch to be merged into another branch (usually the main branch) of a repository. It allows others to review your code, discuss improvements, and approve or request changes before the code becomes part of the main project.

Pull requests must be created using the GitHub web interface. This ensures all required information is filled out and the review process is properly tracked.

1. Go to your repository on GitHub
2. Click the **Pull requests** tab
3. Click the **New pull request** button
3. Fill in:
   - Title (e.g., `Add: feature X`)
   - Description (what/why of the change)
4. Assign your teammate as reviewer
5. Click **Create pull request**

## Code review: Approve or request changes

### Approving a pull request

1. Navigate to the PR
2. Click **Files changed**
3. Click **Review changes**
4. Leave a constructive comment
5. Click **Approve** > **Submit review**

### Requesting Changes

1. Click on a line of code
2. Leave a constructive comment
3. Click **Review changes > Request changes > Submit**

> [!IMPORTANT]
> The PR cannot be merged until all requested changes are resolved.

## Merging a Pull Request

> [!IMPORTANT]
> Only merge a PR after it’s been approved by a reviewer.

1. Open the PR on GitHub
2. Click **Merge pull request**
3. Confirm the merge
4. Click **Delete branch** (recommended) if it's not already been done automatically by a branch rule

## Enable branch protection rules

Branch protection rules help enforce collaboration best practices by requiring code reviews and preventing direct pushes to important branches like `main`. Enabling these rules ensures that all changes are reviewed and approved before being merged, reducing the risk of errors and maintaining code quality.

1. Go to GitHub repo > **Settings > Branches**
2. Click **Add rule**
3. Configure:
   - Branch name pattern: `main`
   - [✓] Require pull request before merging
   - [✓] Require at least 1 approving review
   - [✓] Dismiss stale approvals
   - [✓] (Optional) Require linear history
4. Click **Create**

## Keeping Your Local Repo Updated

#### Using Git CLI
```
git checkout main
git pull origin main
git status
```

## References

- [GitHub Docs](https://docs.github.com)
    - [Quickstart for repositories](https://docs.github.com/en/repositories/creating-and-managing-repositories/quickstart-for-repositories)
    - [Pull requests](https://docs.github.com/en/pull-requests)
    - [Branch Protection Rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-a-branch-protection-rule)
  - Git 
    - Simple Git Guide: https://rogerdudler.github.io/git-guide/
    - Git Book: https://git-scm.com/book/en/v2
- Markdown formatting
    - GitHub Alert Syntax: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#alerts
    - Markdown Guide: https://www.markdownguide.org/


## Contributions

Contributions are welcome — fork the repo, create a branch, and open a pull request.

Learn more at GitHub’s [Contributing to a project](https://docs.github.com/en/get-started/exploring-projects-on-github/contributing-to-a-project).


## License

MIT [LICENSE](LICENSE.md)  
More details: [Choose an open source license](https://choosealicense.com/licenses/mit/)