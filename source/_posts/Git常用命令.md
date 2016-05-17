---
title: Git常用命令
comments: true
date: 2016-05-17 11:23:37
tags:
---

* 修改指定提交的author信息：
```
git commit --amend --author="Author Name <email@address.com>"
```

* [修改历史提交中的author信息][Change commit author at one specific commit]

* [修改仓库的第一次提交的信息][Change first commit of project with Git]
```
git rebase -i --root
```

* 删除远程分支
```
git push origin --delete branchToDelete
```

* 打tag
```
git tag -a versionNum -m "commit message"
```

* 重命名当前分支
```
git branch -m <newBranchName>
```






[Change commit author at one specific commit]:http://stackoverflow.com/a/3042512/2674213
[Change first commit of project with Git]:http://stackoverflow.com/a/2309391/2674213
