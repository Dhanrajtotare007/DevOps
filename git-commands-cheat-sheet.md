# Git Commands Cheat Sheet

### Practical commands for everyday development, QA automation, and DevOps work

A quick reference for working with Git repositories locally and
collaborating through GitHub or another remote Git platform.

> Replace example URLs, branch names, and file names with your own.

## 1. Configure Git (one-time setup)

``` bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --list
```

Use the email associated with your GitHub account if you want your
commits linked to that account.

## 2. Start a repository

``` bash
git --version
mkdir my-project
cd my-project
git init
git clone https://github.com/USERNAME/REPOSITORY.git
git clone https://github.com/USERNAME/REPOSITORY.git my-folder
```

## 3. Check status, branches, and history

``` bash
git status
git branch
git branch -a
git log --oneline
git log --oneline --graph --decorate --all
git diff
git diff --staged
```

-   `git status`: shows the current branch and working-tree changes.
-   `git diff`: shows unstaged changes.
-   `git diff --staged`: shows changes staged for the next commit.

## 4. Stage and commit changes

``` bash
git add README.md
git add .
git add -A
git restore --staged README.md
git commit -m "Add project documentation"
git commit --amend -m "Update project documentation"
```

**Good practice:** Make focused commits and write messages that explain
the change, such as `Add login API test cases` or
`Fix Docker build path`.

## 5. Work with branches

``` bash
git branch feature/login
git switch feature/login
git switch -c feature/login
git switch main
git branch -m main
git branch -m master main
git branch -d feature/login
```

`git branch -m master main` renames a local branch named `master` to
`main`. If you are already on `master`, `git branch -m main` is
sufficient. Renaming locally does not automatically rename the remote
branch.

## 6. Connect to a remote repository

``` bash
git remote -v
git remote add origin https://github.com/USERNAME/REPOSITORY.git
git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
git remote show origin
```

## 7. Fetch, pull, and push

``` bash
git fetch origin
git pull
git pull origin main
git push -u origin main
git push
```

-   `git fetch` downloads remote updates but does not integrate them
    into your current branch.
-   `git pull` fetches and integrates updates into your current branch.
-   `git push` sends your local commits to the remote repository.
-   `-u` sets the upstream tracking branch, so later pushes can usually
    use `git push`.

## 8. Merge branches

``` bash
git switch main
git merge feature/login
git merge --abort
```

Switch to the branch that should receive the changes before running
`git merge`.

## 9. Undo and recover changes carefully

``` bash
git restore README.md
git restore --staged README.md
git revert COMMIT_ID
git reset --soft HEAD~1
git reset --mixed HEAD~1
git reflog
```

-   `git restore README.md` discards unstaged changes in that file.
-   `git restore --staged README.md` unstages the file but keeps its
    edits.
-   `git revert COMMIT_ID` creates a new commit that reverses an earlier
    commit.
-   `git reset --soft HEAD~1` moves HEAD back one commit and keeps
    changes staged.
-   `git reset --mixed HEAD~1` moves HEAD back one commit and keeps
    changes unstaged.
-   `git reflog` shows recent movements of HEAD and can help locate
    previous states.

**Caution:** Avoid destructive commands such as `git reset --hard`
unless you understand exactly what work will be discarded. Prefer
`git revert` for undoing commits that have already been shared.

## 10. Resolve merge conflicts

``` bash
git status
# Edit conflicted files and remove conflict markers
git add path/to/resolved-file
git commit
```

A conflicted file may contain `<<<<<<<`, `=======`, and `>>>>>>>`
markers. Edit it to the correct final content, stage the resolved file,
and complete the merge. Use `git merge --abort` if you need to cancel
the merge.

## 11. Remove and ignore files

``` bash
git rm --cached .env
git rm old-file.txt
git status --ignored
```

Example `.gitignore`:

``` gitignore
# Secrets and local environment files
.env
.env.*

# Logs
*.log

# Python cache
__pycache__/
*.py[cod]

# Node dependencies and build output
node_modules/
dist/
build/

# IDE settings
.vscode/
.idea/
```

Never commit passwords, access tokens, private keys, or other secrets.
If a secret is committed, deleting it in a later commit does not make it
safe; revoke or rotate it.

## 12. Useful inspection commands

``` bash
git blame README.md
git show COMMIT_ID
git log --grep="login"
git grep "TODO"
git ls-files
```

-   `git blame`: shows who last changed each line.
-   `git show`: displays details of a commit.
-   `git log --grep`: searches commit messages.
-   `git grep`: searches tracked files.
-   `git ls-files`: lists files tracked by Git.

## 13. A common end-to-end workflow

``` bash
# Get the latest main branch
git switch main
git pull origin main

# Create a branch for your work
git switch -c feature/add-login-tests

# Make changes, then inspect them
git status
git diff

# Stage and commit
git add .
git commit -m "Add login automation tests"

# Push your branch
git push -u origin feature/add-login-tests
```

Then open GitHub, create a Pull Request, explain the changes, request a
review, and merge after the required checks and approvals.

## 14. How Git helps QA and DevOps teams

**Automation QA** - Version-control test scripts, framework code, and
configuration examples. - Use branches for new test coverage and bug
fixes. - Review test changes through Pull Requests. - Keep credentials
and environment-specific secrets out of repositories.

**DevOps** - Version-control Dockerfiles, pipeline definitions,
deployment manifests, and infrastructure-as-code. - Review
infrastructure and pipeline changes before merging. - Tag releases and
inspect history when investigating changes. - Store secrets in an
approved secret manager or CI/CD secret store, not in Git.

## Quick reference

  Command                       Purpose
  ----------------------------- ----------------------------------------
  `git status`                  Check working-tree status
  `git add .`                   Stage changes in the current directory
  `git commit -m "message"`     Commit staged changes
  `git switch -c branch-name`   Create and switch to a branch
  `git fetch origin`            Download remote updates
  `git pull`                    Fetch and integrate remote updates
  `git push`                    Push local commits to a remote
  `git log --oneline`           View compact commit history
  `git diff`                    Inspect unstaged changes
  `git diff --staged`           Inspect staged changes
  `git revert COMMIT_ID`        Reverse a commit with a new commit

**Practice tip:** Try these commands in a small practice repository
before using them in a work repository. Check `git status` often,
especially before committing or changing history.
