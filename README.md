# GiT-reminds-commands
#### Note
##### I use my own knowledge and experience to describe the commands I use as my reminder.

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
  - `-s` flag is short for "short" provide a shorter & more concise summary of the *changes* in my *working directory*; Git shows 3 columns:
    - The first column indicates the status of the file in the *staging area* (whether the file is new, modified, deleted, renamed, or copied).
      - A green *M* letter means a file is modified and staged into the *staging area*. 
    - The second column indicates the status of the file in the *working directory* (whether the file is new, modified, deleted, renamed, or copied).
      - A red *M* letter means a file is modified but is only in the *working directory* & not *staging area*.
      - Double question marks *??* means file is recently created & is *untracked* in the *staging area*
    - The third column indicates the name of the file.

- `git branch` to create, list, rename, and delete branches. The default command without flags will show a list of all branches in the repository; the branch prefixed with an asterisk sign (\*) is the current branch I am in.
  - `-a` flag would show the *remote* branches as well.
  - `-m` flag is used to rename the *current* branch.
  - `-d` flag deletes a branch *other* than the branch I'm currently at -> *note:* to delete the current branch, a workaround is to first switch to a different branch first.
  - `git branch new-feature` the branch named *new-feature* will be created and will point to the same commit as the current branch -> *note* this command *won't* switch to the new branch automatically.
- `git switch` is used to *switch* or *check out* at a specific branch. Introduced in same version as *git restore* v2.23.
  - If I have *uncommitted changes* in my *working directory*, I won't be able to use *git switch* to switch branches; in that case I would need to *commit* my changes or to *stash* them before switching to a different branch.
  - *git switch* needs to be provided with the correct branch name as it won't create a new one if it doesn't exist.
  - If there are conflicts between my current branch and the branch I'm trying to switch onto, an error will be thrown and I'd have to resolve that first.
  - *Only* works with branches and can't use it for switching commits like *git checkout* but a better alternative is the *git restore --source* flag.
 - `git checkout` *switches* to the specified branch and *updates* the *working directory* to *match* the *contents* of *that* branch. It is an older command in Git that can be used to *switch* between branches or to *check out* specific commits; its functionality is split into *git restore* && *git switch*.
   - `-b` flag creates a new branch with the specified name, pointing to the current commit, and switches to the new branch; while capital "b" a `-B` flag would overwrite a branch even if it exists and already has contents in it; to create a branch without switching to it automatically use *git branch new-feature*
- `git merge` merges the specified branch into the current branch. It can lead to merge conflicts that would need to be solved manually; here is a YouTube video by freeCodeCamp that shows <a href="https://youtu.be/RGOj5yH7evk?t=2950">how to solve merging conflict</a> at 49:00 into the video.
- - `git diff` is used to *show differences* between two versions of a file, or between two branches or commits in a Git repository; it means I can use *commit hash* or *HEAD*s
  - `--staged` flag shows the differences between the *staging area* and the most recent commit (even if they were *modified* or *added* or *deleted*).
- `git ls-trees` lists all the *files* in a *tree* from a commit (either by *HEAD* or its *UI* AKA Unique identifier AKA hash which is generated by Git)
  - The second column in the list refers to the *type* of the *object* and it can be '*blob*' or '*tree*':
    - *blob* represents a file
    - *tree* represents a folder (/directory)
    - *Types* can also be *commits* or *tags* which are less common.
  - *git show* command followed by the *hash*/*UI* can be used to view an object.
- `git ls-files` shows the files in the *staging area* EVEN IF they are removed from *working directory*; these are the staged changes (*git add*)
  - If *file.js* was removed from *working directory* then I'm required to run *git add* to stage those changes & they will show up in *git status* as *changes to be commited* (or *green 'D'* if used with *-s* flag).
- `ls` lists files. Without flags it lists all files in the current directory except for hidden files.
  - `-a` flag shows hidden files (usually files starting with a dot (.))
---
*Below are the Git commands that I haven't yet needed to apply them in my projects*:
- `git cherry-pick` git cherry-pick command followed by the *commit hash* of the commit I want to apply it to; useful when I'd want to apply a bug fix (*hotfix* is the term) or a new feature from one branch to another *without* merging the entire branch.
  - "Cherry picking" is the act of selecting a specific commit from one branch and applying it to another branch.
  - It can be used even for fixing a *typo*; for ex.: if I have 2 branches "*main*" and "*dev"* branch, if there is a *typo* in the "*main*" branch that has already been fixed in the "*dev*" branch I can use *git cherry-pick* command to *apply* the *typo fix* commit from the "*dev*" branch into the "*main*" branch; altough this is a small change so it most likely will be just fixed in the current branch.
- `git stash` will *save* my *changes* and *revert* my *working directory* to the *last commit* -> later, when I'd be ready to continue working on my feature, I can *apply* the *stash* using the `git stash apply` command. This will *restore* my *saved changes* into my *working directory*.
  - "Stashing" is the act of *temporarily saving changes* that I've made to my *working directory* without committing them.
  - Git saves my changes in a *hidden stash entry*, which is stored in the Git repository. The *stash entry* itself is a *commit object* that contains the changes I've made to my *working directory*, but it is not stored in any branch or commit history. Instead, it is kept separate from the *current branch*, allowing me to *temporarily set aside changes* and I can switch to a different branch or work on a different task without having to commit my changes to the *current branch* -> *note*: the *stash entry* is *not* associated with any branch until I apply it to a branch using `git stash apply` or `git stash pop` command.
    - While similar, the differences between those 2 commands are: "*git stash apply*" leaves the *stash entry* in the *stash list* so I can *apply* it again later if needed, while "*git stash pop*" removes the *stash entry* from the *stash list* & I must be sure that I would no longer need to *apply stash entry*.
  - `git stash list` shows a list of all the stash entries in my repository, including their index numbers, the branch on which they were stashed, and a short description of the changes that were stashed.
  - `git stash drop` removes the *stash entry* from the *stash list* (Git repository).
  -  `git stash show` to inspect the changes in the stash entry before deciding whether to drop it.









