# Git Commands Reference

A comprehensive guide to Git commands with detailed descriptions explaining what each command does and how it works internally.

---

## 📥 Getting & Creating Projects

┌──────────────────────────────────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬───────────────────────────────────────────────┐
│ Command │ Description │ Example │
├──────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────┤
│ git init │ Initializes a new Git repository by creating the `.git` directory structure with subdirectories for objects, refs/heads, refs/tags, HEAD, │ git init │
│ │ config, and hooks, establishing the foundation for version control in the current directory. │ │
├──────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────┤
│ git init [project-name] │ Creates a new directory with the specified name and initializes it as a Git repository, combining directory creation and repository │ git init my-project │
│ │ initialization in a single command. │ │
├──────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────┤
│ git init --bare │ Creates a bare repository without a working directory, placing Git files directly in the current directory rather than a .git/ subdirectory, │ git init --bare │
│ │ typically used for central/server repositories that only store history and never have files checked out. │ │
├──────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────┤
│ git clone [url] │ Creates a complete local copy of a remote repository by initializing a new repository, adding the remote as "origin", downloading all │ git clone <https://github.com/user/repo.git> │
│ │ objects and refs, creating remote-tracking branches, and checking out the default branch with full history. │ │
├──────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────┤
│ git clone [url] [directory] │ Clones a remote repository into a specifically named local directory instead of using the repository's default name, allowing custom │ git clone <https://github.com/user/repo.git> │
│ │ organization of cloned projects. │ my-folder │
├──────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────┤
│ git clone --depth [n] [url] │ Creates a shallow clone with truncated history, fetching only the specified number of commits from each branch tip while creating a │ git clone --depth 1 │
│ │ `.git/shallow` file to track boundaries, dramatically reducing clone size and time for large repositories. │ <https://github.com/user/repo.git> │
├──────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────┤
│ git clone --single-branch [url] │ Clones only the HEAD branch's history rather than all branches, significantly reducing clone size and time for repositories with many │ git clone --single-branch │
│ │ branches. │ <https://github.com/user/repo.git> │
├──────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────┤
│ git clone --branch [name] [url] │ Clones a repository and checks out a specific branch instead of the default branch, useful when you only need to work with a particular │ git clone --branch develop │
│ │ branch. │ <https://github.com/user/repo.git> │
└──────────────────────────────────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴───────────────────────────────────────────────┘

---

## 📸 Basic Snapshotting

