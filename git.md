# Git Tutorial: Basics

### Initialize a repository
* ``git init``
* ``git status``

### Add files and commit
* ``git add file1 file2 file3``
* ``git commit -m"commit message"``
* ``git log``

### Add more files
* ``git add -A``

### Branch
* ``git branch new-branch``
* ``git checkout new-branch``
* ``git branch -b new-branch``

### Push and Pull
* git clone http://github.com/yourAccount/repoName
* git pull
* git push -u origin master

# Git Tutorial: Workflow

#### If you haven't clone the project yet

``git clone https://github.com/idreamsintern/DiscoverOnTravel``

#### First thing you do every day
```
git pull
```

#### Create `new_branch` and Work on `new_branch`

``git checkout -b new_branch``

#### After you finished your `own_branch` for the day
```
git add -A
git commit -m "complete branch own_branch"
git push origin own_branch
```

#### Others pushed on the same `conflict_branch` before you
```
git add -A
git commit -m "complete branch conflict_branch"
git pull
git push origin new_branch
```

#### Merge and Delete `local_branch `
(merge `completed_branch` into `master`)
```
git checkout master
git merge local_branch
git branch -D local_branch
```

#### Delete `remote_branch` (delete completed_branch in remote)
```
git push origin :remote_branch
git remote prune origin
git push origin master
```

#### Go to the point at last commit
```
git reset --hard
```

#### Unstage all staged files
```
git reset
```
