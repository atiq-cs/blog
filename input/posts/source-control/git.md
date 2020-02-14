Title: Useful Git Commands
Published: 07/25/2016
Tags:
  - git
  - source Control
  - version Control
---
[first few sections are complete, later sections yet to be updated]
# Introduction
A push to the remote repo consists of following two steps,

 1. To add changes to the staging area
 2. push the staged changes to remote repo

## How to add files or changes to staged area (to commit later)
[update not complete yet]

Note: commit stores file locally. Commit is not sent to remote repository unless a `push` command is applied.

Here, we give some examples of adding files/directories to staged area. [Staged area][1] means the stage before commit. Anything that are modified (added/changed/deleted) in the stage area will be performed whenever a `commit` command is applied.

To add files/directories to staged area, at first, we modify or change files,

Then we can add all those changes to staged area. This will include any new file as well,

    $ git add --all

But if we want to add only modifications on previously added files on repo we do this,

    $ git add -u

This means new files won't be added to the staged area.

Or, we can add selectively a few (after all modifications are done on those files), the files we want to commit we add them by applying a series of `git add` commands,

    git add file1
    git add file2
    ………….
    ………….
    ……………
    git add filen


Or, we can choose to add directory or files inside another directory (is this recursive, has to check),

    $ git add code_python/*

Remove a dir entirely,

    $ git rm -r _old_code/res/

How to remove a file,

    $ git rm file1.txt
    $ git commit -m "remove file1.txt"

How to rename or move a file in the repo,

    $ git mv original_name new_name

Pleae don't be confused by "name" here. It supports renaming using full path as wel. It also support renaming a directories.

Removing a file from staged area, [ref][2]

    $ git reset /location/to/file

Note, this will not touch the file; only removes it from git staged area.

Show the changes which have been staged (git add`ed but not committed)?, [ref][3]

    $ git diff --cached

The changes we made are called deltas.

# How to commit staged changes (git commit)
Commit to actually push the changes to local repo (will be in remote as soon as pushed). If you are doing this for the first time in the git client you installed, you need to configure committer email and user-name,

    git config --global user.email "John.Doe@Domain.Com"
    git config --global user.name "John Doe"

To check if those are set,

    git config --global --get user.email
    git config --global --get user.name


`git commit` is different than `svn commit` as `svn commit` pushes to remote repo immediately whereas git commit does not. git usually have the entire copy just like remote repo, svn does not have entire copy.

Note: commit stores file locally. Commit is not sent to remote repository unless a `push` command is applied (we will discuss push command on next section).

To add the commit message use -m switch in arguments as showed below, this supports multiline,

    git commit -m "first commit message"

Here is an example of multi-line commit message, surrounding the entire commit message with double quotes works,

    $ git commit -m "Implemented secure functions - line1
    line2 - example line that means code won't work on Linux, we'll have to add separate
    line3 - branch for Linux
    line4 - Update on logic based on tests"

In powershell, single quote works for multi-line commit message,

    $ git commit -m "line1
    line2
    line3
    line4"


# Pushing to remote(sending deltas to remote repo server)

Then use "git push origin branch_name"

Here is an example of Pushing to remote master branch,

    $ git push origin master

To show info about git repo's remote origin,

    $ git remote show origin

# git undo, edit commit etc
To Rollback to a specific previous commit we do,

    git reset --hard 
    git push -f

To roll back to previous to last commit we do,

    git reset --hard ~HEAD
    git push -f

We can edit commit message of the last commit anytime before push,

    $ git commit --amend

If we want to use commit message from a file file,

    $ git commit -F filepath.ext

Commit message from file (amend previous one),

    $ git commit -eF filepath.ext

After pushing you still can edit the commit message,

    $ git commit -eF filepath.ext

Then do a force push,

    $ git push -f origin master
However, if someone has made another commit in the meantime those commits will be gone. So be aware of that!

To edit previous commit messages that are pushed,

    $ git rebase --interactive 88af37e

Then Ctrl X then,

    $ git rebase --continue
    $ git push -f origin master

Again, a force push destroys commit made by others.

To correct commit author name upto specific commit (probably? or is it only for single commit?),

    git reset author
    git add -u
    git rebase -i -p 7508a2627bee2a28535e7ae306c51788fd287f9f

mark all of them as ‘e’ and keep applying following commands. In this example, the author's email address is `atiqcs@fftsys.com`,

    git commit --amend --author "Atiq Rahman "
    git rebase --continue

Removing a single commit from history,
There might be conflicts during the operation, some commit order gets changed as well (not order by date anymore), Be aware this is dangerous and might now work,

    $ git rebase --onto dadfe3e^ dadfe3e HEAD

Then, after conflict resolution, ref

    $ git rebase --continue

# Stashing Commands
Be careful while applying these commands.

Stashing all local changes
Hard - stash all local changes and pull updated code [\[ref\]](http://stackoverflow.com/questions/4157189/git-pull-while-ignoring-local-changes)

This is better than ‘git stash’ It deletes unstaged changes,

    $ git checkout -- .

Clean up all non version controlled files,

    $ git clean -df

Find last commit id,
To get long commit id,

    $ git rev-parse HEAD

To get short commit id,

    $ git rev-parse --short HEAD

### Checking specific revision of a file or getting specific revision

Checkout specific revision of a file,

    $ git checkout abcde file/to/restore

git checkout specific commit

    git checkout -b old_state f5bf0d670686143a67177e9d81474687535a761e

Or do following,

    git reset --hard origin/master
    git fetch
    git checkout -b old_state f5bf0d6

git checkout can be useful in restoring a mistakenly delete file as well or to stash changes on a specific file,

    $ git checkout abcde file/to/restore

This restore the file of the current revision. \[[ref][4]\]

View a commit,

    $ git show commit_id

Show specific revision of a file, ref

    $ git show commit_id:file_path
    $ git show 61c2ac6:src/bootstrapper.cc

# Conflict Resolution
Most common approach in resolving conflict is to check which files are having conflict,

    $ git status

And what changes are coming from what side that is causing the conflict,

    $ git diff

Addionally, a file path can be specified to see what changes are coming from the remote and local,

    $ git diff /path/to/file

Then, we use a merge tool or manually modify the content file and finalize the conflict resolution. Afterwards, when are done doing that with that file we do,

    $ git add file_path

When conflict  resolved for all files finally we do,m

    $ git commit -m "conflict resolved commit and so on.."

However, in some case conflict resolution can be easier. For example, you might want to discard local changes and just accept remote forcefully. In such a case we can follow this:

A basic overview how to merge is on this [gitguys' article][5]

    git fetch

Accept their change for single file, ref

    git checkout --theirs src/register-configuration.cc

# Origin tricks and Branches
Change repository URL,

    git remote set-url origin https://github.com/sacsdu/testing.git

switch to a new branch,

    git checkout -b dev_branch

show current branch and other branches,

    git branch

Delete a branch on your local filesystem,

    git branch -d testing_branch

The git push syntax is: git push [remote-repository-name] [branch-or-commit-name],

    git push origin dev_branch

Change remote origin,

    git remote set-url origin URL


# Detached Head
If we want our git repo to have no current branch then we can do this,

    git checkout COMMIT-SHA-ID

# Git configuration Files
When we perform `git config` it applies only to the current project. The configuration file that gets update is,

    ./.git/config

When we apply `git config --global` it updates following file,

    ~/.gitconfig

If none of these exist then the systems git config file is applied. According to priority these files are presented below,

    ./.git/config
    ~/.gitconfig
    /etc/gitconfig (guessing I don't recall the exact location)

# References
 1. [Getting Started - Git Basics][6]


  [1]: https://git-scm.com/book/en/v2/Getting-Started-Git-Basics
  [2]: http://data.agaric.com/undo-git-add-remove-files-staged-git-commit
  [3]: https://stackoverflow.com/questions/1587846/how-do-i-show-the-changes-which-have-been-staged
  [4]: http://stackoverflow.com/questions/953481/find-and-restore-a-deleted-file-in-a-git-repository
  [5]: http://www.gitguys.com/topics/merging-with-a-conflict-conflicts-and-resolutions/
  [6]: https://git-scm.com/book/en/v2/Getting-Started-Git-Basics