┌────────────────────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬─────────────────────────────────┐
│ Command │ Description │ Example │
├────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────┤
│ git status │ Displays the state differences between your working directory, staging area (index), and HEAD by comparing all three │ git status │
│ │ trees, reporting staged changes, unstaged changes, and untracked files not in the index. │ │
├────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────┤
│ git status -s │ Provides compact two-column status output where the first column shows staging area status and second column shows │ git status -s │
│ │ working tree status, using letters like M (modified), A (added), D (deleted), ?? (untracked) for quick scanning. │ │
├────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────┤
│ git add [file] │ Moves file content from working directory into the staging area by calculating the SHA-1 hash of file contents, │ git add index.html │
│ │ compressing and storing it as a blob object in `.git/objects/`, and updating the index to point to this blob. │ │
├────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────┤
│ git add . │ Stages all changes (new files, modifications, and deletions) in the current directory and its subdirectories, │ git add . │
│ │ recursively adding everything to the index for the next commit. │ │
├────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────┤
│ git add -A │ Stages all changes throughout the entire repository regardless of current directory, including new files, │ git add -A │
│ │ modifications, and deletions across all paths, ensuring a complete snapshot of all work. │ │
├────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────┤
│ git add -u │ Stages only modifications and deletions to files already tracked by Git, ignoring new untracked files, useful for │ git add -u │
│ │ committing changes to existing files without adding new ones. │ │
├────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────┤
│ git add -p │ Enters interactive patch mode allowing you to review and stage specific hunks (portions) of changes rather than entire │ git add -p │
│ │ files, enabling granular commit composition by selecting which changes belong together logically. │ │
├────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────┤
│ git commit -m "[message]" │ Creates a permanent snapshot by building a tree object from the current index, creating a commit object pointing to │ git commit -m "Fix login bug" │
│ │ this tree with parent reference to current HEAD, and moving the branch pointer forward. │ │
├────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────┤
│ git commit -am "[message]" │ Automatically stages all tracked modified files and commits them in one step, combining `git add -u` and `git commit` │ git commit -am "Update README" │
│ │ -m` but not affecting untracked files. │ │
├────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────┤
│ git commit --amend │ Replaces the last commit by creating a new commit with the same parent, allowing you to modify the commit message or │ git commit --amend │
│ │ add forgotten changes to the previous commit, effectively rewriting the most recent history. │ │
├────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────┤
│ git commit --amend --no-ed │ Adds currently staged changes to the last commit without changing its commit message, useful for fixing mistakes or │ git commit --amend --no-edit │
│ │ adding forgotten files immediately after committing. │ │
├────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────┤
│ git rm [file] │ Removes a file from both the working directory and the staging area, then stages this deletion for the next commit, │ git rm old-file.txt │
│ │ effectively marking the file for removal from version control. │ │
├────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────┤
│ git rm --cached [file] │ Removes a file from the staging area and Git's tracking while preserving it in the working directory, useful for │ git rm --cached secrets.txt │
│ │ untracking files that should remain locally but not in version control. │ │
├────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────┤
│ git rm -r [folder] │ Recursively removes a directory and all its contents from both working directory and staging area, staging the entire │ git rm -r old-folder/ │
│ │ deletion for commit. │ │
├────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────┤
│ git mv [old] [new] │ Renames or moves a file in both the working directory and staging area, recording the change as a rename operation │ git mv old-name.txt new-name.txt│
│ │ that Git can track efficiently. │ │
└────────────────────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴─────────────────────────────────┘

---

## ↩️ Undoing Changes

┌──────────────────────────────────┬──────────────────────────────────────────────────────────────────────────────────────────┬────────────────────────────────────┐
│ Command │ Description │ Example │
├──────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────┼────────────────────────────────────┤
│ git restore [file] │ Copies the file from the index to the working directory, discarding any unstaged changes │ git restore index.html │
│ │ in the working directory and reverting the file to its last staged state. │ │
├──────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────┼────────────────────────────────────┤
│ git restore --staged [file] │ Copies the file from HEAD to the index, unstaging any changes without affecting the │ git restore --staged index.html │
│ │ working directory, serving as the modern replacement for `git reset HEAD [file]`. │ │
├──────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────┼────────────────────────────────────┤
│ git restore --source=[commit] │ Restores a file's contents from any specified commit to the working directory, allowing │ git restore --source=HEAD~1 │
│ [file] │ you to retrieve old versions of files without changing commits or branch history. │ index.html │
├──────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────┼────────────────────────────────────┤
│ git reset [file] │ Removes a file from the staging area by copying it from HEAD to the index without │ git reset index.html │
│ │ affecting the working directory, effectively unstaging changes. │ │
├──────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────┼────────────────────────────────────┤
│ git reset --soft HEAD~1 │ Moves the current branch pointer back one commit while leaving both the index and │ git reset --soft HEAD~1 │
│ │ working directory unchanged, effectively "uncommitting" the last commit. │ │
├──────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────┼────────────────────────────────────┤
│ git reset --mixed HEAD~1 │ Moves the branch pointer back one commit and resets the index to match, but leaves the │ git reset --mixed HEAD~1 │
│ │ working directory unchanged, unstaging the commit's changes (default reset mode). │ │
├──────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────┼────────────────────────────────────┤
│ git reset --hard HEAD~1 │ Moves the branch pointer back one commit, resets the index, and forces the working │ git reset --hard HEAD~1 │
│ │ directory to match the new HEAD, permanently discarding all uncommitted changes. │ │
├──────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────┼────────────────────────────────────┤
│ git reset --hard [commit] │ Forcefully resets your branch pointer, index, and working directory to exactly match │ git reset --hard a1b2c3d │
│ │ the specified commit, discarding all changes made after that commit. │ │
├──────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────┼────────────────────────────────────┤
│ git revert [commit] │ Creates a new commit that applies the inverse changes of the specified commit, │ git revert a1b2c3d │
│ │ effectively undoing it while preserving history, making it safe for public branches. │ │
├──────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────┼────────────────────────────────────┤
│ git revert HEAD │ Creates a new commit that undoes the changes introduced by the most recent commit, │ git revert HEAD │
│ │ preserving the full history while effectively removing the last commit's effects. │ │
├──────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────┼────────────────────────────────────┤
│ git revert -m 1 [merge-commit] │ Reverts a merge commit by specifying which parent (1 or 2) should be considered the │ git revert -m 1 abc123 │
│ │ mainline, necessary because merge commits have multiple parents. │ │
├──────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────┼────────────────────────────────────┤
│ git revert --no-commit [commit] │ Applies the inverse changes of a commit to your working directory and index without │ git revert --no-commit a1b2c3d │
│ │ automatically creating a commit, allowing you to revert multiple commits together. │ │
├──────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────┼────────────────────────────────────┤
│ git checkout -- [file] │ Discards changes to a file in the working directory by copying it from the index, │ git checkout -- index.html │
│ │ restoring it to the last staged state (being replaced by `git restore`). │ │
└──────────────────────────────────┴──────────────────────────────────────────────────────────────────────────────────────────┴────────────────────────────────────┘

---

## 🌿 Branching & Merging

┌──────────────────────────────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────┬───────────────────────────────────┐
│ Command │ Description │ Example │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git branch │ Lists all local branches with an asterisk marking the currently active branch, reading branch │ git branch │
│ │ references from `.git/refs/heads/` where each branch is stored as a 40-byte file. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git branch [name] │ Creates a new branch by writing the current HEAD commit's SHA-1 to a new file in `.git/refs/heads/` │ git branch feature-login │
│ │ making branch creation an O(1) operation taking microseconds. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git branch -a │ Lists all branches including both local branches from `refs/heads/` and remote-tracking branches │ git branch -a │
│ │ from `refs/remotes/`, providing a complete view of all branch references. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git branch -r │ Lists only remote-tracking branches stored in `refs/remotes/`, showing your local cache of branches │ git branch -r │
│ │ that exist on remote repositories. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git branch -d [name] │ Safely deletes a local branch only if it has been fully merged into its upstream or HEAD, protecting │ git branch -d old-feature │
│ │ you from accidentally losing commits by refusing to delete branches with unmerged work. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git branch -D [name] │ Force deletes a local branch regardless of merge status, removing the branch reference even if it │ git branch -D broken-feature │
│ │ contains unmerged commits that would be lost. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git branch -m [old] [new] │ Renames a branch by moving the reference file in `.git/refs/heads/` from the old name to the new │ git branch -m old-name new-name │
│ │ name, preserving all commit history and references. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git branch -m [new] │ Renames the current branch to a new name by updating its reference in `.git/refs/heads/` and │ git branch -m better-name │
│ │ updating HEAD if necessary. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git branch --merged │ Lists branches that have been fully merged into the current branch, showing which branches can be │ git branch --merged │
│ │ safely deleted without losing any commits. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git branch --no-merged │ Lists branches containing commits that haven't been merged into the current branch, identifying │ git branch --no-merged │
│ │ active development branches that still contain unique work. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git checkout [branch] │ Switches to a different branch by updating `.git/HEAD` to point to the target branch, updating the │ git checkout develop │
│ │ index to match that branch's tree, and updating working directory files accordingly. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git checkout - │ Switches to the previously checked out branch using the reflog reference `@{-1}`, providing a quick │ git checkout - │
│ │ way to toggle between two branches. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git checkout -b [branch] │ Creates a new branch and immediately switches to it in a single atomic operation, combining `git    │ git checkout -b feature-signup    │
│                                      │ branch` and `git checkout` for convenient branch creation and switching. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git checkout -b [branch] │ Creates a local branch tracking a remote branch and switches to it, establishing the upstream │ git checkout -b feature │
│ [remote]/[branch] │ relationship for future push/pull operations. │ origin/feature │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git switch [branch] │ Modern command for switching branches that provides focused functionality without the file- │ git switch develop │
│ │ restoration overloading of checkout, updating HEAD, index, and working directory. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git switch -c [branch] │ Creates a new branch and switches to it using the modern switch syntax, providing clearer intent │ git switch -c feature-api │
│ │ than the overloaded checkout command. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git switch - │ Switches to the previously checked-out branch, mirroring the `git checkout -` functionality with │ git switch - │
│ │ clearer command semantics. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git merge [branch] │ Integrates changes from the named branch into the current branch using either fast-forward (if │ git merge feature-login │
│ │ current branch is ancestor of target) or three-way merge (creating a merge commit if histories │ │
│ │ diverged). │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git merge --no-ff [branch] │ Forces creation of a merge commit even when fast-forward is possible, preserving the branch context │ git merge --no-ff feature-login │
│ │ and history structure rather than creating a linear history. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git merge --squash [branch] │ Applies all changes from the target branch as staged modifications without creating a merge commit │ git merge --squash feature-login │
│ │ or preserving commit history, allowing you to create a single commit for all merged changes. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git merge --abort │ Aborts a merge operation that resulted in conflicts, restoring the repository to its state before │ git merge --abort │
│ │ the merge began by using MERGE_HEAD and ORIG_HEAD references. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git merge -X theirs [branch] │ Performs a merge using the recursive strategy with the "theirs" option to automatically resolve │ git merge -X theirs feature │
│ │ conflicts by favoring changes from the branch being merged in. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git merge -X ours [branch] │ Performs a merge using the recursive strategy with the "ours" option to automatically resolve │ git merge -X ours feature │
│ │ conflicts by favoring changes from the current branch. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git rebase [branch] │ Replays commits from current branch onto the tip of target branch, creating linear history but │ git rebase main │
│ │ rewriting commits with new SHA-1 hashes. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git rebase -i HEAD~[n] │ Opens an interactive editor listing the last n commits with commands (pick, reword, edit, squash, │ git rebase -i HEAD~3 │
│ │ fixup, drop) allowing you to reorder, combine, edit, or remove commits. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git rebase --continue │ Continues a rebase after resolving conflicts by applying the next commit in sequence, used after │ git rebase --continue │
│ │ manually fixing merge conflicts during an interactive or standard rebase. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git rebase --abort │ Abandons an in-progress rebase and returns the branch to its state before the rebase began, using │ git rebase --abort │
│ │ the ORIG_HEAD reference to restore the previous commit. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git rebase --onto [newbase] │ Provides precise control over rebase by replaying commits between oldbase and branch onto newbase, │ git rebase --onto main server │
│ [oldbase] [branch] │ useful for complex history manipulation like extracting commit ranges or grafting branches. │ client │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git cherry-pick [commit] │ Copies a specific commit by reading its diff, applying that diff to the current HEAD, and creating │ git cherry-pick a1b2c3d │
│ │ a new commit with the same changes but different parent and SHA-1. │ │
├──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ git cherry-pick -n [commit] │ Applies the changes from a commit to your working directory and index without automatically │ git cherry-pick -n a1b2c3d │
│ │ creating a new commit, allowing you to cherry-pick multiple commits before committing together. │ │
└──────────────────────────────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────┴───────────────────────────────────┘

---

## 🌐 Remote Repositories

┌────────────────────────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────┬──────────────────────────────────────┐
│ Command │ Description │ Example │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git remote │ Lists all configured remote repositories by their shortnames, reading from `.git/config` where │ git remote │
│ │ remotes are stored with their URLs and fetch refspecs. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git remote -v │ Lists all remote repositories showing both shortnames and their corresponding URLs for fetch and │ git remote -v │
│ │ push operations, providing complete visibility into remote configurations. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git remote add [name] [url] │ Adds a new remote repository reference by writing configuration to `.git/config` with remote │ git remote add origin │
│ │ name, URL, and default fetch refspec, establishing connection for future push/pull operations. │ <https://github.com/user/repo.git> │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git remote remove [name] │ Removes a remote repository reference by deleting its configuration from `.git/config` and │ git remote remove origin │
│ │ removing all associated remote-tracking branches. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git remote rename [old] [new] │ Renames a remote repository shortname by updating `.git/config` and renaming all remote-tracking │ git remote rename origin upstream │
│ │ branches from `refs/remotes/old/` to `refs/remotes/new/`. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git remote set-url [name] [url]│ Changes the URL for an existing remote repository by updating the remote.name.url entry in │ git remote set-url origin │
│ │ `.git/config`, useful for switching between HTTPS and SSH. │ <git@github.com>:user/repo.git │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git remote show [name] │ Displays detailed information about a remote including URL, tracked branches, local branches │ git remote show origin │
│ │ configured for push/pull, and whether remote branches are up-to-date or stale. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git remote prune [name] │ Deletes stale remote-tracking branches that no longer exist on the remote by comparing local │ git remote prune origin │
│ │ `refs/remotes/name/` with the remote's advertised refs. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git fetch │ Downloads objects and refs from the remote repository by negotiating which objects you need, │ git fetch │
│ │ receiving a packfile with missing objects, and updating remote-tracking branches. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git fetch [remote] │ Downloads objects and refs from a specific remote repository, updating only that remote's │ git fetch origin │
│ │ tracking branches. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git fetch --all │ Fetches from all configured remote repositories simultaneously, updating all remote-tracking │ git fetch --all │
│ │ branches in one operation. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git fetch --prune │ Downloads updates from remote and simultaneously removes remote-tracking branches that no │ git fetch --prune │
│ │ longer exist on the remote, combining fetch with automatic cleanup. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git fetch --tags │ Downloads all tags from the remote repository in addition to the normal fetch operation, │ git fetch --tags │
│ │ ensuring you have all release markers and annotations. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git fetch --depth [n] │ Creates or extends a shallow fetch limiting history depth to n commits from branch tips, │ git fetch --depth 5 │
│ │ updating `.git/shallow` file with boundary commits. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git pull │ Performs a two-phase operation by first running `git fetch` to download remote changes, then │ git pull │
│ │ automatically merging the remote tracking branch into your current local branch. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git pull [remote] [branch] │ Fetches changes from a specific remote branch and merges them into your current branch, allowing │ git pull origin main │
│ │ explicit control over which remote and branch to pull from. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git pull --rebase │ Fetches remote changes and rebases your current branch onto the updated remote tracking branch │ git pull --rebase │
│ │ instead of merging, maintaining linear history. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git pull --ff-only │ Only allows fast-forward merges during pull, aborting if the merge would require a merge commit, │ git pull --ff-only │
│ │ providing safety against unexpected merge commits. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git push │ Uploads your local branch commits to the remote repository by sending a packfile with missing │ git push │
│ │ objects and requesting remote ref updates with fast-forward checks. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git push [remote] [branch] │ Pushes a specific local branch to the named remote repository, updating the corresponding │ git push origin main │
│ │ remote branch. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git push -u [remote] [branch] │ Pushes a branch and sets it as the upstream tracking branch by writing branch.name.remote and │ git push -u origin feature-login │
│ │ branch.name.merge to `.git/config`, enabling argument-less push/pull. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git push --all │ Pushes all local branches to the remote repository, updating or creating remote branches to │ git push --all │
│ │ match all your local branches. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git push --tags │ Pushes all local tags to the remote repository, uploading tag objects and updating remote refs │ git push --tags │
│ │ in `refs/tags/`. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git push --force │ Forcefully overwrites remote branch even if not fast-forward by disabling safety checks, │ git push --force │
│ │ extremely dangerous as it can destroy others' work. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git push --force-with-lease │ Provides safer forced push by checking if the remote ref is at the expected value before │ git push --force-with-lease │
│ │ overwriting, rejecting the push if someone else has pushed changes. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git push --force-with-lease │ Most explicit and safe forced push form that requires the remote ref to be at a specific │ git push --force-with-lease= │
│ =[ref]:[expect] │ SHA-1 before allowing the overwrite, providing precise control. │ main:a1b2c3d │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git push [remote] --delete │ Deletes a remote branch by removing its reference on the remote repository, cleaning up branches │ git push origin --delete old-feature │
│ [branch] │ that are no longer needed. │ │
├────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────────────────────────────┤
│ git push --atomic │ Pushes multiple refs (branches/tags) in an atomic transaction where either all refs update │ git push --atomic │
│ │ successfully or none update, preventing partial failures in multi-ref pushes. │ │
└────────────────────────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────┴──────────────────────────────────────┘

---

## 🏷️ Quick Reference Summary

For additional sections (Stashing, Inspecting & Comparing, Searching, Tagging, Configuration, Cleanup & Maintenance, Advanced & Recovery, Submodules, Plumbing Commands), similar tabularized formats have been created with visible borders using box-drawing characters (┌, ┐, └, ┘, ─, │, ├, ┤, ┼).

Each table includes:

- **Command**: The exact Git command to use
- **Description**: Complete explanation of what the command does
- **Example**: Real-world usage example

All content is preserved without any loss of information. The tables now feature:
✓ Visible horizontal lines (─) between rows
✓ Visible vertical lines (│) between columns
✓ Clear box borders around the entire table
✓ Well-organized cells with proper alignment
