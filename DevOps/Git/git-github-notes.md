# Git & GitHub Notes

## 1. What is Git?

Git is a **Version Control System (VCS)** — a tool that helps track changes in code.

Think of it like a bank statement: it records every credit, debit, and detail about your account over time. Similarly, a version control system tracks the complete history of a project — when a file was added, when it was deleted, what line was changed, what was updated.

### Why use Git?
- **One of the most popular VCS in the world** — used not just for personal projects but by companies worldwide (e.g., used during a Microsoft internship for managing code).
- **Free and open source** — anyone can use it without paying.
- **Fast and scalable** — works well for both small and large-scale projects.

### Two primary uses of Git
1. **Track history** — Lets you go back to any previous state of your code. For example, if you added a "Sign Up" feature, then some buttons, then a "Help" form that isn't working out, you can revert back to the state right after the buttons were added — without manually deleting files/lines.
2. **Collaborate** — In companies, many developers work on the same project. Git helps track who changed what, and prevents developers from overwriting each other's changes.

Git is not just a tool — it's a **skill** every developer should have.

---

## 2. What is GitHub?

- **Git** = a software/tool that runs on your local computer.
- **GitHub** = a **website** (github.com) that lets developers store and manage their code online using Git.

You upload your project's code to GitHub, generally as a folder — this folder is called a **repository** (repo).

When applying for jobs/internships, you typically share your GitHub link in your resume so recruiters can verify your project work.

You can also browse other people's repositories, copy them, and add your own changes.

---

## 3. Setting up a GitHub Account

1. Go to github.com and click **Sign up**.
2. Enter your email — prefer using a **personal email** (not a college email that may expire after graduation).
3. Set a password, choose a username.
4. Verify you're human, verify your email with the code sent.
5. Answer a few basic onboarding questions (e.g., "I'm a student").
6. Choose the **Free** plan to finish setup.

Your GitHub profile shows:
- **Overview** — activity graph (green squares = days you contributed).
- **Repositories** — your projects.

### Creating your first repository
1. Go to Repositories → **New**.
2. Give it a name (e.g., `apna-college-demo`) and an optional description.
3. Choose **Public** (visible to everyone) or **Private** (visible only to you, like private Instagram stories).
4. Check **"Initialize this repository with a README"**.
   - README.md is a special file containing project details: what the project is, how to use it, why it was built, features, etc.
5. Click **Create repository**.

---

## 4. Commits on GitHub (Web UI)

- Any change you make and save is called a **commit** — think of it like taking a "screenshot" of the change and storing it in memory.
- Analogy: committing is like a relationship's commitment process — first **engagement** (staging/adding a change), then **marriage** (committing it).
  - **Add** = marking a change as ready to be committed.
  - **Commit** = finalizing the change (the actual "screenshot").
- On the GitHub website, the **Add** step is skipped — you directly commit changes.
- Each file shows its most recent commit message next to it.
- README files use **Markdown** syntax (basic HTML also works) for formatting — e.g., `<br>` for a new line.

---

## 5. Installing Git Locally

### Tools needed
- **VS Code** — a free, open-source code editor by Microsoft, supports almost all languages (Python, Java, C/C++, HTML/CSS/JS, etc.). Download from code.visualstudio.com based on your OS.
- **Git Bash** (Windows) — download from git-scm.com.
- **Terminal** (Mac) — comes pre-installed; search "Terminal" in Finder.

### Verify installation
Run in your terminal:
```
git --version
```
If it returns a version number, Git is already installed.

### Installing Git on Windows
1. Go to git-scm.com → Windows → download the installer.
2. Run installer → click **Next** through the setup screens (keep most settings default).
3. Enable the **"Add a Git Bash Profile to Windows Terminal"** / desktop icon option if you want.
4. On the "initial branch name" screen, you can set the default branch name to `main`.
5. Choose **"Git from the command line and also from 3rd-party software"** (or similar "Use Git and optional Unix tools" option).
6. Continue clicking Next → **Install** → **Finish**.

### Basic terminal commands to know
- `git --version` — check Git version.
- `ls` — list files in current directory.
- `ls -a` — list **all** files, including hidden ones (like `.git`).
- `clear` — clear the terminal screen.
- `pwd` — print working directory (shows your current folder path).
- `cd <folder>` — change directory (move into a folder).
- `cd ..` — move out to the parent directory.
- `mkdir <name>` — make a new directory/folder.

---

## 6. Configuring Git

Before using Git, tell it which account/identity to associate commits with.

