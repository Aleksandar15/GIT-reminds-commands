# GIT-reminds-commands
#### Note
##### I use my own knowledge and experience to describe the commands I use as my reminder.

- `git reset` moves HEAD pointer to a different commit AKA undoing the commit that I want to remove. 
    - ***Default*** flag: `--mixed` will undo the most recent commit and *unstage* the changes, *but leave* the changes in my *working directory*; use case: splitting a commit into multiple commits: if I made several changes in a single commit and I want to split them into multiple commits -> I can then *stage* the specific ones again using *git add*, and then *commit* them again. 
      -  Another example can be merging conflicts: if I am on a branch and need to merge changes from another branch, I can use *git reset --mixed* to *unstage* the *changes* and resolve the conflicts manually in my *working directory*.
         - `--mixed` being the default flag I can use simply `git reset`; use case: to *unstage all files* from *staging area* but keeps the local changes untouched which, again, means: the changes will be removed from the staging area, but I will still have the modified files in my local workspace.
    - On the softer side `--soft` flag will will undo the most recent commit, but *leave* the changes in both *staging area* & *working directory*; use case ex.: when I want to change the commit ***message*** of a previous commit - for example:
      - *git reset --soft `HEAD~1`* or *`HEAD^`* can be both used to "*uncommit*" and just re-do *`git commit -m`* with a different **message** provided.
        - Note that both *HEAD* pointers achieve the same results as *HEAD~1* refers to the commit one generation behind; while *HEAD^* refers the parent commit of the current commit (and *HEAD^^* to the grandparent, etc.).
  - `--hard` flag will undo the most recent commit and discard *all* changes, both in the *staging area* & *working directory*, means permanently deleting the changes from *working (local) directory*; use case: if I have merged two branches together and later realize that the merge was a mistake and I rather *revert* back in a destructive way.
  - New experience with *git reset* command: Scroll down below to the "[extra experiences](https://github.com/Aleksandar15/GIT-reminds-commands/edit/main/README.md#extra-experiences)".
- `git branch -r` checks which branches exists in my remote repository unlike `git branch` that shows branches in my local repository.
 - `git reflog` shows a log of *all* the *actions* that have changed the head of the repository, including commits, merges, rebases, and even branch checkouts; it can be used to recover lost commits, branches, or changes that were accidentally deleted or overwritten; for ex.: to reverse accidental `git reset --hard` *but* it will *only* work if I have committed changes before *git reset --hard*.
- `git log` shows the commit history of the current branch, and it displays the commit messages, author, date, and other relevant information.
    - `git log --oneline --decorate --graph`: this command will show a compact log with _one line per commit_, displaying only the _commit hash_ and the _commit **message**_. The `--decorate` option adds information about _branches_ and _tags_, and the `--graph` option draws a text-based _graph_ to represent _commit history_. 
- `git add` will *stage* files; use cases: before making a commit I need to add the files from my *working directory* into the *staging area* also called *index* or *cache*.
- `git commit` used with the `-m` flag for providing a *message*, will save changes ('snapshot') from *staging area* to the *local "repository"(database)*.
- `git commit -am` shorthand got performing *git add* and *-m* flag of the commit ; useful for when there's no untracked files (meaning: _recently created new files_) but rather *modified* or *deleted* files yet to be staged for commit, as it will not "*add*" or "stage" any new files (because they are "_untracked_") that have *not* been previously tracked by Git (requires manual *git add*).
- `git push` push changes to the previously set upstream branch.
  - If it's not then Git will throw error fatal: *The current branch main(/branch-name) has no upstream branch.* & suggests *To push the current branch and set the remote as upstream use* command *git push --set-upstream origin main*.
   - For ex.: if I am on the '*feature*' branch and I have previously set '*'origin/feature*' as the *upstream branch* for '*feature*', running *git push* will push the changes I've made on the '*feature*' branch onto the '*origin/feature*' branch on the '*remote*' repository.
     - `-u` flag short for `--set-upstream` Git will remember the '*remote branch*' that the '*local branch*' is tracking. For ex.:*git branch --set-upstream-to=origin/feature feature* allows me next time(s) to run *git push* without any flags & it will push changes to "*origin/feature*".
     - In Git `origin` is a commonly used name for the default remote repository.
       - A *remote repository* is a repository that is hosted on a different machine or server, such as on GitHub or GitLab.
       - When I'd clone a repository from a remote repository, Git automatically sets up a "*remote*" called "*origin*" that *points* to the repository I cloned from. This allows me to *push* and *pull* changes to and from the *remote repository*. `git push origin main`: Git will *push* the changes *made* on the "*main" branch* to the '*remote' repository* named "*origin*".
   - *When* or *how often* do I push changes? I can choose between pushing on every *single commit*, but it's also common to stack up a few *changes* (*commits*) and on the last (bigger) *commit* to do the *pushing*.
   - *NOTE*: the most confusing part about GitHub & Bitcuket can be the fact that "*pull request*" term stands for "*merge request*" as is called in cloud like GitLab; essentially both terms mean the same thing: *a request to merge changes from one branch into another*.
   - A great blog about <a href="https://www.atlassian.com/git/tutorials/syncing/git-pull">git push</a>.
