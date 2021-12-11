---
title: git 相关笔记
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

- `feat` 增加新功能
- `fix` 修复问题/BUG
- `style` 代码风格相关无影响运行结果的
- `perf` 优化/性能提升
- `refactor` 重构
- `revert` 撤销修改
- `test` 测试相关
- `docs` 文档/注释
- `chore` 依赖更新/脚手架配置修改等
- `workflow` 工作流改进
- `ci` 持续集成
- `types` 类型定义文件更改
- `wip` 开发中
