# Git Cheat Sheet

This cheat sheet features the most important and commonly used Git commands for easy reference.

## Setup

Configure name and email for the project:

    git config user.name "Juan Perez Nava"
    git config user.email "juan23@gmail.com"

You can set these parameters as global (for all your git projects) by adding the global flag:

    git config --global user.name "Juan Perez Nava"

Configure default editor for commit messages:

    git config --global core.editor "code --wait"

## Adding and Committing

Add files to *Staging Area*:

    git add file1 file2...
    git add .

Commit all the staged files with a message:

    git commit -m "my message"

Automatically stage files that have been modified or deleted and commit in the same line:

    git commit -a -m "my message"

Add files or changes to the previous commit to prevent adding a new commit. A use case would be when you're addressing some PR comments that include improvements, refactors, UI or style changes:

    git commit --amend

This also helps update the commit message from the previous commit. But if you don't want to update the commit message, then use:

    git commit --amend --no-edit

> Only amend commits that are still local and have not been pushed somewhere.

#### Logging

Show commit logs:

    git log

Show summary of commits:

    git log --oneline

#### Traveling in Time

Travel and check out old commits of a specific branch, for example, on master (this detaches HEAD):

    git checkout hashOfASpecificCommitFromMaster

Re-attaching the detached HEAD (switch to the name of the branch):

    git switch master
    git switch -

Travel and check out old commits by referencing previous commits relative to HEAD:

    git checkout HEAD~1

> HEAD\~1 refers to the commit before HEAD. HEAD\~2 refers to 2 commits before HEAD.

Travel and check out remote branches:

    git checkout remoteName/branchName

#### Undoing Changes

Unstage a file:

    git reset HEAD fileToBeUnstaged 
    git restore --staged fileToBeUnstaged

Discard changes of a file in the working directory:

    git checkout HEAD fileToBeDiscarded
    git checkout -- fileToBeDiscarded
    git restore fileToBeDiscarded

*Restore* command uses HEAD as the default source but we can change that using *--source* option:

    git restore --source HEAD~2 fileToBeDiscarded

Undo one or more commits:

    git reset myCommitHash

> This will reset the repo back to the given commit and keep the changes of the undone commits in the working directory. You don't lose the work of the undone commits, you only loose the commits.

Undo the commits AND remove the changes related to those commits:

    git reset --hard HEAD~1

> This will delete the last commit and associated changes.

Create a brand new commit after undoing changes of one or more commits (good to keep commit history):

    git revert myCommitHash

> *Reset* and *revert* commands reverse changes, but there's a difference when it comes to collaboration. If you want to reverse some commits that other people already have on their machines, you should use *revert*. If you want to reverse commits that you haven't shared with others, use reset and no one will ever know.

#### Viewing Changes

List all the changes in our working directory that are not staged:

    git diff

List all the changes in our working directory since the last commit (staged and unstaged):

    git diff HEAD

List all the changes in our working directory that are staged:

    git diff --staged
    git diff --cached

List the changes of a specific file

    git diff filename
    git diff HEAD filename
    git diff --staged filename1 filename2...

List the changes between branch1 and branch2 (it also applies for commits by using the commit hashes):

    git diff branch1..branch2

## Branching and Merging

View all branches:

    git branch

View remote branches:

    git branch -r

Create a new branch based upon the current HEAD:

    git branch "my new branch"

Switch to a branch:

    git switch "my other branch"

Create a new branch and switch to it all in one go:

    git switch -c "my new branch"
    git switch --create "my new branch"

Switch to a branch or restore working tree files:

    git checkout "my other branch"

Another way to create a new branch and switch to it all in one go:

    git checkout -b "my new branch"

Rename a branch (first switch to the branch you want to rename):

    git branch -m "my new branch name"

Delete a branch:

    git branch -d "my branch"
    git branch --delete "my branch"

Delete a branch irrespective of its merged status (i.e. when a branch hasn't been merged yet):

    git branch -d -f "my branch"
    git branch --delete --force "my branch"
    git branch -D "my branch"

Merge and bring changes from a separate branch (featureBranch) into the current branch (i.e. master) with a fast-forward strategy:

    git merge featureBranch

In this example, the fast-forward merge will add the extra commits contained in featureBranch to master.

## Stashing

Save changes (staged or unstaged) that you are not ready to commit:

    git stash
    git stash save

Remove the most recently stashed changes in your stash and re-apply them in your working directory:

    git stash pop

Apply whatever is stashed away, without removing it from stash (this can be helpul if you want to apply stashed changes to multiple branches):

    git stash apply

You can add multiple stashes onto the stack of stashed. They will be stashed in the order you added them:

    git stash (1st stash)
    git stash (2nd stash)
    git stash (3rd stash)

List stashes:

    git stash list

Apply specific stash:

    git stash apply stash@{2}

Delete a specific stash:

    git stash drop stash@{2}

Clear out all stashes:

    git stash clear

## Rebasing

Switch to a feature branch and rebase onto master:

    git switch feature
    git rebase master

> Rebasing a feature branch onto a master branch moves the entire feature branch so that it BEGINS at the tip of the master branch. 
> Instead of using a merge commit, rebasing rewrites history by creating new commits for each of the original feature branch commits.
## Sharing and Updating Projects

#### Remotes

View existing remotes for your repository:

    git remote
    git remote -v

A remote is a URL and a name/label. To add a new remote, we need to provide both:

    git remote add remoteName remoteUrl

Rename a remote:

    git remote rename oldRemoteName newRemoteName

Remove a remote:

    git remote remoteName

> Origin is a conventional Git remote name, but it's not special. It's just a name for a URL.

#### Pushing

To push some work:

    git push remoteName branchName
    git push remoteName localBranchName:remoteBranchName

To avoid setting remoteName and branchName every time we push, set a link between the remote and local branch:

    git push -u remoteName branchName
    git push --set-upstream remoteName branchName

> When setting this connection, we can push using the shortcut `git push`

To push a commit that used *amend* flag:

    git push --force-with-lease

#### Fetching and Pulling

Create a new local branch from the remote branch of the same name:

    git switch remoteBranchName
    git checkout --track remoteName/branchName

Fetch branches and history from a specific remote repository:

    git fetch
    git fetch remoteName

Fetch a specific branch from a remote:

    git fetch remoteName branchName

> Fetching allows to download changes from a remote repository to your local repository, but your working directory stays the same. 
> Think of it as *"please go and get the latest from Github, but don't screw up my working directory"*

Retrieve changes from a remote repository:

    git pull remoteName branchName

Retrieve changes shortcut:

    git pull

> Using this, it's assumed remote will default to origin and branch will default to whatever connection is configured for your current branch.