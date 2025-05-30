### Create user
 git config --global user.name <'name'> - add username
 git config --local user.email <'email'> - add username only this repo

### Check git loocked file
```
git ls-files - view all loocked files
git ls-files yourfile.txt - check one file
```
### add github repo 
git init - expose git
git remote add origin` 'github' - tie local repository with align repository
git remote -v - view align repository name
git remote remove 'origin' - delete align repository

>rewrite .git history on local repo:
```
rm -rf .git 
git init 
git add .
```
### git push
git add . - add all files in git directory
git commit -m 'commit' - mark this step
git push -u origin <branch'> - send current commit to align repository

>if you want to rewrite align repository on gitlab. Write:
```
git push --force origin <main>
```

### git pull
Change local repo from alighn repo
```
git fetch
git pull
```
### arrive in commit before
git log - view all commits in this branch
git checkout 'commits09012' - change head on another commit
git rm --cached - delete in git but save in directory

## another branch
git branch - view all branches
git branch <name'> - create new branch
git checkout <new branch'> - change head on new branch
git checkout <old branch'> - come in old branch

## publicate new branch
git checkout <'new branch'> - change head on new branch
git push origin <'new branch name'> - expose new branch in new branch in github

## merge new branch with old
1. change head on  general branch:
	-git checkout <name branch'>
2. - git merge 'name another branch' - add all changes from 'name another branch' to 'general branch' and commit it in new commit
3. git commit -m 'commit'
4. git push origin - push it on github


### if your issue is can't push, because you have additionaly commits on github
```
git fetch origin
git rebase origin/master
git rm 'theirChangeFile' 
git push
```