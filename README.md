# Git Commands Reference

A comprehensive guide to Git commands organized by category.

---

## 📥 Getting & Creating Projects

| Command | Description | Example |
|---------|-------------|---------|
| `git init` | <small>Initializes a new Git repository by creating the `.git` directory with subdirectories for objects, refs, HEAD, config, and hooks.</small> | `git init` |
| `git init [project-name]` | <small>Creates a new directory with the specified name and initializes it as a Git repository in one step.</small> | `git init my-project` |
| `git init --bare` | <small>Creates a bare repository without a working directory — files go directly in the current directory rather than a `.git/` subdirectory. Used for central/server repositories.</small> | `git init --bare` |
| `git clone [url]` | <small>Creates a complete local copy of a remote repository, downloading all objects, refs, and history, then checking out the default branch.</small> | `git clone https://github.com/user/repo.git` |
| `git clone [url] [directory]` | <small>Clones a remote repository into a custom-named local directory instead of using the repo's default name.</small> | `git clone https://github.com/user/repo.git my-folder` |
| `git clone --depth [n] [url]` | <small>Creates a shallow clone with truncated history, fetching only the last n commits from each branch tip. Dramatically reduces clone size for large repositories.</small> | `git clone --depth 1 https://github.com/user/repo.git` |
| `git clone --single-branch [url]` | <small>Clones only the HEAD branch history rather than all branches, reducing clone size for repositories with many branches.</small> | `git clone --single-branch https://github.com/user/repo.git` |
| `git clone --branch [name] [url]` | <small>Clones a repository and checks out a specific branch instead of the default branch.</small> | `git clone --branch develop https://github.com/user/repo.git` |
| `git clone --recurse-submodules [url]` | <small>Clones the repository and automatically initializes and updates all nested submodules.</small> | `git clone --recurse-submodules https://github.com/user/repo.git` |

---

## 📸 Basic Snapshotting

| Command | Description | Example |
|---------|-------------|---------|
| `git status` | <small>Displays differences between your working directory, staging area (index), and HEAD — showing staged changes, unstaged changes, and untracked files.</small> | `git status` |
| `git status -s` | <small>Compact two-column status output where the first column shows staging area status and second shows working tree status (M, A, D, ??).</small> | `git status -s` |
| `git add [file]` | <small>Stages a file by hashing its contents, storing it as a blob object in `.git/objects/`, and updating the index to reference it.</small> | `git add index.html` |
| `git add .` | <small>Stages all changes (new files, modifications, deletions) in the current directory and its subdirectories recursively.</small> | `git add .` |
| `git add -A` | <small>Stages all changes throughout the entire repository regardless of current directory, including new files, modifications, and deletions.</small> | `git add -A` |
| `git add -u` | <small>Stages only modifications and deletions to already-tracked files, ignoring new untracked files.</small> | `git add -u` |
| `git add -p` | <small>Enters interactive patch mode allowing you to stage specific hunks of changes rather than entire files — useful for granular, logical commits.</small> | `git add -p` |
| `git commit -m "[message]"` | <small>Creates a permanent snapshot by building a tree object from the current index, creating a commit pointing to that tree, and advancing the branch pointer.</small> | `git commit -m "Fix login bug"` |
| `git commit -am "[message]"` | <small>Automatically stages all tracked modified files and commits in one step — equivalent to `git add -u` followed by `git commit -m`. Does not include untracked files.</small> | `git commit -am "Update README"` |
| `git commit --amend` | <small>Replaces the last commit by creating a new one with the same parent, allowing you to modify the message or add forgotten changes to the previous commit.</small> | `git commit --amend` |
| `git commit --amend --no-edit` | <small>Adds currently staged changes to the last commit without opening the editor to change the commit message.</small> | `git commit --amend --no-edit` |
| `git rm [file]` | <small>Removes a file from both the working directory and staging area, then stages the deletion for the next commit.</small> | `git rm old-file.txt` |
| `git rm --cached [file]` | <small>Removes a file from the staging area and Git's tracking while preserving it in the working directory — useful for untracking files that should stay locally.</small> | `git rm --cached secrets.txt` |
| `git rm -r [folder]` | <small>Recursively removes a directory and all its contents from both working directory and staging area, staging the entire deletion for commit.</small> | `git rm -r old-folder/` |
| `git mv [old] [new]` | <small>Renames or moves a file in both the working directory and staging area, recording the change as a rename that Git can track efficiently.</small> | `git mv old-name.txt new-name.txt` |

