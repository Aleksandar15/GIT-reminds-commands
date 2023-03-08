# GiT-reminds-commands
#### Note
I use my own knowledge and experience to describe the commands I use as my reminder.

- `git revert` **creates** a new commit that undoes the changes made by a previous commit, *without* actually removing the commit from my repository's history (unlike `git reset` which rewrites repository's history and requires to use `-f` on push). First switch to the branch I'd like to undo a commit, revert, and push.
- `git reset` moves HEAD pointer to a different commit AKA undoing the commit that I want to remove. 
    - *Default* flag: `--mixed` will undo the most recent commit and *unstage* the changes, *but leave* the changes in my *working directory*; use case: splitting a commit into multiple commits: if I made several changes in a single commit and I want to split them into multiple commits -> I can then *stage* the specific ones again using *git add*, and then *commit* them again. 
      -  Another example can be merging conflicts: if I am on a branch and need to merge changes from another branch, I can use *git reset --mixed* to *unstage* the *changes* and resolve the conflicts manually in my *working directory*.
    - On the softer side `--soft` flag will will undo the most recent commit, but *leave* the changes in both *staging area* & *working directory*; use case ex.: when I want to change the commit *message* of a previous commit. 
  - `--hard` flag will undo the most recent commit and discard *all* changes, both in the *staging area* & *working directory*, means permanently deleting the changes from *working (local) directory*; use case: if I have merged two branches together and later realize that the merge was a mistake and I rather *revert* back in a destructive way.
 - `git reflog` shows a log of *all* the *actions* that have changed the head of the repository, including commits, merges, rebases, and even branch checkouts; it can be used to recover lost commits, branches, or changes that were accidentally deleted or overwritten; for ex.: to reverse accidental `git reset --hard` *but* it will *only* work if I have committed changes before *git reset --hard*.
- `git log` shows the commit history of the current branch, and it displays the commit messages, author, date, and other relevant information.
- `git add` will *stage* files; use cases: before making a commit I need to add the files from my *working directory* into the *staging area* also called *index* or *cache*.
- `git commit` used with the `-m` flag for providing a *message*, will save changes ('snapshot') from *staging area* to the *local "repository"(database)*.
- `git commit -am` shorthand got performing *git add* and *-m* flag of the commit ; useful for when there's no untracked files (recently created) but rather *modified* or *deleted* files yet to be staged for commit, as it will not *add* or "stage" any new files that have *not* been previously tracked by Git (requires manual *git add*).
- `git clean` must be used with the `-f` flag to remove *untracked files* from *working directory*; used for removing recently created files that exist *only* in the *working directory* so a simple *git restore file.js* will *not* work because Git doesn't know where to restore it from. 
  - It's an irreversible command, so a *git checkout file.js* to restore the file from the most recent commit *perhaps* will *not* work as it is a recently created file.
- `git rm` to remove file from *staging area* must be used with either `--cached` to keep the file in *working directory*, or `-f` to force removal from both *working directory* & *staging area*
  - If I have committed the changes and this *-f* removal was done by mistake: I have the option to *git revert* to *uncommit*/undoing a commit; or *git restore --source=HEAD file.js* to restore the removed file from the last commit.
  - If *--cached* was a mistake, I can just re-do *git add*.
- `git revert` to *uncommit*/undo a commit; Git does this by committing a "*reverted commit*". By default Git will add "Revert" to the message of the commit-being-reverted or otherwise I might add *-m* flag and write my own one.
  - Now if I switch into a commit by *git reset --soft #hash* to the commit before the mistaken commit & reverted mistaken commit; running *git log* won't show those  2 commits but they will all show up in *git reflog* in case I need, there's the full history.
- `git restore` is used to restore files in my *working directory* to a previous *state* 
  - A "state" can be a *commit* or *branch* or *index(/staging area)*; in a case of using *git add* the "state" includes the changes that have been staged in the *staging area*; if then I *git commit* - the "state" would refer to the files in the commit AKA the '*snapshot*' in the *local repository*.
  - By default if "state" or `--source` flag is not provided then Git will *restore* the file(s) from the *next area*; ex: when *file*'s changes are in the *working directory* Git will try to restore it from *staging area* but the *default*'s tricky part is that if I re-run *git restore file1.js* it will not go the next area - because, reminder: it doesn't *unstage* changes by *default* - unless & until I unstage the *file.js* (with *git restore --staged file.js*), so basically the *staging area* would be the limit in *default* behaviour unless I use the *--staged* flag; if there aren't staged changes of that file Git will try to restore it from the 'snapshot' in the *local repository*.
  - *NOTE*: it will work similarly (but not the same) as *git reset --hard* & tests:
    - If I had the *file.js* in the *staging area* (previously ran *git add file.js*) and I modified *file.js* for the second time, but I haven't yet added it to the *staging area* (haven't yet re-run *git add file.js*) and I run `git restore file.js` it will restore the changes from my *staging area* basically removing my last *working directory*'s changes to my *file.js* (changes which were not staged).
    - If I ran `git restore --source=HEAD~1 file.js` to restore a file to the "state" that was in the second-to-last AKA previous commit there is 2 behaviours that can happen:
      - First, if changes made in *working directory* to *file.js* were not yet staged (*git add*) AKA *staging area* is clean and *git status* shows '*changes not staged for commit*: *file1.js*' the above command would simply restore *file.js* to the *HEAD~1* commit's snapshot: *file.js* in the *working directory* will be restored && Git repository will be clean, *git status* shows '*On branch X nothing to commit, working tree clean*'.
      - Second, if changes made in *working directory* to *file.js* were staged by *git add file.js* && *git status* shows '*changes to be committed*: *file.js*'; the command above will again restore *file.js* to the *HEAD~1*'s commit snapshot but *git status* shows *file.js* in both fields:'*changes to be commited*'&'*changes not staged for commit*' - because the *working directory* differentiates from *staging area*. *EXTRA*: *git restore --staged file.js* won't modify *working directory* but will clean the *git status* by *unstaging changes* if such behaviour is wanted (*note*: *git reset --mixed* would do the same job here but it's an older command and I can't specify a file(s)); otherwise run *git restore file.js* if the *--source* command was an accident & I want to restore my *file.js* from my *staging area* into my *working directory*.
    - Considering these things, if I were to switch *HEAD*s only to *restore* files (by *git reset --soft #hash*; or *git branch*; or *git switch*, etc.), if in *staging area* I have the files I don't want to commit -> I would first need to unstage them (*git restore --staged*) & then *git restore file.js*, otherwise the *staging area* would be there as the "wall"/limit and would not be able to restore files from local repository's/commit's *snapshot* unless I free the path to the commit-snapshot by unstaging unwanted changes.
    - It works only for tracked files; for removed files use *git clean -f* or *git rm -f*.
    - *git restore* requires Git version 2.23 or higher.
- `git restore --staged` unstages the changes that were staged into the *staging area* (huh). Humanly: cleans the *git status* from '*changes to be committed*'
- `git status` shows the current status of the Git *repository* of the checked out (current) branch: the changes made in the *working directory* against the last commit & whether they are staged for the next commit or not.
  - *Untracked files* are the recently created files which are not yet staged (*git add*-ed).
- `git checkout`
- `git branch`
- `git switch`
- `git merge`
- `git merge`