- `git pull` is used to update my local repository with changes from a remote repository (/cloud/distributed repository like GitHub or GitLab). 
  - `git pull` command is a combination of 2 other commands: `git fetch` followed by `git merge`.
    - In the first stage of operation `git pull` will execute a `git fetch` scoped to the *local branch* that *HEAD is pointed at* -> once the content is downloaded, `git pull` will do `git merge` where a new merge commit will be-created and *HEAD* updated to *point at the new commit
    - Example: `git pull origin main` equals to process of:
        - `git fetch origin main` or rather `git fetch origin/main` command fetches the latest changes from the `main` branch in the remote repository (`origin`) without merging them into your current branch. It updates my remote-tracking branch `origin/main` to match the latest state of the `remote main` branch.
        - `git merge origin main` or rather `git merge origin/main` command merges the changes from the `main` branch in the `remote` repository (`origin`) into your current branch. It combines the commit histories of both branches, bringing the changes from `main` into your branch. -> This command incorporates the new commits from the `origin/main` branch into my local branch. It performs a three-way `merge`, combining the changes from the `remote` branch with your local branch's commits. This preserves your local commits and allows you to integrate the new changes while maintaining a history of my work.
  - When doing *git pull* a *merge conflict* can occur -> Git will *pause* the *merge process* and notify me of the conflict; I can use the *git status* command to see which *files* have conflicts; it will show me "*unmerged paths*" which means these are *files* with *conflicts* that need to be *resolved* in a way that makes sense for the project.
    - Since *git merge* is the second part of the process; with *git pull* Git will create a new commit that includes both my *local changes (working directory)* & the changes I've *pulled* from the *remote repository*. 
      - To clarify any possible confusion: it does *not* overwrite anything from my *local repository* as it only adds a *new commit*; but in a case of *merging conflict* it pauses the *merging process* because it would potentially *overwrite changes* in both my *working directory* and *staging area* -> that's why a best practice is to *stage* my *local changes* & *commit* them before running *git pull*.
        - Once *conflicts* are *resolved* those changes I have made *locally (in working directory)* that have *not* yet been *committed*, I will need to *stage* those *changes* (*git add*) & *commit* (*git commit -m "resolved conflicts with file.js"*) them first to creata a *new commit* that includes these *resolved changes* -> since I don't need to re-run *git pull* as the *merging process* was waiting for me to *resolve* the *conflicts* -> once the *merge process* has completed by *staging* & *committing* the fix I can then run *git push* or keep working from there.
        - A second option AKA alternative to *git pull* when there are conflicting changes is `git fetch` to retrieve the *latest changes* from a *remote repository* without automatically *merging* them into my *local branch* -> after that I can review the *changes* using `git diff` or other Git commands to see what has changed in the *remote branch* -> once I've reviewed the *changes* I can choose to *integrate* them into my *local branch* using either `git merge` or `git rebase`.
  - *git pull* `--rebase` flag will attempt to *reapply* my *local changes* on *top* of the *remote changes*, rather than creating a *merge commit*. This can potentially *overwrite changes* in my *working directory* and *staging area* if there are *conflicts* between my *local changes* and the *remote changes*.
    - A *git status* of the merge conflict would look like:
```
<<<<<<< HEAD
// my conflicting local changes (typically in local repository)
=======
// conflicting changes from the remote branch
>>>>>>> origin/main
```
   - Once the conflicts are *resolved* I can *stage* the changes (*git add conflicting-file.js*) & commit the merge (*git commit*).
     - For the ease of fixing *merge conflicts*, I can use a *visual merge tool* like `git mergetool` to help out; this will launch a tool like `vimdiff` or `meld` that will show the *conflicting changes* side-by-side and allow me to choose which changes to keep or I can modify the code directly.
     - *Merge conflicts* typically occur in the *local repository* ('*committed area'*) when attempting to *merge changes* from a *remote repository*; the *local directory* and *staging area* are *not* directly involved in *merge conflicts*, but they can contribute to conflicts by containing changes that conflict with changes in the other branch. 
     - In general, it's a best practice to regularly *commit change*s to the *local repository* and *pull changes* from the remote repository to *minimize* the chances of *merge conflicts*.
       - *IMPORTANT*: The logic behind when I *commit changes locally* regularly is that I'm saving a *snapshot* of my work that I can refer back to if something goes wrong && when I *pull changes* from the *remote repository*, I'm *bringing* in any *changes* that have been made since my last pull -> if there are any *conflicts* in my *local changes* I'd be notified & have the opportunity to fix (*resolve*) those breaking changes (*conflicts*) *EARLY ON* and then continue my work from there.
     - *git pull* flags:
       - Default (no flags) `git pull <remote>` is as same as `git fetch ＜remote＞` followed by `git merge origin/＜current-branch＞`.
       - `--no-commit <remote>` flag *fetches* the remote content but does *not* create a new merge commit.
       - `--rebase <remote>` instead of using *git merge* to *integrate* ('*merge*') the *remote branch* with the *local branch* (as by default *git pull*), uses *git rebase* instead.
       - `--verbose` shows the *content* being downloaded and the *merge details*.
