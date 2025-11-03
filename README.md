# Git Commands Reference

A comprehensive guide to Git commands with detailed descriptions explaining what each command does and how it works internally.

---

## 📥 Getting & Creating Projects

| Command | Description | Example |
|---------|-------------|---------|
| `git init` | Initializes a new Git repository by creating the `.git` directory structure with subdirectories for objects (256 hash-based subdirs), refs/heads/ for branches, refs/tags/ for tags, HEAD file, config, and hooks, establishing the foundation for version control in the current directory. | `git init` |
| `git init [project-name]` | Creates a new directory with the specified name and initializes it as a Git repository, combining directory creation and repository initialization in a single command. | `git init my-project` |
| `git init --bare` | Creates a bare repository without a working directory, placing Git files directly in the current directory rather than a .git/ subdirectory, typically used for central/server repositories that only store history and never have files checked out. | `git init --bare` |
| `git clone [url]` | Creates a complete local copy of a remote repository by initializing a new repository, adding the remote as "origin", downloading all objects and refs, creating remote-tracking branches, and checking out the default branch with full history. | `git clone https://github.com/user/repo.git` |
| `git clone [url] [directory]` | Clones a remote repository into a specifically named local directory instead of using the repository's default name, allowing custom organization of cloned projects. | `git clone https://github.com/user/repo.git my-folder` |
| `git clone --depth [n] [url]` | Creates a shallow clone with truncated history, fetching only the specified number of commits from each branch tip while creating a `.git/shallow` file to track boundaries, dramatically reducing clone size and time for large repositories. | `git clone --depth 1 https://github.com/user/repo.git` |
| `git clone --single-branch [url]` | Clones only the HEAD branch's history rather than all branches, significantly reducing clone size and time for repositories with many branches. | `git clone --single-branch https://github.com/user/repo.git` |
| `git clone --branch [name] [url]` | Clones a repository and checks out a specific branch instead of the default branch, useful when you only need to work with a particular branch. | `git clone --branch develop https://github.com/user/repo.git` |

---

## 📸 Basic Snapshotting

| Command | Description | Example |
|---------|-------------|---------|
| `git status` | Displays the state differences between your working directory, staging area (index), and HEAD by comparing all three trees, reporting staged changes (index vs HEAD), unstaged changes (working directory vs index), and untracked files not in the index. | `git status` |
| `git status -s` | Provides compact two-column status output where the first column shows staging area status and second column shows working tree status, using letters like M (modified), A (added), D (deleted), ?? (untracked) for quick scanning. | `git status -s` |
| `git add [file]` | Moves file content from working directory into the staging area by calculating the SHA-1 hash of file contents, compressing and storing it as a blob object in `.git/objects/`, and updating the index to point to this blob, preparing the snapshot for the next commit. | `git add index.html` |
| `git add .` | Stages all changes (new files, modifications, and deletions) in the current directory and its subdirectories, recursively adding everything to the index for the next commit. | `git add .` |
| `git add -A` | Stages all changes throughout the entire repository regardless of current directory, including new files, modifications, and deletions across all paths, ensuring a complete snapshot of all work. | `git add -A` |
| `git add -u` | Stages only modifications and deletions to files already tracked by Git, ignoring new untracked files, useful for committing changes to existing files without adding new ones. | `git add -u` |
| `git add -p` | Enters interactive patch mode allowing you to review and stage specific hunks (portions) of changes rather than entire files, enabling granular commit composition by selecting which changes belong together logically. | `git add -p` |
| `git commit -m "[message]"` | Creates a permanent snapshot by building a tree object from the current index, creating a commit object pointing to this tree with parent reference to current HEAD, and moving the branch pointer forward, recording the message inline without opening an editor. | `git commit -m "Fix login bug"` |
| `git commit -am "[message]"` | Automatically stages all tracked modified files and commits them in one step, combining `git add -u` and `git commit -m` but not affecting untracked files. | `git commit -am "Update README"` |
| `git commit --amend` | Replaces the last commit by creating a new commit with the same parent, allowing you to modify the commit message or add forgotten changes to the previous commit, effectively rewriting the most recent history. | `git commit --amend` |
| `git commit --amend --no-edit` | Adds currently staged changes to the last commit without changing its commit message, useful for fixing mistakes or adding forgotten files immediately after committing. | `git commit --amend --no-edit` |
| `git rm [file]` | Removes a file from both the working directory and the staging area, then stages this deletion for the next commit, effectively marking the file for removal from version control. | `git rm old-file.txt` |
| `git rm --cached [file]` | Removes a file from the staging area and Git's tracking while preserving it in the working directory, useful for untracking files that should remain locally but not in version control (like secrets or IDE configs). | `git rm --cached secrets.txt` |
| `git rm -r [folder]` | Recursively removes a directory and all its contents from both working directory and staging area, staging the entire deletion for commit. | `git rm -r old-folder/` |
| `git mv [old] [new]` | Renames or moves a file in both the working directory and staging area, recording the change as a rename operation that Git can track efficiently, equivalent to `mv` + `git rm` + `git add` but with proper rename detection. | `git mv old-name.txt new-name.txt` |

---

## ↩️ Undoing Changes