---

## ↩️ Undoing Changes

| Command | Description | Example |
|---------|-------------|---------|
| `git restore [file]` | <small>Copies the file from the index to the working directory, discarding unstaged changes and reverting the file to its last staged state.</small> | `git restore index.html` |
| `git restore --staged [file]` | <small>Copies the file from HEAD to the index, unstaging changes without affecting the working directory. Modern replacement for `git reset HEAD [file]`.</small> | `git restore --staged index.html` |
| `git restore --source=[commit] [file]` | <small>Restores a file's contents from any specified commit to the working directory, retrieving old versions without changing branch history.</small> | `git restore --source=HEAD~1 index.html` |
| `git reset [file]` | <small>Removes a file from the staging area by copying it from HEAD to the index without affecting the working directory — effectively unstages changes.</small> | `git reset index.html` |
| `git reset --soft HEAD~1` | <small>Moves the branch pointer back one commit while leaving the index and working directory unchanged — "uncommits" the last commit with changes still staged.</small> | `git reset --soft HEAD~1` |
| `git reset --mixed HEAD~1` | <small>Moves the branch pointer back one commit and resets the index to match, but leaves the working directory unchanged. This is the default reset mode.</small> | `git reset --mixed HEAD~1` |
| `git reset --hard HEAD~1` | <small>Moves the branch pointer back one commit, resets the index, and forces the working directory to match — permanently discards all uncommitted changes.</small> | `git reset --hard HEAD~1` |
| `git reset --hard [commit]` | <small>Resets the branch pointer, index, and working directory to exactly match the specified commit, discarding all changes made after it.</small> | `git reset --hard a1b2c3d` |
| `git revert [commit]` | <small>Creates a new commit that applies the inverse of the specified commit's changes — undoes it while preserving history, safe for public branches.</small> | `git revert a1b2c3d` |
| `git revert HEAD` | <small>Creates a new commit that undoes the changes introduced by the most recent commit, preserving full history.</small> | `git revert HEAD` |
| `git revert -m 1 [merge-commit]` | <small>Reverts a merge commit by specifying parent 1 as the mainline — necessary because merge commits have multiple parents.</small> | `git revert -m 1 abc123` |
| `git revert --no-commit [commit]` | <small>Applies the inverse changes of a commit to your working directory and index without auto-creating a commit — useful for reverting multiple commits together.</small> | `git revert --no-commit a1b2c3d` |
| `git checkout -- [file]` | <small>Discards changes to a file in the working directory by copying it from the index, restoring it to the last staged state. Superseded by `git restore`.</small> | `git checkout -- index.html` |

---

## 🌿 Branching & Merging