- `git merge` merges ('*integrates'*) the specified branch into my current branch. It can lead to *merge conflicts* that would need to be solved manually; here is a YouTube video by freeCodeCamp that shows <a href="https://youtu.be/RGOj5yH7evk?t=2950">how to solve merging conflict</a> at 49:00 into the video.
- - `git diff` is used to *show differences* between two versions of a file, or between two branches or commits in a Git repository; it means I can use *commit hash* or *HEAD*s
    - `--staged` flag shows the differences between the *staging area* and the most recent commit (even if they were *modified* or *added* or *deleted*).
  - If the *merge* is *successful*, the *changes* will be added to my *commit history* as a *new commit*.
    - To clarify any possible confusion: *a merging conflict* will *not* overwrite anything from my *local repository* as I have to *resolve* those *conflicts manually* & the process will lead me to add a *new commit* by *staging* the *'resolved-conflict' changes* & making a *new commit* manually.
      - That's why if I have made *changes locally* that have *not* yet been *committed*, I must *commit* them first *before* running *git merge*.
- `git fetch` is a command that *retrieves* the *latest changes* from a *remote repository* without automatically *merging* them into my *local branch*. After running *git fetch*, I can review the changes using *git diff* command to see what has *changed* in the *remote branch*.
  - `--force` or `-f` flag forces Git to *overwrite* any *local changes* that *conflict* with the *changes* being *fetched* from the *remote repository*.
  - `--dry-run`: This flag *simulates* the *fetch process* without actually downloading any changes, allowing me to see what *changes would* be *fetched* if the *command* were run for real.
  - `--depth <depth>` flag limits the depth of the fetch to a specified number of commits back from the tip of each branch. This can be useful for fetching a repository quickly without downloading the entire history.
- `git rebase` is a command that *integrates* ('*merges*') *changes* from one branch into another by *reapplying* the *changes* on top of my *current branch*. Essentially Git will attempt to *apply* the *changes* from a *specified branch* on top of my *current branch*.
  - The same case as *git merge*, if there are any *conflicts* between the *changes*  in the 2 branches, I will need to resolve them *manually* before Git can compelte the *rebase* process.
  - *git rebase* allows me to *apply* a series of *commits* from one *branch* onto another *branch* (ex. my *current branch*). It does this by first "*rewinding*" the *changes* made in my *current branch* since it diverged from the *branch* being *rebased* onto, and then *applying* the *commits* from the *other branch* on top of the *rewritten history*. Real life example:
    - In case where I have 2 branches: `feature` and `master`, the `feature` branch has diverged from `master` at some point in the past, and I've made several *commits* on `feature` branch since then. Meanwhile, other changes have been made on `master` branch -> then, If I want to *integrate* ('*merge*') the *changes* from `master` branch into `feature` branch, I can run `git rebase master` while on the `feature branch`. This will "*rewind*" the changes made on `feature` branch since it "*diverged*" from `master` branch -> "*apply*" the *changes* from `master` branch onto the new "*base*", and then "*replay*" the *changes* made on `feature` on *top* of the *new commits*. All of this can result in a *cleaner, more linear history*, as opposed to a *history* that *shows* many *branches* and *merge commits*.
  - *git rebase* does create new *commits*, as it *applies* the *changes* from *one branch* onto *another* by *creating new commits* on *top* of the *rewritten history*
  - *git rebase* flags:
    - `-i` or `--interactive`: Starts an *interactive rebase*, allowing me to *edit, squash, or drop* individual commits as part of the *rebase process*; ex.: to *combine multiple commits* into a *single commit* before pushing to GitHub, it will rebase my commits and squash them into a *single commit*.
    - `-p` or `--preserve-merges`: Tries to *preserve merge commits* during the *rebase process*, rather than *flattening* them into a *linear sequence* of *commits*.
    - `--onto`: Specifies a new *base commit* to *rebase onto*, instead of the *default branch* being tracked.
    - `--squash` will *combine* the *changes* from *multiple commits* into a *single commit*, and give me the opportunity to *edit* the *commit message* before finalizing the *squashed commit*.
  - *Potential downsides* are *conflicts*; or *loss of context* caused by *reordering commits* or *squashing* commits; or "*history rewriting*" by the actual *changing*/*rewriting* the *commit history of a branch* -> this can cause *problems* if other developers have already *cloned* or *pulled that branch* as they will now have a different *commit history*; overall it can cause confusion for the rest of developers & they will have to manually *merge* their *changes* with the *new commit history*.
- `git clone`is used to create a copy of an existing Git repository; Git creates a new repository with all the *history* and *branches*, including all the files, directories (folders), and commits of the *original* repository.
- `git clean` must be used with the `-f` flag to remove *untracked files* from *working directory*; used for removing recently created files that exist *only* in the *working directory*. '*Untraacked file'* is a file that isn't '*tracked*' (*staged*) so it doesn't exist in the *staging area*; a simple *git restore file.js* will *not* work because Git doesn't know where to restore it from. 
  - It's an irreversible command, so a *git checkout file.js* to restore the file from the most recent commit *perhaps* will *not* work as it is a recently created file.
- `git rm` to remove file from *staging area* must be used with either `--cached` to keep the file in *working directory*, or `-f` to force removal from both *working directory* & *staging area*.
  - If I have committed the changes and this *-f* removal was done by mistake: I have the option to *git revert* to *uncommit*/undoing a commit; or *git restore --source=HEAD file.js* to restore the removed file from the last commit.
  - If *--cached* was a mistake, I can just re-do *git add*.