| Command | Description | Example |
|---------|-------------|---------|
| `git restore [file]` | Copies the file from the index to the working directory, discarding any unstaged changes in the working directory and reverting the file to its last staged state without affecting commits or the index. | `git restore index.html` |
| `git restore --staged [file]` | Copies the file from HEAD to the index, unstaging any changes without affecting the working directory, serving as the modern replacement for `git reset HEAD [file]`. | `git restore --staged index.html` |
| `git restore --source=[commit] [file]` | Restores a file's contents from any specified commit to the working directory, allowing you to retrieve old versions of files without changing commits or branch history. | `git restore --source=HEAD~1 index.html` |
| `git reset [file]` | Removes a file from the staging area by copying it from HEAD to the index without affecting the working directory, effectively unstaging changes while preserving your edits. | `git reset index.html` |
| `git reset --soft HEAD~1` | Moves the current branch pointer back one commit while leaving both the index and working directory unchanged, effectively "uncommitting" the last commit while keeping all changes staged and ready for a new commit. | `git reset --soft HEAD~1` |
| `git reset --mixed HEAD~1` | Moves the branch pointer back one commit and resets the index to match, but leaves the working directory unchanged, unstaging the commit's changes while preserving them in your working files (this is the default reset mode). | `git reset --mixed HEAD~1` |
| `git reset --hard HEAD~1` | Moves the branch pointer back one commit, resets the index, and forces the working directory to match the new HEAD, permanently discarding all uncommitted changes—this is dangerous and cannot be undone without reflog. | `git reset --hard HEAD~1` |
| `git reset --hard [commit]` | Forcefully resets your branch pointer, index, and working directory to exactly match the specified commit, discarding all changes made after that commit—use with extreme caution as this destroys uncommitted work. | `git reset --hard a1b2c3d` |
| `git revert [commit]` | Creates a new commit that applies the inverse changes of the specified commit, effectively undoing it while preserving history, making it safe for use on public branches where rewriting history would cause problems for collaborators. | `git revert a1b2c3d` |
| `git revert HEAD` | Creates a new commit that undoes the changes introduced by the most recent commit, preserving the full history while effectively removing the last commit's effects. | `git revert HEAD` |
| `git revert -m 1 [merge-commit]` | Reverts a merge commit by specifying which parent (1 or 2) should be considered the mainline, necessary because merge commits have multiple parents and Git needs to know which history to preserve. | `git revert -m 1 abc123` |
| `git revert --no-commit [commit]` | Applies the inverse changes of a commit to your working directory and index without automatically creating a commit, allowing you to revert multiple commits together before creating a single revert commit. | `git revert --no-commit a1b2c3d` |
| `git checkout -- [file]` | Discards changes to a file in the working directory by copying it from the index, restoring it to the last staged state (note: this older syntax is being replaced by `git restore`). | `git checkout -- index.html` |

---

## 🌿 Branching & Merging

| Command | Description | Example |
|---------|-------------|---------|
| `git branch` | Lists all local branches with an asterisk marking the currently active branch, reading branch references from `.git/refs/heads/` directory where each branch is stored as a 40-byte file containing a commit SHA-1. | `git branch` |
| `git branch [name]` | Creates a new branch by writing the current HEAD commit's SHA-1 to a new file in `.git/refs/heads/`, making branch creation an O(1) operation taking microseconds, without switching to the new branch. | `git branch feature-login` |
| `git branch -a` | Lists all branches including both local branches from `refs/heads/` and remote-tracking branches from `refs/remotes/`, providing a complete view of all branch references. | `git branch -a` |
| `git branch -r` | Lists only remote-tracking branches stored in `refs/remotes/`, showing your local cache of branches that exist on remote repositories. | `git branch -r` |
| `git branch -d [name]` | Safely deletes a local branch only if it has been fully merged into its upstream or HEAD, protecting you from accidentally losing commits by refusing to delete branches with unmerged work. | `git branch -d old-feature` |
| `git branch -D [name]` | Force deletes a local branch regardless of merge status, removing the branch reference even if it contains unmerged commits that would be lost (use cautiously as commits may become unreachable). | `git branch -D broken-feature` |
| `git branch -m [old] [new]` | Renames a branch by moving the reference file in `.git/refs/heads/` from the old name to the new name, preserving all commit history and references. | `git branch -m old-name new-name` |
| `git branch -m [new]` | Renames the current branch to a new name by updating its reference in `.git/refs/heads/` and updating HEAD if necessary. | `git branch -m better-name` |
| `git branch --merged` | Lists branches that have been fully merged into the current branch, showing which branches can be safely deleted without losing any commits. | `git branch --merged` |
| `git branch --no-merged` | Lists branches containing commits that haven't been merged into the current branch, identifying active development branches that still contain unique work. | `git branch --no-merged` |
| `git checkout [branch]` | Switches to a different branch by updating `.git/HEAD` to point to the target branch, updating the index to match that branch's tree, and updating working directory files accordingly. | `git checkout develop` |
| `git checkout -` | Switches to the previously checked out branch using the reflog reference `@{-1}`, providing a quick way to toggle between two branches. | `git checkout -` |
| `git checkout -b [branch]` | Creates a new branch and immediately switches to it in a single atomic operation, combining `git branch` and `git checkout` for convenient branch creation and switching. | `git checkout -b feature-signup` |
| `git checkout -b [branch] [remote]/[branch]` | Creates a local branch tracking a remote branch and switches to it, establishing the upstream relationship for future push/pull operations. | `git checkout -b feature origin/feature` |
| `git switch [branch]` | Modern command for switching branches that provides focused functionality without the file-restoration overloading of checkout, updating HEAD, index, and working directory to match the target branch. | `git switch develop` |
| `git switch -c [branch]` | Creates a new branch and switches to it using the modern switch syntax, providing clearer intent than the overloaded checkout command. | `git switch -c feature-api` |
| `git switch -` | Switches to the previously checked-out branch, mirroring the `git checkout -` functionality with clearer command semantics. | `git switch -` |
| `git merge [branch]` | Integrates changes from the named branch into the current branch using either fast-forward (if current branch is ancestor of target) or three-way merge (creating a merge commit with multiple parents if histories diverged). | `git merge feature-login` |
| `git merge --no-ff [branch]` | Forces creation of a merge commit even when fast-forward is possible, preserving the branch context and history structure rather than creating a linear history. | `git merge --no-ff feature-login` |
| `git merge --squash [branch]` | Applies all changes from the target branch as staged modifications without creating a merge commit or preserving commit history, allowing you to create a single commit for all the merged changes. | `git merge --squash feature-login` |
| `git merge --abort` | Aborts a merge operation that resulted in conflicts, restoring the repository to its state before the merge began by using MERGE_HEAD and ORIG_HEAD references. | `git merge --abort` |
| `git merge -X theirs [branch]` | Performs a merge using the recursive strategy with the "theirs" option to automatically resolve conflicts by favoring changes from the branch being merged in. | `git merge -X theirs feature` |
| `git merge -X ours [branch]` | Performs a merge using the recursive strategy with the "ours" option to automatically resolve conflicts by favoring changes from the current branch. | `git merge -X ours feature` |
| `git rebase [branch]` | Replays commits from current branch onto the tip of target branch by finding the common ancestor, saving each commit as a diff, resetting to the target branch, and applying each diff as a new commit with new SHA-1 hashes, creating linear history but rewriting commits. | `git rebase main` |
| `git rebase -i HEAD~[n]` | Opens an interactive editor listing the last n commits with commands (pick, reword, edit, squash, fixup, drop) allowing you to reorder, combine, edit, or remove commits, providing powerful history rewriting capabilities. | `git rebase -i HEAD~3` |
| `git rebase --continue` | Continues a rebase after resolving conflicts by applying the next commit in sequence, used after manually fixing merge conflicts during an interactive or standard rebase. | `git rebase --continue` |
| `git rebase --abort` | Abandons an in-progress rebase and returns the branch to its state before the rebase began, using the ORIG_HEAD reference to restore the previous commit. | `git rebase --abort` |
| `git rebase --onto [newbase] [oldbase] [branch]` | Provides precise control over rebase by replaying commits between oldbase and branch onto newbase, useful for complex history manipulation like extracting commit ranges or grafting branches. | `git rebase --onto main server client` |
| `git cherry-pick [commit]` | Copies a specific commit by reading its diff, applying that diff to the current HEAD, and creating a new commit with the same changes but different parent and SHA-1, leaving the original commit unchanged. | `git cherry-pick a1b2c3d` |
| `git cherry-pick -n [commit]` | Applies the changes from a commit to your working directory and index without automatically creating a new commit, allowing you to cherry-pick multiple commits before committing them together. | `git cherry-pick -n a1b2c3d` |