| Command | Description | Example |
|---------|-------------|---------|
| `git branch` | <small>Lists all local branches with an asterisk marking the current branch, reading references from `.git/refs/heads/`.</small> | `git branch` |
| `git branch [name]` | <small>Creates a new branch by writing the current HEAD commit's SHA-1 to a new file in `.git/refs/heads/` — an O(1) operation.</small> | `git branch feature-login` |
| `git branch -a` | <small>Lists all branches including local branches and remote-tracking branches from `refs/remotes/`.</small> | `git branch -a` |
| `git branch -r` | <small>Lists only remote-tracking branches stored in `refs/remotes/`, showing your local cache of remote branches.</small> | `git branch -r` |
| `git branch -d [name]` | <small>Safely deletes a local branch only if it has been fully merged into its upstream or HEAD, protecting against accidental commit loss.</small> | `git branch -d old-feature` |
| `git branch -D [name]` | <small>Force-deletes a local branch regardless of merge status, removing the reference even if it contains unmerged commits.</small> | `git branch -D broken-feature` |
| `git branch -m [old] [new]` | <small>Renames a branch by moving its reference file in `.git/refs/heads/`, preserving all commit history.</small> | `git branch -m old-name new-name` |
| `git branch -m [new]` | <small>Renames the current branch to the specified name by updating its reference in `.git/refs/heads/`.</small> | `git branch -m better-name` |
| `git branch --merged` | <small>Lists branches that have been fully merged into the current branch — these can be safely deleted without losing commits.</small> | `git branch --merged` |
| `git branch --no-merged` | <small>Lists branches containing commits not yet merged into the current branch — identifies active branches with unique work.</small> | `git branch --no-merged` |
| `git branch -vv` | <small>Shows local branches with their last commit and upstream tracking branch relationship, including ahead/behind counts.</small> | `git branch -vv` |
| `git checkout [branch]` | <small>Switches to a different branch by updating `.git/HEAD`, updating the index to match the branch's tree, and updating working directory files.</small> | `git checkout develop` |
| `git checkout -` | <small>Switches to the previously checked-out branch using the `@{-1}` reflog reference — quickly toggles between two branches.</small> | `git checkout -` |
| `git checkout -b [branch]` | <small>Creates a new branch and immediately switches to it — combines `git branch` and `git checkout` in a single atomic operation.</small> | `git checkout -b feature-signup` |
| `git checkout -b [branch] [remote]/[branch]` | <small>Creates a local branch tracking a remote branch and switches to it, establishing the upstream relationship for future push/pull operations.</small> | `git checkout -b feature origin/feature` |
| `git switch [branch]` | <small>Modern command for switching branches that provides focused functionality without the file-restoration overloading of `checkout`.</small> | `git switch develop` |
| `git switch -c [branch]` | <small>Creates a new branch and switches to it using the modern switch syntax, with clearer intent than the overloaded `checkout` command.</small> | `git switch -c feature-api` |
| `git switch -` | <small>Switches to the previously checked-out branch, mirroring `git checkout -` with clearer command semantics.</small> | `git switch -` |
| `git merge [branch]` | <small>Integrates changes from the named branch into the current branch using fast-forward if possible, or a three-way merge commit if histories diverged.</small> | `git merge feature-login` |
| `git merge --no-ff [branch]` | <small>Forces creation of a merge commit even when fast-forward is possible, preserving branch context and history structure.</small> | `git merge --no-ff feature-login` |
| `git merge --squash [branch]` | <small>Applies all changes from the target branch as staged modifications without creating a merge commit or preserving commit history — lets you create one clean commit.</small> | `git merge --squash feature-login` |
| `git merge --abort` | <small>Aborts a conflicted merge and restores the repository to its state before the merge began, using MERGE_HEAD and ORIG_HEAD references.</small> | `git merge --abort` |
| `git merge -X theirs [branch]` | <small>Performs a merge and automatically resolves conflicts by favoring changes from the branch being merged in.</small> | `git merge -X theirs feature` |
| `git merge -X ours [branch]` | <small>Performs a merge and automatically resolves conflicts by favoring changes from the current branch.</small> | `git merge -X ours feature` |
| `git rebase [branch]` | <small>Replays commits from the current branch onto the tip of the target branch, creating linear history but rewriting commits with new SHA-1 hashes.</small> | `git rebase main` |
| `git rebase -i HEAD~[n]` | <small>Opens an interactive editor listing the last n commits with commands (pick, reword, edit, squash, fixup, drop) to reorder, combine, edit, or remove commits.</small> | `git rebase -i HEAD~3` |
| `git rebase --continue` | <small>Continues a rebase after resolving conflicts by applying the next commit in sequence.</small> | `git rebase --continue` |
| `git rebase --abort` | <small>Abandons an in-progress rebase and returns the branch to its state before the rebase began, using ORIG_HEAD to restore the previous commit.</small> | `git rebase --abort` |
| `git rebase --onto [newbase] [oldbase] [branch]` | <small>Provides precise rebase control by replaying commits between oldbase and branch onto newbase — useful for complex history manipulation like grafting branches.</small> | `git rebase --onto main server client` |
| `git cherry-pick [commit]` | <small>Copies a specific commit by reading its diff, applying it to the current HEAD, and creating a new commit with the same changes but a different SHA-1.</small> | `git cherry-pick a1b2c3d` |
| `git cherry-pick -n [commit]` | <small>Applies the changes from a commit to your working directory and index without auto-creating a commit — useful for cherry-picking multiple commits at once.</small> | `git cherry-pick -n a1b2c3d` |

---

## 🌐 Remote Repositories

