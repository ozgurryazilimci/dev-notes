# Git Cheatsheet

## Setup and Configuration

**List user name, user email and more:**

```bash
git config --global user.name
git config --global user.email
git config --global core.sshCommand
```

**Set user name:**

```bash
git config --global user.name "yourusername"
```

**Set user email:**

```bash
git config --global user.email "youremail@mail.com"
```

**Set core ssh command:**

```bash
git config --global core.sshCommand "ssh -i ~/.ssh/id_ed25519 -F /dev/null"
```

**Manage Personal Access Token info:**
[GitHub PAT Docs](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)

## Repository Initialization

**Initialize Git repository:**

```bash
git init
```

**Clone repository:**

- Via https:

```bash
git clone https://github.com/yourusername/yourrepo.git your-new-directory
```

- Via ssh:

```bash
git clone git@github.com:yourusername/yourrepo.git your-new-directory
```

## Staging and Committing

**Check the status of changes:**

```bash
git status
```

**Stage a file:**

```bash
git add README.md
```

**Stage multiple files:**

```bash
git add README.md index.html
```

**Stage all changes (excluding ignored):**

```bash
git add *
```

or

```bash
git add .
```

**Unstage a file:**

```bash
git rm --cached .vscode
```

**Unstage multiple files:**

```bash
git rm --cached -r .vscode/ bin/
```

**Unstage all files:**

```bash
git rm -r --cached .
```

**Commit with the message:**

```bash
git commit -m "initial commit"
```

**Amend last commit with staged changes:**

```bash
git commit --amend --all
```

**Change commit message:**

```bash
git commit -am "updated commit" --amend
```

## Branching and Merging

**List branches:**

```bash
git branch
```

**Create new branch:**

```bash
git branch feature-restructure
```

**Create the new branch and switch to it:**

```bash
git checkout -b feature-restructure
```

**Delete branch:**

```bash
git branch -D feature-restructure
```

**Rename branch:**

```bash
git branch -m feature-new-name
```

**Switch branch:**

```bash
git checkout $branch_name
```

**Merge branch into current branch:**

```bash
git merge feature-restructure
```

## Working with Commits

**View commit history:**

```bash
git log
```

**Checkout specific commit:**

```bash
git checkout $commit_id
```

**Hard reset to commit (WARNING: discards changes):**

```bash
git reset --hard $commit_id
```

**Soft reset to commit:**

```bash
git reset --soft HEAD~
```

**Unstage file with restore:**

```bash
git restore --staged index.html
```

**Discard changes in working directory:**

```bash
git restore index.html
```

## Remote Repositories

**Add remote URL:**

```bash
git remote add origin https://github.com/yourusername/yourrepo.git
```

**Remove remote URL:**

```bash
git remote remove origin
```

**Change remote URL:**

```bash
git remote set-url origin https://github.com/yourusername/yourrepo.git
```

**Show remote URL:**

```bash
git remote show origin
git remote get-url origin
git remote -v
```

## Pushing and Pulling

**Push to remote branch:**

```bash
git push origin master
```

**Push and set upstream:**

```bash
git push --set-upstream origin main
```

**Push after the upstream set:**

```bash
git push
```

**Pull with rebase:**

```bash
git pull --rebase origin master
```

**Set upstream branch for current branch:**

```bash
git branch --set-upstream-to=origin/master
```

**Pull after the upstream set:**

```bash
git pull
```

## Miscellaneous

- Skip CI workflow by adding `[skip ci]` in the commit message

  Add `[skip ci]` anywhere in your git commit message to skip CI workflows.

