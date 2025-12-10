
The core concept to remember is the File Lifecycle:

Working Dir $\rightarrow$ git add $\rightarrow$ Staging Area $\rightarrow$ git commit $\rightarrow$ Local Repo $\rightarrow$ git push $\rightarrow$ Remote Repo

### **1. Setup & Initialization**

Start here when setting up a new project or machine.

| **Command**                      | **Description**                                  | **Example**                                        |
| -------------------------------- | ------------------------------------------------ | -------------------------------------------------- |
| `git init`                       | Initialize a new git repo in the current folder. | `git init`                                         |
| `git clone <url>`                | Copy a remote repo to your local machine.        | `git clone https://github.com/...`                 |
| `git config --global user.name`  | Set your username for commits.                   | `git config --global user.name "Your Name"`        |
| `git config --global user.email` | Set your email for commits.                      | `git config --global user.email "you@example.com"` |

### **2. The Daily Cycle (Stage & Commit)**

These are the commands you will use 90% of the time.

| **Command**           | **Description**                                               | **Example**                     |
| --------------------- | ------------------------------------------------------------- | ------------------------------- |
| `git status`          | **Check status.** Shows changed, staged, and untracked files. | `git status`                    |
| `git add <file>`      | **Stage files.** Prepares file for commit.                    | `git add index.html`            |
| `git add .`           | Stage **all** changed files in the directory.                 | `git add .`                     |
| `git commit -m "msg"` | **Commit.** Saves the snapshot to history with a message.     | `git commit -m "Fix login bug"` |
| `git diff`            | Show changes between working dir and staging.                 | `git diff`                      |
| `git diff --staged`   | Show changes between staging and last commit.                 | `git diff --staged`             |

### **3. Branching & Merging**

Work on features in isolation without breaking the main code.

| **Command**              | **Description**                                    | **Example**                   |
| ------------------------ | -------------------------------------------------- | ----------------------------- |
| `git branch`             | List all local branches.                           | `git branch`                  |
| `git branch <name>`      | Create a new branch.                               | `git branch feature-login`    |
| `git checkout <name>`    | Switch to a specific branch.                       | `git checkout feature-login`  |
| `git switch <name>`      | **(Newer)** Safer way to switch branches.          | `git switch main`             |
| `git checkout -b <name>` | Create **and** switch to a new branch in one step. | `git checkout -b new-feature` |
| `git merge <name>`       | Merge branch `<name>` _into_ your current branch.  | `git merge feature-login`     |
| `git branch -d <name>`   | Delete a branch (must be fully merged).            | `git branch -d feature-login` |

### **4. Syncing (Remote Repos)**

interacting with GitHub, GitLab, Bitbucket, etc.

|**Command**|**Description**|**Example**|
|---|---|---|
|`git remote -v`|List remote connections.|`git remote -v`|
|`git fetch`|Download changes from remote but **do not** merge them.|`git fetch origin`|
|`git pull`|Download changes **and** merge them immediately.|`git pull origin main`|
|`git push`|Upload local commits to the remote repo.|`git push origin main`|
|`git push -u origin <name>`|Push a new branch for the first time.|`git push -u origin feature-new`|

### **5. Undo & Fix**

Mistakes happen. Here is how to clean them up.

|**Command**|**Description**|**Use Case**|
|---|---|---|
|`git checkout -- <file>`|Discard changes in working directory.|"I messed up this file, reset it to how it was."|
|`git restore <file>`|**(Newer)** Discard changes in working dir.|Same as above, but clearer syntax.|
|`git reset HEAD <file>`|Unstage a file (remove from Staging area).|"I accidentally typed `git add .`"|
|`git commit --amend`|Change the _last_ commit message (or add forgotten files).|"Oops, typo in my commit message."|
|`git revert <commit-id>`|Create a _new_ commit that undoes a specific past commit.|"That commit 3 days ago broke the app, undo it safely."|

> **Warning:** `git reset --hard` deletes changes permanently. Use with caution!

### **6. Inspection & Logs**

Reviewing history.

|**Command**|**Description**|
|---|---|
|`git log`|Show full commit history.|
|`git log --oneline`|Show history as a clean, single-line list.|
|`git log --graph`|Show history with a visual text-based branch tree.|
|`git blame <file>`|Show who modified each line of a file and when.|

### **7. Stashing**

Save your work temporarily without committing (great when you need to switch branches quickly).

- `git stash` -> Save modified files to a stack and clean working dir.
- `git stash pop` -> Apply the saved changes back to your working dir.
- `git stash list` -> See what you have stashed.

### **Common Workflow Example**

1. **Start:** `git pull origin main` (Get latest code)
2. **Branch:** `git checkout -b feature-dark-mode` (Create new workspace)
3. **Work:** (Write code...)
4. **Save:** `git add .` then `git commit -m "Add dark mode colors"`
5. **Share:** `git push -u origin feature-dark-mode`

## Resolving Merge Conflict

### **1. The Anatomy of a Conflict**

When a conflict occurs, Git pauses the merge and modifies your file to show you both versions. You will see "Conflict Markers" inside the file like this:

- `<<<<<<< HEAD` : This is **Your** code (Current Change).
- `=======` : The **Divider**. Everything above is yours; everything below is theirs.
- `>>>>>>> feature-branch` : This is **Their** code (Incoming Change).

### **2. How to Fix It (Step-by-Step)**

#### **Step 1: Find the Conflict**

Run `git status`. Git will list the files under "Unmerged paths".

Bash

```
git status
# both modified:   index.html
```

#### **Step 2: Open and Edit**

Open the file in your code editor (VS Code, IntelliJ, etc.).

You must deciding what to keep. You have 3 choices:

1. **Keep Current:** Delete the bottom section and the markers.
2. **Keep Incoming:** Delete the top section and the markers.
3. **Combine Both:** Rewrite the code to include both changes and delete the markers.

**Crucial:** You must delete the markers (`<<<<<<<`, `=======`, `>>>>>>>`) completely. The final file should look like normal code.

#### **Step 3: Mark as Resolved**

Once the file looks correct and clean, tell Git you are done with that file.

Bash

```
git add index.html
```

#### **Step 4: Commit**

Once all files are fixed and added, finish the merge.

Bash

```
git commit
```

_Note: You usually don't need `-m` here; Git will pop up a default "Merge branch..." message for you to save._

### **3. The "Panic Button"**

If you get halfway through a conflict, realize you made a mistake, or just get confused, you can **cancel the merge** and go back to exactly how things were before you started.

Bash

```
git merge --abort
```

### **4. Pro Tip: Using VS Code**

Modern editors like VS Code detect these markers automatically.

- They highlight the conflict in colors (usually Green vs Blue).
- They provide clickable buttons right above the text: `Accept Current Change` | `Accept Incoming Change` | `Accept Both Changes`.
- Clicking one of these buttons is safer and faster than deleting lines manually.