| Command | Description | Example |
|---------|-------------|---------|
| `git remote` | <small>Lists all configured remote repositories by shortname, reading from `.git/config`.</small> | `git remote` |
| `git remote -v` | <small>Lists all remotes showing both shortnames and their corresponding URLs for fetch and push operations.</small> | `git remote -v` |
| `git remote add [name] [url]` | <small>Adds a new remote repository reference by writing configuration to `.git/config` with the remote name, URL, and default fetch refspec.</small> | `git remote add origin https://github.com/user/repo.git` |
| `git remote remove [name]` | <small>Removes a remote repository reference from `.git/config` and deletes all associated remote-tracking branches.</small> | `git remote remove origin` |
| `git remote rename [old] [new]` | <small>Renames a remote shortname by updating `.git/config` and renaming all remote-tracking branches from `refs/remotes/old/` to `refs/remotes/new/`.</small> | `git remote rename origin upstream` |
| `git remote set-url [name] [url]` | <small>Changes the URL for an existing remote by updating the remote.name.url entry in `.git/config` — useful for switching between HTTPS and SSH.</small> | `git remote set-url origin git@github.com:user/repo.git` |
| `git remote show [name]` | <small>Displays detailed information about a remote including URL, tracked branches, and whether remote branches are up-to-date or stale.</small> | `git remote show origin` |
| `git remote prune [name]` | <small>Deletes stale remote-tracking branches that no longer exist on the remote by comparing local `refs/remotes/name/` with the remote's advertised refs.</small> | `git remote prune origin` |
| `git fetch` | <small>Downloads objects and refs from the default remote by negotiating missing objects, receiving a packfile, and updating remote-tracking branches.</small> | `git fetch` |
| `git fetch [remote]` | <small>Downloads objects and refs from a specific remote repository, updating only that remote's tracking branches.</small> | `git fetch origin` |
| `git fetch --all` | <small>Fetches from all configured remote repositories simultaneously, updating all remote-tracking branches in one operation.</small> | `git fetch --all` |
| `git fetch --prune` | <small>Downloads updates from remote and simultaneously removes remote-tracking branches that no longer exist on the remote.</small> | `git fetch --prune` |
| `git fetch --tags` | <small>Downloads all tags from the remote repository in addition to the normal fetch operation.</small> | `git fetch --tags` |
| `git fetch --depth [n]` | <small>Creates or extends a shallow fetch, limiting history depth to n commits from branch tips and updating `.git/shallow` with boundary commits.</small> | `git fetch --depth 5` |
| `git pull` | <small>Performs `git fetch` to download remote changes then automatically merges the remote tracking branch into your current local branch.</small> | `git pull` |
| `git pull [remote] [branch]` | <small>Fetches changes from a specific remote branch and merges them into your current branch.</small> | `git pull origin main` |
| `git pull --rebase` | <small>Fetches remote changes and rebases your current branch onto the updated remote tracking branch instead of merging, maintaining linear history.</small> | `git pull --rebase` |
| `git pull --ff-only` | <small>Only allows fast-forward merges during pull — aborts if a merge commit would be required, preventing unexpected merge commits.</small> | `git pull --ff-only` |
| `git push` | <small>Uploads your local branch commits to the remote by sending a packfile with missing objects and requesting remote ref updates.</small> | `git push` |
| `git push [remote] [branch]` | <small>Pushes a specific local branch to the named remote repository.</small> | `git push origin main` |
| `git push -u [remote] [branch]` | <small>Pushes a branch and sets it as the upstream tracking branch by writing to `.git/config`, enabling future argument-less push/pull.</small> | `git push -u origin feature-login` |
| `git push --all` | <small>Pushes all local branches to the remote repository, creating or updating remote branches to match.</small> | `git push --all` |
| `git push --tags` | <small>Pushes all local tags to the remote repository, uploading tag objects and updating remote refs in `refs/tags/`.</small> | `git push --tags` |
| `git push --force` | <small>Forcefully overwrites a remote branch even without fast-forward — dangerous as it can destroy others' work.</small> | `git push --force` |
| `git push --force-with-lease` | <small>Safer forced push that checks if the remote ref is at the expected value before overwriting — rejects if someone else has pushed.</small> | `git push --force-with-lease` |
| `git push [remote] --delete [branch]` | <small>Deletes a remote branch by removing its reference on the remote repository.</small> | `git push origin --delete old-feature` |
| `git push --atomic` | <small>Pushes multiple refs in an atomic transaction where either all refs update or none do — prevents partial failures in multi-ref pushes.</small> | `git push --atomic` |

---

## 📦 Stashing

