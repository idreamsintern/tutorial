# DOT workflow

#### If you haven't clone the project yet

``git clone https://github.com/idreamsintern/DiscoverOnTravel``

#### First thing you do day
```
git pull
```

#### Create `new_branch` and Work on `new_branch`

``git checkout -b new_branch``

#### After you finish
```
git add -A
git commit -m "complete branch new_branch"
git push origin new_branch
```

#### Others pushed on the same branch before you
```
git add -A
git commit -m "complete branch new_branch"
git pull
git push origin new_branch
```

#### Merge and Delete local branch 
(merge `completed_branch` into `master`)
```
git checkout master
git merge completed_branch
git branch -D completed_branch
```

#### Delete remote branch (delete completed_branch in remote)
```
git push origin :completed_branch
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
