# Git Commands Cheat Sheet

A reference table of common (and not-so-common) Git commands with their descriptions.

## Setup & Configuration

| Command | Description |
|---|---|
| `git init` | Initializes a new Git repository in the current directory |
| `git clone <url>` | Clones (downloads) a remote repository to your local machine |
| `git config --global user.name "<name>"` | Sets the global username for commits |
| `git config --global user.email "<email>"` | Sets the global email for commits |
| `git config --list` | Lists all Git configuration settings |
| `git config --global core.editor "<editor>"` | Sets the default text editor for Git |

## Staging & Committing

| Command | Description |
|---|---|
| `git status` | Shows the state of the working directory and staging area |
| `git add <file>` | Stages a specific file for commit |
| `git add .` | Stages all changed/new files in the current directory |
| `git add -p` | Interactively stages parts (hunks) of files |
| `git commit -m "<message>"` | Commits staged changes with a message |
| `git commit -am "<message>"` | Stages tracked files and commits in one step |
| `git commit --amend` | Modifies the most recent commit (message or content) |
| `git rm <file>` | Removes a file from the working directory and stages the removal |
| `git rm --cached <file>` | Unstages/removes a file from tracking without deleting it locally |
| `git mv <old> <new>` | Renames or moves a file and stages the change |

## Branching & Merging

| Command | Description |
|---|---|
| `git branch` | Lists all local branches |
| `git branch <name>` | Creates a new branch |
| `git branch -d <name>` | Deletes a branch (safe, only if merged) |
| `git branch -D <name>` | Force-deletes a branch (even if unmerged) |
| `git checkout <branch>` | Switches to the specified branch |
| `git checkout -b <branch>` | Creates a new branch and switches to it |
| `git switch <branch>` | Switches to the specified branch (modern alternative to checkout) |
| `git switch -c <branch>` | Creates and switches to a new branch |
| `git merge <branch>` | Merges the specified branch into the current branch |
| `git merge --abort` | Aborts a merge in progress due to conflicts |
| `git rebase <branch>` | Reapplies commits from current branch on top of another branch |
| `git rebase -i <commit>` | Starts an interactive rebase (edit/squash/reorder commits) |
| `git rebase --abort` | Aborts an in-progress rebase |
| `git rebase --continue` | Continues a rebase after resolving conflicts |
| `git cherry-pick <commit>` | Applies a specific commit from another branch onto the current branch |

## Remote Repositories

| Command | Description |
|---|---|
| `git remote -v` | Lists remote repositories linked to the local repo |
| `git remote add <name> <url>` | Adds a new remote repository |
| `git remote remove <name>` | Removes a remote repository link |
| `git fetch` | Downloads changes from the remote without merging them |
| `git fetch --all` | Fetches updates from all remotes |
| `git pull` | Fetches and merges changes from the remote into the current branch |
| `git pull --rebase` | Fetches changes and rebases local commits on top instead of merging |
| `git push` | Uploads local commits to the remote repository |
| `git push -u origin <branch>` | Pushes a branch and sets it to track the remote branch |
| `git push --force` | Force-pushes local commits, overwriting remote history |
| `git push --force-with-lease` | Safer force-push; fails if remote has new commits you don't have |
| `git push origin --delete <branch>` | Deletes a branch on the remote repository |

## Viewing History & Differences

| Command | Description |
|---|---|
| `git log` | Shows the commit history |
| `git log --oneline` | Shows commit history in a condensed, one-line-per-commit format |
| `git log --graph --oneline --all` | Shows a visual graph of branch/commit history |
| `git show <commit>` | Shows details and changes of a specific commit |
| `git diff` | Shows unstaged changes between working directory and last commit |
| `git diff --staged` | Shows staged changes not yet committed |
| `git diff <branch1> <branch2>` | Shows differences between two branches |
| `git blame <file>` | Shows who last modified each line of a file and when |

## Undoing Changes

| Command | Description |
|---|---|
| `git reset <file>` | Unstages a file, keeping changes in the working directory |
| `git reset --soft <commit>` | Moves HEAD to a commit, keeping changes staged |
| `git reset --mixed <commit>` | Moves HEAD to a commit, keeping changes unstaged (default mode) |
| `git reset --hard <commit>` | Moves HEAD to a commit, discarding all changes permanently |
| `git revert <commit>` | Creates a new commit that undoes the changes of a specific commit |
| `git checkout -- <file>` | Discards changes in a file, restoring it to the last committed state |
| `git restore <file>` | Restores a file in the working directory (modern alternative to checkout) |
| `git restore --staged <file>` | Unstages a file (modern alternative to reset) |
| `git clean -f` | Removes untracked files from the working directory |
| `git clean -fd` | Removes untracked files and directories |

## Stashing

| Command | Description |
|---|---|
| `git stash` | Temporarily saves uncommitted changes for later use |
| `git stash list` | Lists all stashed changesets |
| `git stash pop` | Applies the most recent stash and removes it from the stash list |
| `git stash apply` | Applies a stash without removing it from the stash list |
| `git stash drop` | Deletes a specific stash |
| `git stash clear` | Deletes all stashes |
| `git stash -u` | Stashes changes including untracked files |

## Tags

| Command | Description |
|---|---|
| `git tag` | Lists all tags |
| `git tag <name>` | Creates a lightweight tag at the current commit |
| `git tag -a <name> -m "<message>"` | Creates an annotated tag with a message |
| `git push origin <tag>` | Pushes a specific tag to the remote |
| `git push origin --tags` | Pushes all local tags to the remote |
| `git tag -d <name>` | Deletes a local tag |

## Inspecting & Searching

| Command | Description |
|---|---|
| `git grep "<pattern>"` | Searches for a pattern in tracked files |
| `git show-branch` | Shows branches and their commits |
| `git reflog` | Shows a log of all HEAD movements (useful for recovering lost commits) |
| `git bisect start` | Begins a binary search to find the commit that introduced a bug |

## Submodules

| Command | Description |
|---|---|
| `git submodule add <url>` | Adds a submodule (a repo within a repo) |
| `git submodule update --init --recursive` | Initializes and updates all submodules |

## Miscellaneous

| Command | Description |
|---|---|
| `git help <command>` | Shows help documentation for a specific Git command |
| `git archive` | Creates a zip/tar archive of files from a specific commit or branch |
| `git worktree add <path> <branch>` | Creates a new working directory linked to the same repository |