| Command | Description | Example |
|---------|-------------|---------|
| `git stash` | <small>Saves your current uncommitted changes (both staged and unstaged) onto a stash stack and reverts the working directory to HEAD, letting you switch context cleanly.</small> | `git stash` |
| `git stash push -m "[message]"` | <small>Stashes changes with a descriptive message to make it easier to identify later when listing multiple stashes.</small> | `git stash push -m "WIP: login form"` |
| `git stash list` | <small>Lists all stashed changesets with their stash index, branch, and message, showing the full stash stack.</small> | `git stash list` |
| `git stash show` | <small>Shows a summary of files changed in the most recent stash.</small> | `git stash show` |
| `git stash show -p` | <small>Shows the full diff of the most recent stash, including all changes made to each file.</small> | `git stash show -p` |
| `git stash pop` | <small>Applies the most recent stash to the working directory and removes it from the stash stack. Fails if conflicts arise.</small> | `git stash pop` |
| `git stash apply [stash@{n}]` | <small>Applies a specific stash to the working directory without removing it from the stash stack — useful for applying the same stash to multiple branches.</small> | `git stash apply stash@{2}` |
| `git stash drop [stash@{n}]` | <small>Removes a specific stash from the stash stack without applying it.</small> | `git stash drop stash@{1}` |
| `git stash clear` | <small>Removes all stashes from the stash stack permanently — use with caution as this cannot be undone.</small> | `git stash clear` |
| `git stash branch [branch]` | <small>Creates a new branch from the commit where the stash was made, applies the stash to it, and drops the stash if successful.</small> | `git stash branch fix-from-stash` |

---

## 🔍 Inspecting & Comparing

| Command | Description | Example |
|---------|-------------|---------|
| `git log` | <small>Shows the commit history for the current branch, displaying commit SHA, author, date, and message in reverse chronological order.</small> | `git log` |
| `git log --oneline` | <small>Displays a condensed log with each commit on one line, showing the short SHA and commit message — great for a quick overview.</small> | `git log --oneline` |
| `git log --graph` | <small>Adds an ASCII graph of the branch and merge history to the left of the log output, visualizing branching structure.</small> | `git log --graph --oneline` |
| `git log --all` | <small>Shows commits from all branches and refs, not just the current branch — useful for seeing the full repository history.</small> | `git log --all --oneline` |
| `git log -p` | <small>Shows the commit log with the full diff (patch) introduced by each commit — combines history with change inspection.</small> | `git log -p` |
| `git log --stat` | <small>Shows the commit log with a summary of files changed, insertions, and deletions for each commit.</small> | `git log --stat` |
| `git log --author="[name]"` | <small>Filters the commit log to show only commits made by the specified author.</small> | `git log --author="Alice"` |
| `git log --since="[date]"` | <small>Shows only commits made after the specified date — supports natural language like "2 weeks ago" or ISO format dates.</small> | `git log --since="2024-01-01"` |
| `git log --until="[date]"` | <small>Shows only commits made before the specified date.</small> | `git log --until="2024-06-01"` |
| `git log --grep="[pattern]"` | <small>Filters commits whose message matches the specified pattern, useful for finding related commits by keyword.</small> | `git log --grep="bugfix"` |
| `git log [branch1]..[branch2]` | <small>Shows commits in branch2 that are not in branch1 — useful for seeing what would be merged or what's ahead/behind.</small> | `git log main..feature` |
| `git log -- [file]` | <small>Shows only commits that changed the specified file, tracing the history of a particular file.</small> | `git log -- src/app.js` |
| `git diff` | <small>Shows changes between the working directory and the staging area — what is unstaged.</small> | `git diff` |
| `git diff --staged` | <small>Shows changes between the staging area and the last commit — what would be included in the next commit.</small> | `git diff --staged` |
| `git diff [branch1]..[branch2]` | <small>Shows the difference between two branches, displaying what changed between their latest commits.</small> | `git diff main..feature` |
| `git diff [commit1] [commit2]` | <small>Shows the changes introduced between two specific commits.</small> | `git diff a1b2c3d e4f5g6h` |
| `git show [commit]` | <small>Displays the metadata and content changes introduced by a specific commit, combining log and diff output.</small> | `git show a1b2c3d` |
| `git show --stat [commit]` | <small>Shows a commit's metadata along with a summary of files changed without the full diff.</small> | `git show --stat a1b2c3d` |
| `git shortlog` | <small>Summarizes `git log` output grouped by author, showing a count of commits per person — useful for release notes.</small> | `git shortlog -sn` |
| `git blame [file]` | <small>Annotates each line of a file with the commit and author that last modified it, tracing the origin of every line.</small> | `git blame index.html` |
| `git blame -L [start],[end] [file]` | <small>Annotates only the specified line range of a file with commit and author information.</small> | `git blame -L 10,25 index.html` |