```
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

- `--global` — applies this config to **all** repositories on your system.
- **Local** config (without `--global`, run inside a specific repo folder) applies only to that one project — useful if you use different accounts for different projects.

To view your current config:
```
git config --list
```

This info (username, email) is Git's **credentials helper** data.

> Tip: It's often easier to work with Git inside a code editor like VS Code (Terminal → New Terminal) rather than a standalone terminal, since your code and Git workflow stay in the same place.

---

## 7. Core Git Commands

### `git clone` — copy a remote repo to your local machine
- **Remote** = the repository on GitHub.
- **Local** = your own laptop/computer.
- Clone duplicates a GitHub repository onto your local system.

```
git clone <repository-https-link>
```
- Get the link from GitHub → **Code** button (green) → copy the **HTTPS** URL.
- HTTPS is the recommended/easiest method for beginners (SSH is an alternative, not covered here).

After cloning, `cd <repo-folder>` to move into it. Inside, you'll see a hidden `.git` folder — this marks that Git is tracking this folder's history.

### `git status` — check current state
```
git status
```
Shows:
- Which branch you're on.
- Whether everything is up to date ("nothing to commit").
- Any modified/untracked files.

### The 4 file states in Git
1. **Untracked** — a new file Git doesn't know about yet (never added/committed).
2. **Modified** — an existing tracked file that has been changed.
3. **Staged** — a file that has been `add`-ed and is ready to be committed (the "engagement" stage).
4. **Unmodified / Committed** — no pending changes; everything is recorded.

### `git add` — stage changes
Adds new/changed files from your working directory into Git's **staging area**.

```
git add <filename>       # stage one file
git add .                # stage all changed/new files
```

### `git commit` — finalize/record changes
```
git commit -m "meaningful message describing the change"
```
- The `-m` flag lets you write a commit message inline.
- Use meaningful messages, e.g., `"Add new button"`, `"Fix login bug"`.
- After committing, `git status` will say your branch is "ahead of origin/main by N commits" — meaning your local repo has commits not yet on GitHub.

### `git push` — upload local commits to GitHub
```
git push origin main
```
- **origin** = the default nickname for your remote GitHub repository (the one you cloned from).
- **main** = the branch you're pushing to.
- First time pushing may prompt you to authorize VS Code / GitHub — click **Allow** / **Authorize**.

### `git pull` — download remote changes to local
```
git pull origin main
```
Fetches and downloads content from the remote repo and merges it into your local repo. Useful when changes were made on GitHub (e.g., a merged pull request) that aren't yet on your machine.

### `git init` — start a new Git repo locally
Used when you start a project on your own machine first (instead of starting from GitHub).
```
git init
```
This creates a `.git` folder, turning the current folder into a Git repository.

### Connecting a local repo to a new GitHub repo
1. Create a new (empty) repository on GitHub — don't initialize with a README if you're pushing existing local code.
2. Link it locally:
```
git remote add origin <repository-link>
```
   - `git remote add` — adds a new remote connection.
   - `origin` — the name given to this remote (convention; could be anything).
3. Verify the remote:
```
git remote -v
```
4. Push your code, setting the upstream so future pushes are simpler:
```
git push -u origin main
```
   - `-u` sets the **upstream**, so afterward you can simply run `git push` without specifying `origin main` every time.

### Typical Git workflow (recap)
1. Create/clone a repo.
2. Make changes.
3. `git add` the changes.
4. `git commit` the changes.
5. `git push` to GitHub.

---

## 8. Branches

A **branch** is a separate copy/line of development in a project. Just like a tree has branches, a Git project's main line of code can branch off for different features or teams (e.g., a frontend team, a backend team, a bug-fix team) to work independently without waiting on each other. Later, branches are **merged** back into the main line.

- Older default branch name: **master**. Since "master" was considered to have a negative connotation, GitHub changed the default branch name to **main**.

### Branch commands
```
git branch                     # list branches / show current branch
git branch -m <new-name>       # rename current branch
git checkout <branch-name>     # switch to another branch
git checkout -b <new-branch>   # create AND switch to a new branch
git branch -d <branch-name>    # delete a branch (must not be currently on it)
```

### Why use branches?
So multiple developers can work on different features simultaneously without blocking or overwriting each other, then merge their work into the main branch once done.

---

## 9. Merging Branches

Two ways to merge:

### Method 1: Via command line
```
git diff <branch-name>     # compare current branch with another
git merge <branch-name>    # merge the specified branch into the current branch
```

### Method 2: Via GitHub — Pull Requests (PR)
A **Pull Request** lets you tell others about changes you've pushed to a branch, so they can review before merging into the main branch.

Steps:
1. Push your feature branch: `git push origin <branch-name>`.
2. On GitHub, click **"Compare & pull request"**.
3. Add a title/message describing the change.
4. GitHub checks if it can auto-merge (shown in green if no conflicts).
5. Click **Create pull request**, then **Merge pull request** → **Confirm merge**.
6. This creates a new commit (e.g., "Merge pull request...") recording the merge.
7. After merging on GitHub, run `git pull origin main` locally to bring those changes down to your machine.

In real teams, a senior developer / project manager typically reviews PRs before approving the merge, and may leave comments requesting changes.

---

## 10. Merge Conflicts

A **merge conflict** happens when Git is unable to automatically resolve differences — e.g., when two branches change the **same line(s)** of the **same file** differently. Git doesn't know which version to keep, so it needs manual resolution.

### Resolving conflicts (in VS Code)
1. Run `git merge <branch-name>` — VS Code will flag the conflicting file(s).
2. VS Code shows options: **Accept Current Change**, **Accept Incoming Change**, **Accept Both Changes**, etc.
   - **Current change** = what's in your current branch.
   - **Incoming change** = what's coming from the branch being merged in.
3. Remove the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`) and keep the code you want.
4. Save the file, then:
```
git add .
git commit -m "resolved merge conflict message"
```
5. Push the resolved changes: `git push`.

