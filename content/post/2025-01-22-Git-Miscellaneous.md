---
title:       "Git Miscellaneous"
subtitle:    ""
description: ""
date:        "2025-01-22T16:26:28Z"
author:      "Hao Xu"
image:       ""
tags:        ["Git"]
categories:  ["Tech" ]
draft:       false
---

```bash
git status                // 查看状态  
git add <file>            // 添加文件  
git commit -m "message"   // 提交  
git log                   // 查看提交记录  
git pull origin <branch>  // 拉取远程分支  
git push origin <branch>  // 推送到远程分支  
```

<!--more-->

![Git](/img/2025-01-22-Git-Miscellaneous/git.png)

# 创建本地仓库并推送到远程仓库

```bash
mkdir <repo_name>  
cd <repo_name>  
git init                                    // 初始化本地仓库  
echo "# My New Repository" > README.md
git add README.md  
git commit -m "first commit"  
git remote add origin <remote_repo>         // 添加远程仓库  
git push origin master  
```

# 克隆分支

```bash
git clone <remote_repo>  
git clone -b <branch> <remote_repo>         // 克隆指定分支
```

# 分支操作

```bash
git branch                  // 查看本地所有分支  
git branch -r               // 查看远程所有分支  
git branch -a               // 查看本地和远程所有分支  
git checkout <branch>       // 切换分支  
git checkout -b <branch>    // 创建并切换分支  
```