---

## 🔎 Searching

| Command | Description | Example |
|---------|-------------|---------|
| `git grep "[pattern]"` | <small>Searches the working directory for lines matching a pattern — faster than system grep as it respects `.gitignore` and only searches tracked files.</small> | `git grep "TODO"` |
| `git grep -n "[pattern]"` | <small>Searches for a pattern and includes line numbers in the output.</small> | `git grep -n "TODO"` |
| `git grep -c "[pattern]"` | <small>Shows only the count of matching lines per file rather than the matching lines themselves.</small> | `git grep -c "TODO"` |
| `git grep -i "[pattern]"` | <small>Performs a case-insensitive search for the pattern across tracked files.</small> | `git grep -i "todo"` |
| `git log -S "[string]"` | <small>Finds commits that added or removed the specified string (pickaxe search) — useful for tracing when a specific value appeared or disappeared.</small> | `git log -S "secretKey"` |
| `git log -G "[regex]"` | <small>Finds commits where the diff matches the specified regular expression — more powerful than `-S` for pattern-based searches.</small> | `git log -G "function.*auth"` |
| `git bisect start` | <small>Begins a binary search through commit history to find the commit that introduced a bug — starts the bisect session.</small> | `git bisect start` |
| `git bisect good [commit]` | <small>Marks a commit as good (bug not present) during bisect, narrowing the search range.</small> | `git bisect good v1.0` |
| `git bisect bad [commit]` | <small>Marks a commit as bad (bug present) during bisect, narrowing the search range to between good and bad.</small> | `git bisect bad HEAD` |
| `git bisect reset` | <small>Ends the bisect session and returns the repository to the original branch/commit state.</small> | `git bisect reset` |
| `git bisect run [script]` | <small>Automatically runs a script on each bisect step, marking commits as good or bad based on the script's exit code — fully automates the binary search.</small> | `git bisect run ./test.sh` |

---

## 🏷️ Tagging

| Command | Description | Example |
|---------|-------------|---------|
| `git tag` | <small>Lists all tags in the repository in alphabetical order.</small> | `git tag` |
| `git tag [name]` | <small>Creates a lightweight tag — a named pointer to the current commit, stored as a simple ref with no extra metadata.</small> | `git tag v1.0.0` |
| `git tag -a [name] -m "[message]"` | <small>Creates an annotated tag with a tag object that stores the tagger, date, message, and optionally a GPG signature — recommended for releases.</small> | `git tag -a v1.0.0 -m "Release v1.0.0"` |
| `git tag -a [name] [commit]` | <small>Creates an annotated tag pointing to a specific commit rather than HEAD — useful for retroactively tagging past releases.</small> | `git tag -a v0.9.0 a1b2c3d` |
| `git tag -d [name]` | <small>Deletes a local tag by removing its reference from `.git/refs/tags/`.</small> | `git tag -d v0.9.0` |
| `git show [tag]` | <small>Shows the tag metadata and the commit it points to, including the tag message for annotated tags.</small> | `git show v1.0.0` |
| `git push [remote] [tag]` | <small>Pushes a specific tag to the remote repository — tags are not pushed by default with `git push`.</small> | `git push origin v1.0.0` |
| `git push [remote] --tags` | <small>Pushes all local tags to the remote repository in one operation.</small> | `git push origin --tags` |
| `git push [remote] --delete [tag]` | <small>Deletes a tag from the remote repository.</small> | `git push origin --delete v0.9.0` |

---

## ⚙️ Configuration