---

## 📦 Stashing

| Command | Description | Example |
|---------|-------------|---------|
| `git stash` | Saves your current working directory and index changes to a stack by creating a special commit structure (storing working tree state, index state, and untracked files as separate tree objects), then reverts your working directory to match HEAD, allowing you to switch contexts without committing half-done work. | `git stash` |
| `git stash push -m "[message]"` | Creates a stash with a descriptive message for easier identification when you have multiple stashes, storing it at the top of the stash stack. | `git stash push -m "WIP: login feature"` |
| `git stash -u` | Stashes working directory changes including untracked files by creating a third parent in the stash commit to store the untracked file tree, preserving new files that aren't yet in Git's index. | `git stash -u` |
| `git stash -a` | Stashes all changes including ignored files, creating a complete snapshot of the entire working directory state regardless of .gitignore rules. | `git stash -a` |
| `git stash -p` | Interactively selects which hunks of changes to stash by presenting each change and asking whether to include it, providing granular control over what gets stashed. | `git stash -p` |
| `git stash --keep-index` | Stashes all changes but leaves the staging area (index) intact, useful when you want to stash unstaged changes while keeping staged changes ready for commit. | `git stash --keep-index` |
| `git stash list` | Displays all stashes in the stack with their identifiers (stash@{0}, stash@{1}, etc.), commit messages, and the branch they were created on, stored in the reflog at `.git/refs/stash`. | `git stash list` |
| `git stash show` | Shows the changes recorded in the most recent stash as a diffstat summary, displaying which files were modified and how many lines changed. | `git stash show` |
| `git stash show -p` | Shows the full diff of changes in the most recent stash, displaying the actual line-by-line changes rather than just a summary. | `git stash show -p` |
| `git stash pop` | Applies the most recent stash by merging its working tree state into the current working directory, optionally restores index state, and removes the stash from the stack if successful (leaves it if conflicts occur). | `git stash pop` |
| `git stash pop stash@{n}` | Applies and removes a specific stash identified by its index number, allowing you to apply stashes out of order rather than always using the most recent. | `git stash pop stash@{2}` |
| `git stash apply` | Applies the most recent stash to your working directory without removing it from the stack, allowing you to reuse the same stash across multiple branches. | `git stash apply` |
| `git stash apply stash@{n}` | Applies a specific stash by index without removing it from the stack, useful for applying the same changes to multiple branches. | `git stash apply stash@{1}` |
| `git stash drop stash@{n}` | Permanently deletes a specific stash from the stack without applying it, used to clean up stashes you no longer need. | `git stash drop stash@{0}` |
| `git stash clear` | Permanently deletes all stashes from the stack, removing all stored snapshots—use carefully as this cannot be undone. | `git stash clear` |
| `git stash branch [name]` | Creates a new branch from the commit where the stash was originally created, checks out that branch, applies the stash, and drops it if successful—perfect for when a stash can't apply cleanly to the current branch due to conflicts. | `git stash branch temp-work` |

---

## 🌐 Remote Repositories

