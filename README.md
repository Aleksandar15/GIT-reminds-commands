# GiT-reminds-commands
##### Note
I use my own knowledge and the internet to describe the commands to use as my reminder.

- `git revert` **creates** a new commit that undoes the changes made by a previous commit, *without* actually removing the commit from my repository's history (unlike `git reset` which rewrites repository's history and requires to use `-f` on push). First switch to the branch I'd like to undo a commit, revert, and push.
- `git reset` moves HEAD pointer to a different commit AKA undoing the commit that I want to remove. 
    - *Default* flag: `--mixed` will undo the most recent commit and *unstage* the changes, *but leave* the changes in my *working directory*; use case: splitting a commit into multiple commits: if I made several changes in a single commit and I want to split them into multiple commits -> I can then *stage* the specific ones again using *git add*, and then *commit* them again. 
    - On the softer side `--soft` flag will will undo the most recent commit, but *leave* the changes in both *staging area* & *working directory*; use case ex.: when I want to change the commit *message* of a previous commit. 
  - `--hard` flag will undo the most recent commit and discard *all* changes, both in the *staging area* & *working directory*, means permanently deleting the changes from *working (local) directory*; use case: if I have merged two branches together and later realize that the merge was a mistake and I rather *revert* back in a destructive way.
 - `git reflog` shows a log of all the actions that have changed the head of the repository, including commits, merges, rebases, and even branch checkouts. It can be used to recover lost commits, branches, or changes that were accidentally deleted or overwritten; for ex.: to reverse accidental `git reset --hard` but it will *only* work if I have committed changes before *git reset --hard*.
-`git log` shows the commit history of the current branch, and it displays the commit messages, author, date, and other relevant information.
- `git commit -am` shorthand got performing *git add* and *-m* flag of the commit ; useful for when there's no untracked files (recently created) but rather *modified* or *deleted* files yet to be staged for commit, as it will not *add* or "stage" any new files that have *not* been previously tracked by Git (requires manual *git add*).