| Command | Description | Example |
|---------|-------------|---------|
| `git config --global user.name "[name]"` | <small>Sets your name for all commits globally by writing to `~/.gitconfig` — this appears as the author name in commits.</small> | `git config --global user.name "Alice Smith"` |
| `git config --global user.email "[email]"` | <small>Sets your email for all commits globally — used to associate commits with your identity on platforms like GitHub.</small> | `git config --global user.email "alice@example.com"` |
| `git config --list` | <small>Displays all configuration settings from system, global, and local config files with their current values.</small> | `git config --list` |
| `git config --global core.editor [editor]` | <small>Sets the default text editor for commit messages and interactive operations globally.</small> | `git config --global core.editor vim` |
| `git config --global alias.[alias] "[command]"` | <small>Creates a shortcut alias for a Git command, stored in `~/.gitconfig` for reuse across repositories.</small> | `git config --global alias.st status` |
| `git config --global merge.tool [tool]` | <small>Sets the default merge conflict resolution tool used when running `git mergetool`.</small> | `git config --global merge.tool vimdiff` |
| `git config --global color.ui auto` | <small>Enables colored output in Git commands when outputting to a terminal, improving readability.</small> | `git config --global color.ui auto` |
| `git config --global pull.rebase true` | <small>Configures `git pull` to rebase instead of merge by default, maintaining a cleaner linear history.</small> | `git config --global pull.rebase true` |
| `git config --local [key] [value]` | <small>Sets a configuration value only for the current repository by writing to `.git/config`, overriding global settings.</small> | `git config --local core.ignorecase false` |
| `git config --global --unset [key]` | <small>Removes a configuration entry from the global config file.</small> | `git config --global --unset user.email` |

---

## 🧹 Cleanup & Maintenance

| Command | Description | Example |
|---------|-------------|---------|
| `git clean -n` | <small>Performs a dry run of `git clean`, showing which untracked files would be removed without actually deleting them.</small> | `git clean -n` |
| `git clean -f` | <small>Removes untracked files from the working directory — cannot be undone, use `-n` first to preview.</small> | `git clean -f` |
| `git clean -fd` | <small>Removes untracked files and directories from the working directory recursively.</small> | `git clean -fd` |
| `git clean -fx` | <small>Removes all untracked files including those matched by `.gitignore`, useful for a completely clean build state.</small> | `git clean -fx` |
| `git gc` | <small>Runs garbage collection — packs loose objects, removes unreachable objects, and optimizes repository storage for performance.</small> | `git gc` |
| `git gc --aggressive` | <small>Runs a more thorough but slower garbage collection that recomputes delta compression for better storage optimization.</small> | `git gc --aggressive` |
| `git prune` | <small>Removes object files that are no longer referenced by any commits or refs from `.git/objects/` — usually called automatically by `git gc`.</small> | `git prune` |
| `git fsck` | <small>Verifies the integrity of the object database, checking for corrupted or missing objects and unreachable commits.</small> | `git fsck` |
| `git reflog` | <small>Shows a log of all HEAD movements including branch switches, commits, resets, and rebases — your safety net for recovering lost commits.</small> | `git reflog` |
| `git reflog expire --expire=now --all` | <small>Expires all reflog entries immediately — removes the safety net for recovering old states. Use with caution.</small> | `git reflog expire --expire=now --all` |
| `git maintenance start` | <small>Registers the repository for scheduled background maintenance tasks (GC, fetch, etc.) run by the system's task scheduler.</small> | `git maintenance start` |

---

## 🚀 Advanced & Recovery

| Command | Description | Example |
|---------|-------------|---------|
| `git worktree add [path] [branch]` | <small>Creates an additional working directory linked to the same repository, allowing you to check out multiple branches simultaneously without stashing.</small> | `git worktree add ../hotfix hotfix-branch` |
| `git worktree list` | <small>Lists all working trees associated with the repository, showing their paths and checked-out branches.</small> | `git worktree list` |
| `git worktree remove [path]` | <small>Removes a linked working tree and cleans up its administrative files from the main repository.</small> | `git worktree remove ../hotfix` |
| `git archive --format=zip HEAD` | <small>Creates a zip archive of the working tree at HEAD without including the `.git` directory — useful for creating source releases.</small> | `git archive --format=zip HEAD > release.zip` |
| `git archive --prefix=[dir]/ [commit]` | <small>Creates an archive of a specific commit's content with all files placed under the specified directory prefix.</small> | `git archive --prefix=myapp/ HEAD` |
| `git notes add -m "[note]" [commit]` | <small>Attaches a note to a commit without modifying it — notes are stored separately and can be pushed/pulled independently.</small> | `git notes add -m "Reviewed" a1b2c3d` |
| `git commit --fixup [commit]` | <small>Creates a commit intended to be squashed into the specified previous commit during an interactive rebase with `--autosquash`.</small> | `git commit --fixup a1b2c3d` |
| `git rebase -i --autosquash HEAD~[n]` | <small>Interactive rebase that automatically orders fixup and squash commits next to their target commits, streamlining history cleanup.</small> | `git rebase -i --autosquash HEAD~5` |