| Command | Description | Example |
|---------|-------------|---------|
| `git remote` | Lists all configured remote repositories by their shortnames, reading from `.git/config` where remotes are stored with their URLs and fetch refspecs. | `git remote` |
| `git remote -v` | Lists all remote repositories showing both shortnames and their corresponding URLs for fetch and push operations, providing complete visibility into remote configurations. | `git remote -v` |
| `git remote add [name] [url]` | Adds a new remote repository reference by writing configuration to `.git/config` with remote name, URL, and default fetch refspec (+refs/heads/*:refs/remotes/name/*), establishing connection for future push/pull operations. | `git remote add origin https://github.com/user/repo.git` |
| `git remote remove [name]` | Removes a remote repository reference by deleting its configuration from `.git/config` and removing all associated remote-tracking branches from `refs/remotes/name/`. | `git remote remove origin` |
| `git remote rename [old] [new]` | Renames a remote repository shortname by updating `.git/config` and renaming all remote-tracking branches from `refs/remotes/old/` to `refs/remotes/new/`. | `git remote rename origin upstream` |
| `git remote set-url [name] [url]` | Changes the URL for an existing remote repository by updating the remote.name.url entry in `.git/config`, useful for switching between HTTPS and SSH or updating repository locations. | `git remote set-url origin git@github.com:user/repo.git` |
| `git remote show [name]` | Displays detailed information about a remote including URL, tracked branches, local branches configured for push/pull, and whether remote branches are up-to-date or stale. | `git remote show origin` |
| `git remote prune [name]` | Deletes stale remote-tracking branches that no longer exist on the remote by comparing local `refs/remotes/name/` with the remote's advertised refs, cleaning up outdated branch references. | `git remote prune origin` |
| `git fetch` | Downloads objects and refs from the remote repository by negotiating which objects you need, receiving a packfile with missing objects, and updating remote-tracking branches in `refs/remotes/` without touching your local branches or working directory, making it safe for inspecting changes. | `git fetch` |
| `git fetch [remote]` | Downloads objects and refs from a specific remote repository, updating only that remote's tracking branches. | `git fetch origin` |
| `git fetch --all` | Fetches from all configured remote repositories simultaneously, updating all remote-tracking branches in one operation. | `git fetch --all` |
| `git fetch --prune` | Downloads updates from remote and simultaneously removes remote-tracking branches that no longer exist on the remote, combining fetch with automatic cleanup. | `git fetch --prune` |
| `git fetch --tags` | Downloads all tags from the remote repository in addition to the normal fetch operation, ensuring you have all release markers and annotations. | `git fetch --tags` |
| `git fetch --depth [n]` | Creates or extends a shallow fetch limiting history depth to n commits from branch tips, updating `.git/shallow` file with boundary commits. | `git fetch --depth 5` |
| `git pull` | Performs a two-phase operation by first running `git fetch` to download remote changes, then automatically merging (or rebasing with --rebase) the remote tracking branch into your current local branch, combining fetch and integration in one command. | `git pull` |
| `git pull [remote] [branch]` | Fetches changes from a specific remote branch and merges them into your current branch, allowing explicit control over which remote and branch to pull from. | `git pull origin main` |
| `git pull --rebase` | Fetches remote changes and rebases your current branch onto the updated remote tracking branch instead of merging, maintaining linear history by replaying your local commits on top of fetched changes. | `git pull --rebase` |
| `git pull --ff-only` | Only allows fast-forward merges during pull, aborting if the merge would require a merge commit, providing safety against unexpected merge commits. | `git pull --ff-only` |
| `git push` | Uploads your local branch commits to the remote repository by sending a packfile with missing objects, requesting remote ref updates, and having the remote perform fast-forward checks (unless forced) to prevent overwriting others' work. | `git push` |
| `git push [remote] [branch]` | Pushes a specific local branch to the named remote repository, updating the corresponding remote branch. | `git push origin main` |
| `git push -u [remote] [branch]` | Pushes a branch and sets it as the upstream tracking branch by writing branch.name.remote and branch.name.merge to `.git/config`, enabling argument-less push/pull and tracking status display for future operations. | `git push -u origin feature-login` |
| `git push --all` | Pushes all local branches to the remote repository, updating or creating remote branches to match all your local branches. | `git push --all` |
| `git push --tags` | Pushes all local tags to the remote repository, uploading tag objects and updating remote refs in `refs/tags/`. | `git push --tags` |
| `git push --force` | Forcefully overwrites remote branch even if not fast-forward by disabling safety checks, extremely dangerous as it can destroy others' work by rewriting shared history—avoid on collaborative branches. | `git push --force` |
| `git push --force-with-lease` | Provides safer forced push by checking if the remote ref is at the expected value before overwriting, rejecting the push if someone else has pushed changes you haven't incorporated, protecting against accidentally destroying others' work. | `git push --force-with-lease` |
| `git push --force-with-lease=[ref]:[expect]` | Most explicit and safe forced push form that requires the remote ref to be at a specific SHA-1 before allowing the overwrite, providing precise control over what you're replacing. | `git push --force-with-lease=main:a1b2c3d` |
| `git push [remote] --delete [branch]` | Deletes a remote branch by removing its reference on the remote repository, cleaning up branches that are no longer needed. | `git push origin --delete old-feature` |
| `git push --atomic` | Pushes multiple refs (branches/tags) in an atomic transaction where either all refs update successfully or none update, preventing partial failures in multi-ref pushes. | `git push --atomic` |

---

## 🔍 Inspecting & Comparing

| Command | Description | Example |
|---------|-------------|---------|
| `git log` | Traverses the commit directed acyclic graph (DAG) by following parent links backward from specified commits, displaying commit history with full SHA-1 hashes, author information, dates, and commit messages. | `git log` |
| `git log --oneline` | Displays compact commit history with abbreviated SHA-1 hashes (7 characters) and only the first line of each commit message, providing quick scannable history. | `git log --oneline` |
| `git log --graph` | Draws ASCII art graph representation showing branch and merge structure alongside commit history, visualizing the repository's topology with lines showing parent-child relationships. | `git log --graph` |
| `git log --all` | Shows commits reachable from all refs (branches, tags, remotes) plus HEAD, providing complete repository history rather than just the current branch. | `git log --all` |
| `git log --all --graph --oneline` | Combines compact output, graphical branch visualization, and complete history across all refs, providing the most comprehensive yet readable view of repository structure. | `git log --all --graph --oneline` |
| `git log -n [number]` | Limits output to the specified number of most recent commits, useful for viewing just recent history. | `git log -n 5` |
| `git log --since="[date]"` | Shows commits after the specified date using formats like "2 weeks ago", "2024-01-01", or relative dates. | `git log --since="2 weeks ago"` |
| `git log --until="[date]"` | Shows commits before the specified date, useful for viewing history within date ranges. | `git log --until="2024-01-01"` |
| `git log --author="[name]"` | Filters commits by author name or email using regex pattern matching, allowing you to see contributions from specific developers. | `git log --author="John"` |
| `git log --grep="[pattern]"` | Searches commit messages for the specified regex pattern, finding commits containing specific keywords or references. | `git log --grep="bug fix"` |
| `git log -p` | Shows the full diff (patch) introduced by each commit, displaying not just what changed but the actual code modifications line by line. | `git log -p` |
| `git log --stat` | Displays diffstat for each commit showing which files changed and how many lines were inserted/deleted, providing change magnitude without full diffs. | `git log --stat` |
| `git log --follow [file]` | Traces the history of a file even through renames by detecting when the file was moved or renamed and continuing the history through those operations. | `git log --follow index.html` |
| `git log [branch1]..[branch2]` | Shows commits reachable from branch2 but not from branch1, displaying what's in branch2 that hasn't been merged into branch1. | `git log main..feature` |
| `git log [branch1]...[branch2]` | Shows the symmetric difference—commits reachable from either branch but not both, displaying changes since the branches diverged from their common ancestor. | `git log main...feature` |
| `git log -S"[string]"` | Uses pickaxe search to find commits that changed the number of occurrences of the specified string by comparing old and new file versions, finding when code was added or removed. | `git log -S "password"` |
| `git log -G"[regex]"` | Searches for commits where the diff matches the specified regex pattern, different from -S because it matches diff lines rather than counting occurrences. | `git log -G "function.*login"` |
| `git show [commit]` | Displays detailed information about a Git object by resolving the reference to SHA-1, loading the object, and showing its contents—for commits this means metadata plus diff, for blobs the content, for trees the listing, and for tags the annotation. | `git show a1b2c3d` |
| `git show [commit]:[file]` | Shows the contents of a specific file as it existed in the specified commit, useful for viewing old versions without checking them out. | `git show a1b2c3d:index.html` |
| `git show --stat [commit]` | Shows commit information with diffstat summary of changed files and line counts instead of full diff. | `git show --stat a1b2c3d` |
| `git diff` | Compares working directory against the index (staging area) by loading tree objects, walking trees recursively to find differences, and generating unified diff format showing all unstaged changes. | `git diff` |
| `git diff --staged` | Compares index against HEAD commit, showing what changes are staged and ready for the next commit. | `git diff --staged` |
| `git diff HEAD` | Compares working directory directly against HEAD, showing all changes both staged and unstaged combined. | `git diff HEAD` |
| `git diff [commit1] [commit2]` | Compares two commits by loading their tree objects and generating diffs, showing all differences between two points in history. | `git diff a1b2c3d e4f5g6h` |
| `git diff [branch1] [branch2]` | Shows all differences between the tips of two branches, useful for reviewing what changed between branches. | `git diff main feature` |
| `git diff [branch1]...[branch2]` | Shows changes on branch2 since it diverged from branch1, comparing branch2's tip against the merge base of both branches. | `git diff main...feature` |
| `git diff [file]` | Shows unstaged changes in a specific file only, limiting diff output to one file. | `git diff index.html` |
| `git diff --name-only` | Lists only the names of changed files without showing the actual diffs, providing quick overview of what files changed. | `git diff --name-only` |
| `git diff --name-status` | Shows file names with status indicators (M for modified, A for added, D for deleted), providing more context than name-only. | `git diff --name-status` |
| `git blame [file]` | Annotates each line of a file with the commit SHA-1, author, and timestamp of the last modification by walking back through history and comparing blob objects to identify when each line changed. | `git blame index.html` |
| `git blame -L [start],[end] [file]` | Annotates only specific line ranges using formats like `-L 10,20` (lines 10-20) or `-L :funcname:` (function matching regex). | `git blame -L 10,20 index.html` |
| `git blame -C [file]` | Detects lines moved or copied from other files within the same commit, tracking code movement to find original authorship. | `git blame -C index.html` |
| `git blame -w [file]` | Ignores whitespace changes when determining blame, preventing whitespace reformatting from obscuring actual code authorship. | `git blame -w index.html` |

---

## 🔎 Searching

| Command | Description | Example |
|---------|-------------|---------|
| `git grep "[pattern]"` | Searches tracked files for regex pattern using multi-threaded search that's faster than Unix grep for repository content, searching working directory by default. | `git grep "TODO"` |
| `git grep -n "[pattern]"` | Searches for pattern and prefixes each matching line with its line number in the file. | `git grep -n "function"` |
| `git grep -i "[pattern]"` | Performs case-insensitive search for the pattern across tracked files. | `git grep -i "error"` |
| `git grep -c "[pattern]"` | Shows count of matching lines per file rather than the actual matching lines. | `git grep -c "import"` |
| `git grep "[pattern]" [commit]` | Searches for pattern in tracked files as they existed in a specific commit by loading blob objects from that commit's tree. | `git grep "bug" HEAD~5` |
| `git grep --cached "[pattern]"` | Searches the index (staging area) rather than working directory, showing what will be in the next commit. | `git grep --cached "function"` |
| `git grep -l "[pattern]"` | Lists only filenames containing matches without showing the actual matching lines. | `git grep -l "error"` |
| `git grep --and --or --not` | Combines multiple patterns with boolean operators to perform complex searches like finding files containing both "MAX" and "MIN". | `git grep -e '#define' --and \( -e MAX -e MIN \)` |
| `git grep -p "[pattern]"` | Shows the enclosing function name for each match using hunk header logic from git diff. | `git grep -p "return"` |
| `git log -S"[string]"` | Finds commits that changed the occurrence count of a string (pickaxe search) by generating diffs and counting matches in old vs new versions, useful for finding when functions were added or removed. | `git log -S "loginUser"` |
| `git log -G"[regex]"` | Finds commits where the diff matches a regex pattern, searching actual diff lines rather than counting occurrences like -S. | `git log -G "function.*auth"` |
| `git log -L [start],[end]:[file]` | Traces evolution of a specific line range through history by walking backward through commits and following line movements. | `git log -L 10,20:index.html` |
| `git log -L :[funcname]:[file]` | Traces evolution of a function by name using regex to identify function boundaries. | `git log -L :loginUser:auth.js` |

---

## 🏷️ Tagging

| Command | Description | Example |
|---------|-------------|---------|
| `git tag` | Lists all tags in alphabetical order by reading tag references from `.git/refs/tags/` directory. | `git tag` |
| `git tag [name]` | Creates a lightweight tag—a simple pointer to a commit stored as a 40-byte file in `refs/tags/` containing only the commit SHA-1 without creating a separate tag object. | `git tag v1.0.0` |
| `git tag -a [name] -m "[message]"` | Creates an annotated tag—a full Git object stored in `.git/objects/` containing tag name, tagger info, date, message, and optional GPG signature, pointed to by a ref in `refs/tags/`. | `git tag -a v1.0.0 -m "Release version 1.0.0"` |
| `git tag [name] [commit]` | Creates a tag pointing to a specific commit rather than current HEAD, useful for retroactively tagging historical commits. | `git tag v1.0.0 a1b2c3d` |
| `git tag -d [name]` | Deletes a local tag by removing its reference file from `.git/refs/tags/`, leaving the commit it pointed to untouched. | `git tag -d v1.0.0` |
| `git tag -s [name]` | Creates a GPG-signed tag using your configured key, storing the signature in the tag object for cryptographic verification of authenticity. | `git tag -s v1.0.0` |
| `git tag -v [name]` | Verifies the GPG signature of a signed tag, checking cryptographic authenticity against known keys. | `git tag -v v1.0.0` |
| `git push origin [tag]` | Pushes a specific tag to the remote repository (tags aren't pushed by default with normal push). | `git push origin v1.0.0` |
| `git push origin --tags` | Pushes all local tags (both lightweight and annotated) to the remote repository. | `git push origin --tags` |
| `git push --follow-tags` | Pushes commits and also pushes annotated tags that are reachable from the pushed commits, excluding lightweight tags. | `git push --follow-tags` |
| `git push origin --delete [tag]` | Deletes a tag from the remote repository by removing its reference. | `git push origin --delete v1.0.0` |
| `git checkout [tag]` | Checks out a specific tag, putting you in detached HEAD state at the commit the tag points to. | `git checkout v1.0.0` |
| `git show [tag]` | Shows the tag object's metadata (for annotated tags) plus the commit it points to, or just the commit for lightweight tags. | `git show v1.0.0` |

---

## ⚙️ Configuration

| Command | Description | Example |
|---------|-------------|---------|
| `git config --list` | Displays all Git configuration settings from system (`/etc/gitconfig`), global (`~/.gitconfig`), and local (`.git/config`) config files, with later settings overriding earlier ones. | `git config --list` |
| `git config --global user.name "[name]"` | Sets your name globally in `~/.gitconfig` for all repositories, used in commit metadata to identify the author. | `git config --global user.name "John Doe"` |
| `git config --global user.email "[email]"` | Sets your email globally in `~/.gitconfig` for all repositories, used in commit metadata and GPG key matching. | `git config --global user.email "john@example.com"` |
| `git config --global core.editor "[editor]"` | Sets the default text editor for commit messages, rebase instructions, and other interactive Git operations. | `git config --global core.editor "code --wait"` |
| `git config --global color.ui auto` | Enables colored terminal output for Git commands to improve readability. | `git config --global color.ui auto` |
| `git config --global alias.[alias] [command]` | Creates command aliases by adding entries to `~/.gitconfig`, allowing shorter names for frequently used commands. | `git config --global alias.co checkout` |
| `git config --local [key] [value]` | Sets repository-specific configuration in `.git/config` that overrides global and system settings. | `git config --local user.email "work@example.com"` |
| `git config --global --unset [key]` | Removes a configuration setting from the global config file. | `git config --global --unset user.name` |
| `git config --global init.defaultBranch [name]` | Sets the default branch name for new repositories created with git init. | `git config --global init.defaultBranch main` |
| `git config --global pull.rebase true` | Configures git pull to rebase by default instead of merging, keeping linear history. | `git config --global pull.rebase true` |
| `git config --show-origin [key]` | Shows which config file (system, global, or local) a particular setting comes from. | `git config --show-origin user.name` |

---

## 🧹 Cleanup & Maintenance

| Command | Description | Example |
|---------|-------------|---------|
| `git clean -n` | Performs a dry run showing which untracked files would be removed without actually deleting them, providing safe preview before cleanup. | `git clean -n` |
| `git clean -f` | Removes untracked files from working directory by deleting files not in the index, making the working directory match tracked content. | `git clean -f` |
| `git clean -fd` | Removes both untracked files and directories recursively, cleaning up all untracked content. | `git clean -fd` |
| `git clean -fX` | Removes only ignored files (from .gitignore) while preserving untracked files, useful for cleaning build artifacts. | `git clean -fX` |
| `git clean -fx` | Removes both untracked and ignored files, performing comprehensive cleanup of all non-tracked content. | `git clean -fx` |
| `git gc` | Runs garbage collection to optimize the repository by packing loose objects into packfiles with delta compression, removing redundant packs, pruning unreachable objects older than 2 weeks, and packing refs into `.git/packed-refs`. | `git gc` |
| `git gc --aggressive` | Performs more thorough garbage collection with deeper delta chains (depth 250) and larger window size (250), recomputing all deltas from scratch—much slower but achieves better compression, use sparingly. | `git gc --aggressive` |
| `git gc --auto` | Runs garbage collection only if needed based on thresholds (default 6700 loose objects or 50 packs), used automatically by Git after operations. | `git gc --auto` |
| `git prune` | Removes unreachable objects from the object database that are not referenced by any commit, branch, or tag and are older than the expiration period. | `git prune` |
| `git fsck` | Verifies connectivity and validity of objects in the database by walking all reachable objects and checking for corruption or inconsistencies. | `git fsck` |
| `git fsck --full` | Performs comprehensive repository integrity check including all objects, refs, and index. | `git fsck --full` |
| `git count-objects -vH` | Shows repository size statistics including number of objects, disk usage, pack count, and garbage objects in human-readable format. | `git count-objects -vH` |

---

## 🔧 Advanced & Recovery

| Command | Description | Example |
|---------|-------------|---------|
| `git reflog` | Shows a log of when tips of branches and references were updated in your local repository, stored in `.git/logs/` with each ref having its own log file, providing safety net for recovering lost commits. | `git reflog` |
| `git reflog show [ref]` | Displays the reflog for a specific reference using syntax like `HEAD@{2}` (where HEAD was 2 moves ago) or `master@{one.week.ago}` (master's position one week ago). | `git reflog show HEAD` |
| `git reflog expire` | Manually triggers reflog expiration to clean old entries, normally automatic with 90-day retention for reachable entries and 30 days for unreachable. | `git reflog expire --expire=now --all` |
| `git bisect start [bad] [good]` | Initiates binary search through commit history to find which commit introduced a bug by maintaining refs in `.git/refs/bisect/` and checking out midpoint commits for testing, requiring log₂(n) steps for n commits. | `git bisect start HEAD v1.0` |
| `git bisect bad` | Marks the current commit as containing the bug, narrowing the search space to commits between current and last known good. | `git bisect bad` |
| `git bisect good` | Marks the current commit as not containing the bug, narrowing search space to commits between current and first known bad. | `git bisect good` |
| `git bisect reset` | Ends the bisect session and returns to the original commit before bisect started. | `git bisect reset` |
| `git bisect run [command]` | Automates bisect by running a test script that exits 0 for good, 1-127 for bad (except 125 for untestable), repeatedly halving the search space until finding the culprit commit. | `git bisect run ./test.sh` |
| `git worktree add [path] [branch]` | Creates a linked worktree allowing multiple working directories attached to the same repository, enabling you to check out multiple branches simultaneously with metadata in `$GIT_DIR/worktrees/name/`. | `git worktree add ../feature-work feature` |
| `git worktree list` | Shows all worktrees including main and linked worktrees with their paths, branches, and commit SHAs. | `git worktree list` |
| `git worktree remove [path]` | Removes a linked worktree, deleting the working directory and its metadata. | `git worktree remove ../feature-work` |
| `git worktree prune` | Removes worktree administrative files for deleted worktrees, cleaning up stale metadata. | `git worktree prune` |
| `git filter-repo --path [path]` | Modern history rewriting tool that filters repository to keep only specified paths, operating on fast-export stream for speed, 10-50x faster than deprecated filter-branch. | `git filter-repo --path src/` |
| `git filter-repo --invert-paths --path [path]` | Removes specified paths from entire repository history, rewriting commits to exclude the files. | `git filter-repo --invert-paths --path secrets.txt` |
| `git filter-repo --path-rename [old]:[new]` | Renames paths throughout repository history, useful for restructuring directory layouts. | `git filter-repo --path-rename old/:new/` |

---

## 📦 Submodules

| Command | Description | Example |
|---------|-------------|---------|
| `git submodule add [url] [path]` | Adds a Git repository as a subdirectory of another repository by creating `.gitmodules` entry mapping name to path and URL, storing the submodule commit SHA-1 as gitlink (mode 160000) in parent's tree, and placing the actual submodule repo in `.git/modules/name/`. | `git submodule add https://github.com/user/lib.git lib/` |
| `git submodule init` | Initializes submodule configuration in `.git/config` from `.gitmodules` file, required before first update after cloning a repository with submodules. | `git submodule init` |
| `git submodule update` | Updates submodules to match the commit SHA-1 recorded in the superproject by checking out the exact commit the parent repository specifies. | `git submodule update` |
| `git submodule update --init` | Combines initialization and update, setting up submodule configuration and checking out the correct commits in one command. | `git submodule update --init` |
| `git submodule update --recursive` | Updates submodules and recursively updates any nested submodules within them, handling multi-level submodule hierarchies. | `git submodule update --recursive` |
| `git submodule update --remote` | Updates submodules to the latest commit from their upstream repositories rather than the commit recorded in the superproject, bringing in newest submodule changes. | `git submodule update --remote` |
| `git clone --recurse-submodules [url]` | Clones a repository and automatically initializes and updates all its submodules, handling nested submodules recursively. | `git clone --recurse-submodules https://github.com/user/repo.git` |
| `git submodule foreach [command]` | Executes a shell command in each submodule, enabling batch operations across all submodules. | `git submodule foreach git pull` |
| `git submodule status` | Shows current commit SHA-1 for each submodule with status indicators: `-` for uninitialized, `+` for different commit than recorded, `U` for merge conflicts. | `git submodule status` |
| `git submodule deinit [path]` | Unregisters a submodule by removing its configuration from `.git/config` and clearing its working directory while preserving `.gitmodules` entry. | `git submodule deinit lib/` |

---

## 🔨 Plumbing Commands (Low-Level)

| Command | Description | Example |
|---------|-------------|---------|
| `git rev-parse [revision]` | Resolves Git revision syntax to SHA-1 commit hashes, converting human-readable references like HEAD, branch names, or ref syntax into the underlying 40-character SHA-1 identifiers. | `git rev-parse HEAD` |
| `git rev-parse --git-dir` | Returns the path to the .git directory for the current repository, useful in scripts that need to locate Git's administrative files. | `git rev-parse --git-dir` |
| `git rev-parse --show-toplevel` | Returns the absolute path to the root of the working tree, finding the top-level directory of the repository regardless of current directory. | `git rev-parse --show-toplevel` |
| `git rev-parse --abbrev-ref HEAD` | Returns the symbolic name of the current branch rather than commit SHA-1, showing which branch HEAD points to. | `git rev-parse --abbrev-ref HEAD` |
| `git cat-file -t [object]` | Shows the type of a Git object (blob, tree, commit, or tag) by reading the object header. | `git cat-file -t a1b2c3d` |
| `git cat-file -p [object]` | Pretty-prints the contents of a Git object in human-readable format, showing commit metadata, tree listings, blob contents, or tag information. | `git cat-file -p HEAD` |
| `git cat-file -s [object]` | Shows the size in bytes of a Git object's stored content. | `git cat-file -s HEAD` |
| `git ls-tree [tree]` | Lists the contents of a tree object showing mode (file permissions), type (blob/tree), object SHA-1, and filename, revealing Git's internal directory structure. | `git ls-tree HEAD` |
| `git ls-tree -r [tree]` | Recursively lists all blobs in a tree and its subtrees, showing the complete file structure at a point in history. | `git ls-tree -r HEAD` |
| `git hash-object [file]` | Computes the SHA-1 hash that would be assigned to a file if it were added to Git's object database without actually storing it. | `git hash-object file.txt` |
| `git hash-object -w [file]` | Computes SHA-1 hash and writes the file as a blob object into `.git/objects/`, storing it in Git's database. | `git hash-object -w file.txt` |
| `git update-index --add [file]` | Adds a file directly to the index without using git add, manipulating the staging area at a low level. | `git update-index --add file.txt` |
| `git write-tree` | Creates a tree object from the current index, writing it to the object database and returning its SHA-1 hash. | `git write-tree` |
| `git commit-tree [tree] -m "[message]"` | Creates a commit object directly from a tree SHA-1, parent commit(s), and message without using git commit, providing low-level commit creation. | `git commit-tree abc123 -m "message"` |

---

## 💡 Common Workflow Examples

### Starting a New Project
```bash
git init my-project
cd my-project
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/user/repo.git
git push -u origin main
```

### Feature Branch Workflow
```bash
git checkout -b feature-new-ui
# Make changes
git add .
git commit -m "Add new UI components"
git push -u origin feature-new-ui
# After review, merge via PR or:
git checkout main
git merge --no-ff feature-new-ui
git branch -d feature-new-ui
```

### Fixing a Mistake
```bash
# Undo last commit but keep changes
git reset --soft HEAD~1

# Discard changes to specific file
git restore index.html

# Revert a pushed commit safely
git revert abc123
git push
```

### Syncing with Remote
```bash
git fetch origin
git log HEAD..origin/main  # See what's new
git merge origin/main      # Or: git pull
```

### Cleaning Up History
```bash
# Interactive rebase last 3 commits
git rebase -i HEAD~3

# Squash all branch commits before merge
git checkout main
git merge --squash feature-branch
git commit -m "Add feature"
```

---

## 🎯 Best Practices

- **Commit frequently** with focused changes and clear messages
- **Pull before push** to avoid conflicts and rejected pushes
- **Use branches** liberally for features, fixes, and experiments
- **Never rebase** public branches that others depend on
- **Review before commit** using `git diff --staged`
- **Write descriptive** commit messages explaining why, not what
- **Keep commits atomic**—one logical change per commit
- **Use .gitignore** to exclude generated files and secrets
- **Tag releases** with semantic versioning (v1.2.3)
- **Check status frequently** to understand repository state

---

## 🚨 Recovery Commands

| Situation | Solution |
|-----------|----------|
| **Accidentally deleted a file** | `git restore [file]` or `git checkout HEAD [file]` |
| **Need to undo last commit** | `git reset --soft HEAD~1` (keeps changes) |
| **Made commit on wrong branch** | `git cherry-pick [commit]` on correct branch, then `git reset --hard HEAD~1` on wrong branch |
| **Deleted branch with unmerged work** | `git reflog` to find commit, then `git branch [name] [commit]` |
| **Pushed wrong code to remote** | `git revert [commit]` (safe) or `git push --force-with-lease` (dangerous) |
| **Lost commits after reset** | `git reflog` to find SHA, then `git reset --hard [sha]` |
| **Merge conflicts** | `git merge --abort` to cancel, or resolve then `git add` + `git commit` |
| **Need to unstage all files** | `git reset` or `git restore --staged .` |

---

**Pro Tip:** Understanding Git's three-tree architecture (working directory, index/staging area, and HEAD) and how commands manipulate these trees is key to mastering Git. Commands like reset, restore, and checkout move data between these trees in predictable ways once you grasp the underlying model.
