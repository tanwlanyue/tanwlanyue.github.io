---
title: Git
date: 2024-03-21 23:12:54
tags: Git
categories: 工具
---

# Git配置

```shell
# 生成 SSH Key
ssh-keygen -t rsa -C "tanwlanyue@gmail.com"
# 查看生成的 SSH 公钥
cat ~/.ssh/id_rsa.pub
# 测试
ssh -T git@gitee.com
# 用户设置
git config --global user.name "zhanglei"
git config --global user.email "tanwlanyue@gmail.com"
# ssl设置
git config --global http.sslVerify "false"
git config --global https.sslVerify "false"
# 解决Git中文乱码
git config --global core.quotepath false
git config --global gui.encoding utf-8
```
<!-- more -->
# Git命令

```shell
# 删除分支
git branch -D branchName
# 修改最近的 commit message
git commit --amend
# 修改过去的 commit message
git rebase -i parentCommitId # 选择需要修改的commit  前面的pick改为r
# 合并连续的commit
git rebase -i parentCommitId # 选择需要修改的commit  前面的pick改为s
# 查看最近三次提交记录
git log -3
```