---

## 📎 Submodules

| Command | Description | Example |
|---------|-------------|---------|
| `git submodule add [url]` | <small>Adds an external repository as a submodule by cloning it into the specified path and registering it in `.gitmodules` and the index.</small> | `git submodule add https://github.com/user/lib.git` |
| `git submodule init` | <small>Initializes submodule configuration from `.gitmodules` by writing entries to `.git/config` — required after cloning a repo with submodules.</small> | `git submodule init` |
| `git submodule update` | <small>Checks out the commit recorded in the parent repository for each submodule, syncing submodules to their recorded state.</small> | `git submodule update` |
| `git submodule update --init --recursive` | <small>Initializes and updates all submodules (including nested ones) in one step — the recommended command after a fresh clone.</small> | `git submodule update --init --recursive` |
| `git submodule update --remote` | <small>Updates each submodule to the latest commit on its tracked remote branch rather than the pinned commit in the parent repo.</small> | `git submodule update --remote` |
| `git submodule status` | <small>Shows the current commit SHA of each submodule, with a prefix indicating if it's uninitialized (-), modified (+), or in conflict (U).</small> | `git submodule status` |
| `git submodule foreach [command]` | <small>Runs a shell command in each submodule's directory — useful for batch operations like pulling or checking status across all submodules.</small> | `git submodule foreach git pull` |
| `git submodule deinit [path]` | <small>Unregisters a submodule by removing its entry from `.git/config` and emptying its directory, while keeping `.gitmodules` intact.</small> | `git submodule deinit lib/` |

---

## 🔧 Plumbing Commands

| Command | Description | Example |
|---------|-------------|---------|
| `git cat-file -t [sha]` | <small>Shows the type of a Git object (blob, tree, commit, or tag) identified by its SHA-1 hash.</small> | `git cat-file -t a1b2c3d` |
| `git cat-file -p [sha]` | <small>Pretty-prints the content of a Git object — reveals raw commit data, tree listings, or file blob contents.</small> | `git cat-file -p a1b2c3d` |
| `git ls-files` | <small>Lists all files currently tracked by Git in the index, with optional flags to show untracked, modified, or staged files.</small> | `git ls-files` |
| `git ls-tree [commit]` | <small>Lists the tree object contents for a given commit or tree SHA, showing the mode, type, SHA, and name of each entry.</small> | `git ls-tree HEAD` |
| `git ls-tree -r [commit]` | <small>Recursively lists all files in a tree, expanding subdirectories to show every blob in the commit's snapshot.</small> | `git ls-tree -r HEAD` |
| `git rev-parse [ref]` | <small>Resolves a ref or symbolic name (like HEAD, branch name, or tag) to its full 40-character SHA-1 hash.</small> | `git rev-parse HEAD` |
| `git rev-parse --abbrev-ref HEAD` | <small>Outputs the short name of the current branch by resolving HEAD to its symbolic ref.</small> | `git rev-parse --abbrev-ref HEAD` |
| `git hash-object [file]` | <small>Computes and outputs the SHA-1 hash of a file's content as Git would store it, without actually writing anything to the repository.</small> | `git hash-object file.txt` |
| `git hash-object -w [file]` | <small>Computes the SHA-1 hash of a file and writes it as a blob object into `.git/objects/`, making it part of the object database.</small> | `git hash-object -w file.txt` |
| `git write-tree` | <small>Creates a tree object from the current index content and outputs its SHA-1 — used in low-level scripting to capture a snapshot of the staging area.</small> | `git write-tree` |
| `git commit-tree [tree-sha]` | <small>Creates a commit object from a tree SHA — low-level plumbing used in scripting where `git commit` would be too high-level.</small> | `git commit-tree abc123 -m "msg"` |
| `git update-ref [ref] [sha]` | <small>Safely updates a ref (branch or tag pointer) to point to a new SHA-1 — the low-level way to move branch pointers.</small> | `git update-ref refs/heads/main a1b2c3d` |
| `git symbolic-ref HEAD` | <small>Reads or sets the symbolic ref that HEAD points to — useful for scripting to determine or change the current branch at the plumbing level.</small> | `git symbolic-ref HEAD` |
