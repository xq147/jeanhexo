---
title: git
date: 2021-11-12 14:31:55
tags:
---

git相关命令操作、提交规范
<!--more-->

# 命令操作

```shell
git checkout -b newBrach origin/master // 创建分支
git clone 'branch_name'
git commit -m "commit_message" // 将修改 提交到本地仓库, 双引号内是提交的备注信息
git push origin branch_name // 拉取远程dev分支代码
git push origin branch_name // 将本地修改的代码提交到远程的dev分支上
git checkout master // 切换到master分支
git merge dev

git log // 查看近期提价的相关日志
git status // 查看当前文件的状态

```

# 提交规范

feat: 新功能（feature）
fix: 修补bug
docs: 文档（documentation）
style: 格式（不影响代码运行的变动）
refactor: 重构（即不是新增功能，也不是修改bug的代码变动）
chore: 构建过程或辅助工具的变动
revert: 撤销，版本回退
perf: 性能优化
test：测试
improvement: 改进
build: 打包
ci: 持续集成