---

## 11. Undoing Changes

### Case 1: Undo staged (added but not committed) changes
```
git reset <filename>   # unstage a specific file
git reset              # unstage all staged changes
```
This moves files back from "staged" to "modified" (not yet added).

### Case 2: Undo the last commit (keep changes as uncommitted)
```
git reset HEAD~1
```
- `HEAD` = the reference to your latest commit.
- `HEAD~1` = go back one commit from HEAD. Moves your last commit's changes back to modified/unstaged state.

### Case 3: Go back to a specific earlier commit
1. Find the commit's hash using:
```
git log
```
2. Copy the desired commit's hash, then:
```
git reset <commit-hash>
```
This resets HEAD to that commit; changes made after it become unstaged (still present in your files).

### Case 4: Completely discard changes made after a commit (hard reset)
```
git reset --hard <commit-hash>
```
`--hard` removes the changes from your working files too (in VS Code/local files), not just from Git tracking. Use with caution — this is destructive.

### Viewing commit history
```
git log
```
Shows all past commits with messages. Press `q` to quit the log view.

---

## 12. Forking

**Forking** creates your own copy of someone else's repository (with the same code and settings) under your own GitHub account. It's essentially a "rough copy" of a project.

### Why fork?
- To contribute to **open source** projects.
- To work on a company's or a friend's repository without direct write access.

### How to fork
1. Search for the repository on GitHub (e.g., search "express" for the Express.js repo).
2. Click the **Fork** button.
3. Choose to copy just the main branch or the whole project.
4. Click **Create fork**.

The forked repo appears under your own account with all the original code and README.

### Contributing back
- After making **useful** changes (bug fixes, new features) in your fork, you can open a **Pull Request** to the original repository, asking the owner to merge your changes.
- Avoid creating unnecessary/unhelpful pull requests — some repo owners restrict PR creation due to spam/irrelevant PRs.

---

## Quick Command Reference

| Command | Purpose |
|---|---|
| `git --version` | Check Git version |
| `git config --global user.name "name"` | Set global username |
| `git config --global user.email "email"` | Set global email |
| `git config --list` | View current config |
| `git clone <url>` | Clone a remote repo locally |
| `git init` | Initialize a new local repo |
| `git status` | Check current state of files |
| `git add <file>` / `git add .` | Stage file(s) |
| `git commit -m "message"` | Commit staged changes |
| `git push origin <branch>` | Push local commits to remote |
| `git push -u origin <branch>` | Push + set upstream |
| `git pull origin <branch>` | Pull remote changes to local |
| `git remote add origin <url>` | Link local repo to a remote |
| `git remote -v` | Verify remote URL |
| `git branch` | List branches / show current |
| `git branch -m <name>` | Rename current branch |
| `git checkout <branch>` | Switch branches |
| `git checkout -b <branch>` | Create + switch to new branch |
| `git branch -d <branch>` | Delete a branch |
| `git diff <branch>` | Compare current branch with another |
| `git merge <branch>` | Merge a branch into current branch |
| `git reset <file>` | Unstage a file |
| `git reset HEAD~1` | Undo last commit (keep changes) |
| `git reset <hash>` | Reset to a specific commit (soft) |
| `git reset --hard <hash>` | Reset to a commit and discard changes |
| `git log` | View commit history |
