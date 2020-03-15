#Initialize git and create a repository

git init

# see the status

git status

# look at all the commits

git log 

# to see the last 2 commits
git log -2

# with params for git log
git log --decorate --oneline --all --graph

#create alias

git config --global alias.branches "branch -a"
git config --gloabl alias.history "log --decorate --oneline --all --graph"

#get help

git help command


# add new / modified files to staging area
git add .

#commit files to repository
git commit -m "label"
git commit -am "label"  # will commit modified files to repository from working directory. shortcut for using git add. and git commit.


#delete a file in git
git rm filename

# exclude files add .gitingore file

# to remove files that are part of the repository.

git rm -r --cached .



#******************************************##Branches************************************************************************************
#list all branches
git branch -a

git branch -r # list remote branches

# create a new branch
git branch branchname

# to move to the newly created branch
git checkout branchname

# rename a branch
git branch -m branchname newbranchname

#delete a branch
git branch -d branchname # this will delete the branch, if the changes are merged with someother branch

# to forcefully delete a branch
git branch -D branchname

# create branch from a commitid
git branch branchname commitid

#******************************************************************************************************************************


#*************************************## Merge*****************************************************************************************
git merge branchtomergefrom

#3 way merge between the following 3

#common ancestor (before brancing on the master - c2)
#snapshot to merge into (master)
#snapshot to merge in (the commit in the newfeature branch that you are going to merge into master)

#this creates a merge commit - when you merge changes from the newfeature branch to the master.

Fast forward:
#	this means no changes in the branch that you created another branch from. So basically nothing has changed in the original branch.

# in this case, Git will simply move the pointer forward - as no merge is necessary.



#******************************************************************************************************************************

#**************************************## Diff****************************************************************************************
# 3 different types of files:
# working directory
# staging area
# repository

# working directory vs staging area
git diff

# working directory vs last commit (repository)

git diff HEAD

# staging area vs last commit (repository)

git diff --staged 
git diff --cached

#diff between 2 commits
git diff commitid1 commitid2

#the last commit is HEAD, the previous is HEAD^, the prev is HEAD^^ and so on
# we can also use HEAD, HEAD^, HEAD^2
# we can also use HEAD, HEAD~, HEAD~2

# to diff a specific file:
git diff HEAD^ filepath
#******************************************************************************************************************************


#***********************************************## checkout*******************************************************************************

git checkout -b new branch name

git checkout branchname


#******************************************************************************************************************************

#*********************************************UNDO COMMTIS & CHECKOUTS******************************************************************************

# reset - applicable only to git local - because once you push changes to remote, you can't remove commits and push it back to remote.
# with git reset  - you can remove the last n commits. we can't remove a specific commit.
# 3 modes soft, mixed, hard = mixed is the default
git reset --mixed # unstaged changes after the reset from the staging area. the changes are still available in the working directory.

git reset --soft HEAD^ # changes will remain the staging area and working directory. the HEAD will be reset to the HEAD^ in the local repository.
# git will copy the old head to .git/ORIG_HEAD. Once we have made further changes you can do git commit -a -C ORIG_HEAD or do a git commit -am "label"

git reset --hard commitid# changes are lost pretty much every where. this is mostly used if you want to undo your changes and keep working on a topic branch

#example: you want to rollback 3 commits from the master branch - but continue to work on those changes in a diff branch.

git checkout -b topic/wip
git reset --hard HEAD^3
git switch topic/wip



git restore

git revert commitid # lets you revert the changes from a specific commit- revert an n'th commit

git cherry-pick   commitid # lets you apply a specific commit on a branch to another branch. merge & rebase will apply all the commits not the one you want.

git checkout -- filename # to discard changes to that file in the working directory.


#******************************************************************************************************************************

#**********************************************## remote********************************************************************************


# create a connection with remote
git remote add origin repourl

# push local repository to the remote repository.
git push -u origin master

# pull changes from a remote

git pull origin master

# check for changes in the remote repository

git fetch origin

# rename remote branch

git push origin localbranchname:remotebranchname


#delete  remote branch
git push origin :remotebranchname # basically this pushes nothing (space before the colon) 

# or 2nd option is to use delete

git push origin --delete remotebranchname

# this gets you the remote repo url
git remote show origin


#******************************************************************************************************************************

#********************************************## clone**********************************************************************************

git clone repo url # clones the remote repository to your local and pulls allthe changes. You can't push changes to the remote repo, unless

#you are authorized to do so.




#******************************************************************************************************************************

## fork - fork an existing repository to your account. You will get a copy of the repository and you can work on the forked repository all you want.
# you can then use pull requests to suggest your changes to the original repository

#***********************************************## rebase*******************************************************************************

# similar to merge but maintains a cleaner history. When you are trying to merge from one branch to another branch, you can rebase it so that 
# git doesn't create merge commits.

# This operation works by going to the common ancestor of the two branches (the one you’re on and the one you’re rebasing onto), getting the diff introduced by each
# commit of the branch you’re on, saving those diffs to temporary files, resetting the current branch to the same commit as the branch you are rebasing onto,
# and finally applying each change in turn.

git checkout featurebranch
git rebase master
git checkout master
git merge featurebranch


#you can do rebase onto
# create a server branch. make commits, create a client branch from server commit. make fast forward changes to master.
# if you want to commit client to master without server changes, you can do 

git rebase --onto master server client
git checkout master
git merge client

#then you can rebase server when you are ready

git rebase master server

git checkout master
git merge server
#******************************************************************************************************************************


#*************************************************Stashing*****************************************************************************

# lets you cache / save changes to the working directory in a temp cache. The working directory changes will be lost. But you can always get it
# back from the stash 

git stash

# to apply a stash - this applies the most recent stash

git stash apply

git stash pop # this will apply your stash and drop it from your stack

# to apply a different stash use the name

# to list stashes

git stash list

# to drop stashes
git stash drop

# lets say you created a stash with staged changes (in staging area)
git stash apply --index

# if you want to keep the files in the staging area but not in the working directory
git stash --keep-index

# by default stash will only use tracked files. to include untracked files
git stash --include-untracked 
#or
git stash -u

# create branch from a stash
git stash branch branchname # this will create a branch from the place where you stashed, apply the stash changes to that branch and delete the stash if it was applied correctly.



#******************************************************************************************************************************


#**************************************************Tags****************************************************************************

# 2 types of tags
    - lightweight Tags
    - annotated tags

    git tag -a tagname -m "message"

    git tag -a tagname -m 'message' commitid

# you can push tags to gitHub
#******************************************************************************************************************************
