---
title: git-study-1
date: 2016-02-17 13:55:32
tags: git 
---
### **开始**
    
#### 1.clone
``` bash
git clone repository
git clone repository directory
```
#### 2.init
``` bash
git init 
git init directory
```
#### 3.config
``` bash
git config --global user.name "ff"
git config --global user.email bestone@is.me
git config --global core.editor vim
git config --list
```

### **本地**
#### 1.add
``` js
git add filename
git add directroy/
git add .
```
#### 2.rm
``` bash
git rm filaname
```
#### 3.commit
```bash
git commit -m 'message'
git commit --amend
```
#### 4.reset
```bash
git reset --filename
```
#### 5.checkout
```bash
git checkout branchname
git checkout filename
git checkout commit
```
#### 6.stash
```bash
git stash
git stash list
git stash apply
```

### **记录**
#### 1.status
``` bash
git status
```
#### 2.log
``` bash
git log
```
#### 3.diff
``` bash
git diff
git diff --cache
git diff commit commit
git diff branchname
```

### **分支**
#### 1.branch
``` bash
git branch 
git branch branchname
git branch -d branchname
git branch -D branchname
```
#### 2.merge
``` bash
git merge branchname
```

### **远程**
#### 1.remote
``` bash
git remote -v
git remote show
```
#### 2.push
``` bash
git push origin branchname
```
#### 3.pull
``` bash
git pull
```
#### 4.fetch
``` bash

```