- `git revert` to *uncommit*/undo a commit; **WARNING** Git does this by committing a "*reverted commit*". By default Git will add "Revert" to the message of the commit-being-reverted or otherwise I have the option to use *-m* flag and write my own message.
  - Now if I switch into a commit by *git reset --soft #hash* to the commit before the mistaken commit & reverted mistaken commit; running *git log* won't show those  2 commits but they will all show up in *git reflog* in case I need, there's the full history.
  - - *git revert* **creates** a new commit that undoes the changes made by a previous commit by creating a new *reverted commit*, *without* actually removing the commit from my repository's history (unlike `git reset` which rewrites repository's history and requires to use `-f` on push). First switch to the branch I'd like to undo a commit, revert, and push. 
    - `-m` flag is used to write a message.
- `git restore` is used to restore files in my *working directory* to a previous *state* 
  - A "state" can be a *commit* or *branch* or *index(/staging area)*; in a case of using *git add* the "state" includes the changes that have been staged in the *staging area*; if then I *git commit* - the "state" would refer to the files in the commit AKA the '*snapshot*' in the *local repository*.
  - **NOTE**: a specific file (*file.js*) must be provided otherwise an error is thrown: "fatal: you must specify path(s) to restore".
  - `git restore --staged file1.js` unstages the changes that were staged into the *staging area* (huh). Humanly: cleans the *git status* from '*changes to be committed*'; basically *unadding files* - removes files from the staging area but **keeps** them in the working directory, use cases:
    - For when I want to undo add or "unadd a specific file1.js", and commit some changes, and later to "git add file1.js" and commit a new commit with the goal to have this *file1.js* in a separate commit HEAD.
      - Reminder: unlike a ***dangerous warning*** -> a flagless `git restore file1.js` will reset working directory to the state of the next area -> so if there's a staging area of `file1.js` (if I had previously run *git add file1.js*) it will reset the working directory into it but will **not** *unadd* file1.js -> meaining file1.js will remain in staging area ready to be committed -> however if there's no such a state in the staging area, then it will reset the working directory to the last committed state of file1.js -> at which point I need to use CTRL+Z command (to undo changes) on that specific file to return my edits.
  - By default if "state" or `--source` flag is not provided then Git will *restore* the file(s) from the *next area*; ex: when *file*'s changes are in the *working directory* Git will try to restore it from *staging area* but the *default*'s tricky part is that if I re-run *git restore file1.js* it will not go the next area - because, reminder: it doesn't *unstage* changes by *default* - unless & until I unstage the *file.js* (with *git restore --staged file.js*), so basically the *staging area* would be the limit in *default* behaviour unless I use the *--staged* flag; if there aren't staged changes of that file Git will try to restore it from the 'snapshot' in the *local repository*.
  - *NOTE*: it will work similarly (but not the same) as *git reset --hard* & tests:
    - **WARNING**:
    - If I had the *file.js* in the *staging area* (previously ran *git add file.js*) and I modified *file.js* for the second time, but I haven't yet added it to the *staging area* (haven't yet re-run *git add file.js*) and I run `git restore file.js` it will restore the changes from my *staging area* basically removing my last *working directory*'s changes to my *file.js* (changes which were not staged).
    - If I ran `git restore --source=HEAD~1 file.js` to restore a file to the "state" that was in the second-to-last AKA previous commit there is 2 behaviours that can happen:
      - First, if changes made in *working directory* to *file.js* were not yet staged (*git add*) AKA *staging area* is clean and *git status* shows '*changes not staged for commit*: *file1.js*' the above command would simply restore *file.js* to the *HEAD~1* commit's snapshot: *file.js* in the *working directory* will be restored && Git repository will be clean, *git status* shows '*On branch X nothing to commit, working tree clean*'.
      - Second, if changes made in *working directory* to *file.js* were staged by *git add file.js* && *git status* shows '*changes to be committed*: *file.js*'; the command above will again restore *file.js* to the *HEAD~1*'s commit snapshot but *git status* shows *file.js* in both fields:'*changes to be commited*'&'*changes not staged for commit*' - because the *working directory* differentiates from *staging area*. *EXTRA*: *git restore --staged file.js* won't modify *working directory* but will clean the *git status* by *unstaging changes* if such behaviour is wanted (*note*: *git reset --mixed* would do the same job here but it's an older command and I can't specify a file(s)); otherwise run *git restore file.js* if the *--source* command was an accident & I want to restore my *file.js* from my *staging area* into my *working directory*.
    - Considering these things, if I were to switch *HEAD*s only to *restore* files (by *git reset --soft #hash*; or *git branch*; or *git switch*, etc.), if in *staging area* I have the files I don't want to commit -> I would first need to unstage them (*git restore --staged*) & then *git restore file.js*, otherwise the *staging area* would be there as the "wall"/limit and would not be able to restore files from local repository's/commit's *snapshot* unless I free the path to the commit-snapshot by unstaging unwanted changes.
    - It works only for tracked files; for removed files use *git clean -f* or *git rm -f*.
    - *git restore* requires Git version 2.23 or higher.
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
  - `-vv`to see the *upstream branch* of my current branch; also shows the UI of the (*last*) commit.
    - For ex.: it will show up as *master  808b598 [origin/master] Initial commit*; and if there is no *upstream set* then it won't show the name between the square brackets.
- `git switch` is used to *switch* or *check out* at a specific branch. Introduced in same version as *git restore* v2.23.
  - If I have *uncommitted changes* in my *working directory*, I won't be able to use *git switch* to switch branches; in that case I would need to *commit* my changes or to *stash* them before switching to a different branch.
  - *git switch* needs to be provided with the correct branch name as it won't create a new one if it doesn't exist.
  - If there are conflicts between my current branch and the branch I'm trying to switch onto, an error will be thrown and I'd have to resolve that first.
    - Switch error example when there are working directory (local) changes that were not yet commited: "*Your local changes to the following files would be overwritten by checkout: *; ... ;* Please commit your changes or stash them before you switch branches.*".
  - *Only* works with branches and can't use it for switching commits like *git checkout* but a better alternative is the *git restore --source* flag.
    - `-c` flag will create a new branch with the provided name and immediately switch to it.
 - `git checkout` *switches* to the specified branch and *updates* the *working directory* to *match* the *contents* of *that* branch. It is an older command in Git that can be used to *switch* between branches or to *check out* specific commits; in Git version 2.23 its functionality is split into 2 commands *git restore* && *git switch*.
   - `-b` flag creates a new branch with the specified name, pointing to the current commit, and switches to the new branch; while capital "b" a `-B` flag would overwrite a branch even if it exists and already has contents in it; to create a branch without switching to it automatically use *git branch new-feature*
     - *-b* flag is equal to running *git branch* followed by *git switch*.
     - New experience I also tried running `git checkout HEAD~31` (in continuation of the story above about "git reset HEAD~31" (CTRL+F/CTRL+G)) but that just gave me another features I've never encountered:
       - CLI response message: "*You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.
If you want to create a new branch to retain commits you create, you may
do so (now or later) by using `-c` with the switch command. Example:* [by me: no example was given]
*Or undo this operation with:
  `git switch -`
Turn off this advice by setting config variable advice.detachedHead to false
talled types*".
    - I had also ran `git checkout HEAD~31 -- .` where:
      - `checkout`: is used to switch between different branches or commits.
      - `HEAD~31`: refers to the commit that is 31 commits behind the current commit. HEAD represents the current commit, and ~31 means to go back 31 commits from the current commit.
      - `--`: is a separator to separate the commit reference from the file path.
      - `.`: indicates the current directory. This tells Git to update the entire working directory with the contents of the specified commit.
      - Description: this command will update my working directory and staging area to match the specified commit, effectively discarding the local changes I've made after the 31 commits.
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
- `git remote` command is used to specify what remote endpoints the syncing commands will operate on. *git remote* is used to *view, add, or remove* remotes associated with the *local repository*.
  - When I clone a repository from a remote server, Git automatically creates a remote called "*origin*", which *points* to the *original (remote) repository*
  - For ex.: to *add* a new *remote repository* named "upstream" with the URL https://github.com/upstream/repo.git, I can use the following command:`git remote add upstream https://github.com/upstream/repo.git` -> then, I can *fetch* the latest changes from the "*upstream*" *repository* using the following command: `git fetch upstream` this will *download* the *changes* from the "*upstream*" *repository* into my *local repository*.
  - Other common uses of the *git remote* command:
    - `git remote`: List all the remote repositories associated with the local repository.
    - `git remote -v`: List all the remote repositories associated with the local repository along with their URLs.
    - `git remote add <name> <url>`: Add a new remote repository to the local repository with the specified name and URL.
    - `git remote remove <name>`: Remove a remote repository from the local repository with the specified name.
    - `git remote rename <old-name> <new-name>`: Rename a remote repository from the old name to the new name.
- `git remote add origin` *url-example.git* is a Git command used to add a remote repository to my local Git repository.
  - The term "*origin"* in this command is the name of the *remote repository* that I'm adding; "origin" is a common naming convenience but can be anything else. The only rules are no special characters.
- `git rev-list --count origin/main..HEAD`: This command will return the number of commits that are in my local branch but not in the remote. If the count is greater than 0, my local branch is ahead of the remote branch. If the count is 0, my local branch is in sync.

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



---
#### Extra experiences:
1. New experience (***keep reading to the end as there was issues***): on my Server Cars Club I wanted to push changes into a new branch "feature-2", but instead I pushed them at "feature-1" then ran `git checkout -b feature-2` to create that one and `git push origin feature-2` (because commits were 'merged' into my new branch) and then I merged this PR for "feature-2" but **not** "feature-1", but, *logically indeed*, the "feature-1" showed the same amount of "*commits behind*" as the merged PR "feature-2" whereas my initial goal was to leave "feature-1" as-is as if a backup to go back in time, but it was already too late, so as a fix I had to run commands: `git switch feature-1` ~then `git reset HEAD~31`~ (31 was the amount of new commits ahead - prior to the "feature-2"'s PR merge (which, *again*, had exactly same commits as "feature-1")) then `git push origin feature-1 -f`.
    - And an issue: I wasn't able to switch back to "feature-2" because the "feature-1" had (31) '*local changes for commit*', but a *wrong attempt* `git pull origin feature-1` **didn't work** because it says '*Already up to date*' and that's *not logically true*: my local directory files doesn't match the ''origin feature-1'' but it's *technically true* as my local repository matches my "origin feature-1" because I had run the default `git reset` flagless meaning `--mixed` flag was used so my local changes didn't go back 31 commits behind and `git switch` is now asking me to re-`commit` or `git stash` those "*local changes*" before switching as to not overwrite them. The solution was running `git reset--hard` flag but I needed a way to go back/reverse those 31 commits first, as to **not** go back 62 ~commits~ HEADs (thinking back from now: I'd have went 61 commits aback because there was one HEAD 'switching' (caused from the previous `reset` command) in those 62 HEADs.):
      - (Thinking back again, `get reset HEAD~32` should have done it without the `git reflog` solution below.):
      - Yet another silly issue where ChatGPT tricked me: tried to go forward '*31 commits ahead*' by a wrong command `git reset HEAD~31^` (and `git reset HEAD^31` alike), well that's *logically **impossible*** -> once HEAD is RESET there's no HEADs above it; so I instead had ran `git reflog` and copied the hash where initial 'switch from feature-2 to feature-1' happened & ran `git reset --hard abc123` and then ran the **correct** command:
      - **Correct**: `git reset --hard HEAD~31`; now that my "feature-1" PR was already merged (*from the story above*), running `git push origin feature-1 -f` was not necessary because the message is 'Everything up-to-date' but otherwise in the future for such a case I **must** run that command afterwards, and once PR is merged in GitHub: running test command `git pull origin feature-1` shows 'Already up to date' & I can switch freely to `git switch feature-2`.
- Additionally, (good info👍) while I am inside the branch "**feature-2**" running a command like `git push origin feature-1` the command will still safely **push** the changes inside of the "**feature-1**" branch & its own local repository (AKA "_`git commit`-ed_") files without any problems nor negative interaction with the current "***feature-2***" branch's local repository.
2.
- Potential issues with Forks when I tried to Sync *main* branch with the original repo's *main* branch, in a case where I didn't use the "*magical*" **Sync** button (the magic is revealed <a href="https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork#syncing-a-fork-branch-from-the-command-line">here</a>), *which was previously called <a href="https://i.stack.imgur.com/NKEzt.png">Fetch and merge</a> button*, is that when I actually merged those *X commit(s) behind* from the GitHub website at my fork https://github.com/Aleksandar15/<fork-name\> -> my *main* branch became *X commit(s) ahead* - which was the most confusing part because all the files match as the <original-repo\>, so **0 files changes**, but turns out, *under the hood* what happens is that *merge* came from the <original-repo\> so my <fork-repo\> got one (*could been more*) extra merge and changed its *HEAD* to be 1 HEAD(s) above the *upstream/main*.
  - *NOTE*: For the below solution to work I have to set a *upstream* (or any name) "*remote*", which is mentioned in the link above at the "*magic is revealed*" <a href="https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/configuring-a-remote-repository-for-a-fork">part</a>:
    - `git remote -v` to view all of my current *remote*s; `-v` flag for the links.
    - `git remote add upstream <link>` where <link\> is `https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git`.
      - *Note*: the `.git` file extension at the end is not necessary, but it's a good convention to follow.
    - `git remote remove <remote-name>` to remove a remote link.
  - To fix this issue, and return this *X commit(s) ahead* back to *Synced*, there's a great answer I found <a href="https://github.com/orgs/community/discussions/22440#discussioncomment-3236721">here</a>:
  ```
  git checkout main
  git fetch upstream
  git reset --hard upstream/main
  git push --force
    ```
    - Which essentially switches HEAD: *HEAD is now at #hash-here Commit message here (#PR-number)*
      - And now running `git status` shows:
        ```
        On branch main
        Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.   
        (use "git pull" to update your local branch)
        nothing to commit, working tree clean
         ```
          - *NOTE*: **don't** run `git pull` because that's not the *goal* here.
       - Whereas running the normal `git push` **without** a `--force` flag returns an error:
         ```
         To https://github.com/Aleksandar15/react.dev.git
         ! [rejected]          main -> main (non-fast-forward)
         error: failed to push some refs to 'https://github.com/Aleksandar15/react.dev.git'
         hint: Updates were rejected because the tip of your current branch is behind
         hint: its remote counterpart. Integrate the remote changes (e.g.
         hint: 'git pull ...') before pushing again.
         hint: See the 'Note about fast-forwards' in 'git push --help' for details.
         ```
         - So, running the apropriate command (*for this case scenario*) `git push --force` returns a success along the lines of:
           ```
           Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
           To https://github.com/Aleksandar15/react.dev.git
            + d412a7fb...cdc99178 main -> main (forced update)
           ```
             - *Note*: The shorthand flag `-f` works as well.
              - And my Fork at GitHub website says: "*This branch is up to date with reactjs/react.dev:main.*".
  - An ***important notice*** here is that while the opened PR & its branch would stay as "*X commit(s) behind*" that is totally fine in a sense that it's not the main branch, so the only issue would be if that branch has clashing changes AKA conflicting merge files in those "*X commit(s) behind*", then it would be an issue, otherwise there's not much you can do about your *branched off* branch without modifying your local changes - back to original (by *git pull*) which would ruin the purpose of this "*feature branch*"'s goal to improve something; at that point I'd have to re-write all the changes I previously *pushed*  which is not the *goal*.
    - And as expected, even my testings are failing when I run `git fetch upstream ; git checkout feature-branch ; git merge upstream/feature-branch` I get "*merge: upstream/improve-challenge-explanations - not something we can merge*" (*again, these are commands of <a href="https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork#syncing-a-fork-branch-from-the-command-line">behind the magic of Sync</a>*). 
      - *Note*: that's a same error I got when I tried to run the `git merge upstream/main` but the issue was I hadn't set up an "*upstream*" remote link, this time I do have, but fast-forwarding is not possible to a branch* as answered in <a href="https://community.atlassian.com/t5/Bitbucket-questions/What-happens-if-I-merge-a-PR-that-is-out-of-sync-from-master/qaq-p/1291041#M51499">this</a> article.
        - *As to why *fast-rowarding* is not possible on a branched off branch in which I *have* local file changes (*not matching the Upstream's repo's "main" branch* even though they were *staged* and already *pushed* onto my "*improve-challenge-explanatons*" branch, but **not yet merged** ((but at that point I should at least get merging conflicts error, instead of the error above, so the following second reason seems more to be the actual issue)), and on top of it, *commits* which don't exist in the *upstream*'s **Git**'s *main* branch, a combination of both doesn't allow *Fast-forwarding*.
          - It seem very likely, like a **scenario** where I'd need to run `git pull` (a combo of `get fetch`;`git merge`) which has its limitations: I don't want my local changes to be overriden inside my "*improve-challenge-explanations*" if I still plan to update it further, or the PR has comments and other suggestions alike, at that point I **must not** `git pull`, but rather wait, and let this branch be as-is; and if I wanted to start working on a different feature, in a different file, I'd `git checkout main` and `git pull origin main` and then create a new branch from there `git checkout -b new-branch-name`, branch-off and keep working on another feature.
          - Which, funnily enough the **scenario** I was predicting/talking about (_or at least somewhat similar_) has happened to me and is written in _Experience # '3.'_ below.
            
3. All of it was done for my Cars Club Frontend's repository (IDs of the PRs are: ever since [pull/50](https://github.com/Aleksandar15/Cars-Club-frontend/pull/50) - until [pull/59](https://github.com/Aleksandar15/Cars-Club-frontend/pull/59)).
- **UPDATE3:** (*As written in the '2.' experience above towards the very ending*) Again, all of this could have been avoided if I had created a branch by following the instructions below instead of branching off of "_feature_18_" branch and gathering its **unwanted commits history** for the back-then-new "_hotfix_2_" branch!
  - (As I had written in my own [comment](https://github.com/Aleksandar15/Cars-Club-frontend/pull/56#issuecomment-1998917385) in [/pull/56](https://github.com/Aleksandar15/Cars-Club-frontend/pull/56) at Cars-Club-frontend.)
  - Create a branch
    1. `git checkout main` from any folder in my local `project-folder-name` repository.
    1. `git pull origin main` to ensure I have the **latest `main`** code.
    1. `git checkout -b the-name-of-my-branch` (replacing `the-name-of-my-branch` with a suitable name) to create a branch.
       - (*Source for these steps are the react.dev doc's at their GitHub README.*)
- **UPDATE2:** Looking back at it, once I was inside my _then-new_ "**hotfix2**" branch I could have "uncommit" the last 2 commits by running the command `git reset --soft HEAD~2`: and only to `git add` then `git commit` the 3rd Hotfix commit that I will `git push origin hotfix2`; and then switch back to my "**feature_18**" and continue my feature-work (with those _2 commit **intact**_).
  - (*Update2 is above ORIGINAL for easy readability.*)

- **ORIGINAL:** New experiences **explanations** -> the issue was **started** by the mere fact that I did **not** expect switching (by either _`git checkout -b` or `git switch -c`_, _doesn't matter_ which command still _same outcome_**)** from `feature_18` branch (_a branch that has a few `git commit`s but **0** `git push`es_) into a new fresh `hotfix_2` branch would still keep up the _old commits_ from my previous branch `feature_18` -> and hence, while on my then-new `hotfix_2` branch I 1. did my file-changes to fix the bug; 2. ran `git commit -m`; 3. then, running `git push` command turned into a messy `push` of both the _**hotfix bug fixes**_ as well as the _features from my `feature_18` branch_ which were **not** meant for this `hotfix_2` branch!

- Then as a solution I had created new branch **hotfix_2.0** and I did all of its mistakes that ***hotfix_2***'s branch & _same-named_ PR has had:
  - 2 unwanted commits from ***feature_18*** branch; and 1 correct commit from _hotfix_2_ branch.
  - I had tried using `git rebase -i` command that ChatGPT suggested to me, but after many merge conflicts - then, all due to my lack of knowledge with this command I can **not** tell exactly whether 1. the command itself can't do what I wanted it to do; or 2. whether I lack the knowledge (ex.: which _flags_ to use with `git rebase`, etc.) to achieve what I wanted => that is: to remove the 2 unwanted commits from ***feature_18*** branch & to keep the last and **only** 1 correct commit from _hotfix_2_ branch that I do wanted to `git push` & merge the PR quickly into my `main` branch!
  - For visualization this is how my commits looked like:
    1. commit1 (unwanted)
    2. commit2 (unwanted)
    3. commit3 (hotfix changes I wanted to ***push*** immediately; all the while I was working on features in my Cars Club app)
  - I came up with my own solution that is to reset the newly created branch ***hotfix2.0*** into the `origin/main`; and so I did ran these commands: `git reset --hard origin/main` followed by `git push origin hotfix_2.0 -f`.

- After running `git reset --hard origin/main` and then `git push origin hotfix_2.0 -f` the GitHub.com website has automatically "_merged and closed_" this very same _hotfix_2.0_ PR (which has had ***2 unwanted and 1 correct commit***); GitHub also automatically deleted the older commits (all 3 of them _dissapeared_) - expectedly so! ✔ However on my local machine switching to my older branches (say ***feature_18*** or ***hotfix2***) I can see these commits in there - which is all good. 👍

- As a reminder my main issue was with "_hotfix_2_" (minus the _.0_) branch - there I had the commits from the other branch (_feature_18_) so the _hotfix_2_ was about to commit unwanted-future-based _**feature**_ commits (committed in _feature_18_ but not yet _`git push`ed_), and as such, the situation, technically was impossible to remove certain commits only to keep the latest commit inside my _hotfix_2_ branch, so I had created this new branch _hotfix_2.0_ instead, only to, as a last resort, try my own logic of modifying both the _local files/working directory_ AND my commits to match my latest version of _main branch_ (`git reset --hard origin/main`) and then later to re-add **_hotfix bug fixes/changes_** and then run `git push origin hotfix_2.0` once I'm done with my changes, so that that way I'd avoid pushing the commits from _feature_18 branch_.

- Additionally, I've got answers by ChatGPT suggesting me to use `git rebase -i` command, but it's either my lack of knowledge about this command or the incapability of its functionality to do what I wanted it to do. For now I can't be 100% sure because my lack of deep knowledge about `git rebase` command.

- I've also tried running `git pull origin main` (that's `git fetch` + `git merge`) but of course that didn't do what I wanted to do because I had no such issue that'd be fixed by pulling the commits from _remote branches_ - I rather wanted to modify both my _local files/working directory_ AND my commits into my new "_hotfix_2.0_ branch" (_to match the "**main** branch_") in order to only commit the **_hotfix bug fixes_**, and so the above worked. :)

- Of course switching back to my _feature_18_ branch the old commits are kept, also my local files re-update to match my old files-updates ("old files-updates" = pre-_hotfix_2.0_ files-updates) so it's all fine on my _working directory/working tree_! 👍 

- As well of course I can't click the button _"reopen and comment"_ the _hotfix_2.0_ PR because, logically, GitHub says: "there are no new commits on the hotfix_2.0 branch", because, again, technically I've only "removed" the old mistaken commits (by running _`git reset --hard origin/main`_) and re-push to this very same _hotfix_2.0_ branch by using the `-f` flag short of `--force` flag.
  - But, once I do some edits on my _working directory_, then I run commands of 1. _git add ._ + 2. _git commit -m""_ + 3. _git push_ then GitHub.com will auto re-open the _hotfix_2.0_ PR.
    - I'm not sure what would have happened if there are multiple closed PRs of the same "_hotfix-branch to main branch" relationship_, but as of the moment I did my test there was only 1 closed PR "_hotfix_2.0_" so it auto-opened by GitHub.com.
      - Even if I have multiple closed PR's of the same "_hotfix-branch to main branch" relationship_, logically I can **only** open 1 of these closed PRs while the rest of the closed PR's "_reopen PR_" buttons will remain grayed out/unclickable and GitHub (_expectedly_) says: **_"There is already an open pull request from hotfix_2.0 to main."_**.


