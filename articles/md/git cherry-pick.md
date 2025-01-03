---
title: "git cherry-pick"
tag: "Git"
time: 2024-09-01 15:21:24
---

> 应用场景对于多分支的代码库，将代码从一个分支转移到另一个分支是常见需求。这时分两种情况。一种情况是，你需要另一个分支的所有代码变动，那么就采用合并（git merge）。另一种情况是，你只需要部分代码变动（某几个提交），这时可以采用 Cherry pick。

## 基本用法

`git cherry-pick` 命令的作用，就是将指定的提交（`commit`）应用于其他分支。

```sh
git cherry-pick <commitHash>
```

上面命令就会将指定的提交 commitHash，应用于当前分支。这会在当前分支产生一个新的提交，当然它们的哈希值会不一样。

举例来说，代码仓库有 master 和 feature 两个分支。

```sh
a - b - c - d   Master
         \
           e - f - g Feature
```

现在将提交 f 应用到 master 分支。

```sh
# 切换到 master 分支
$ git checkout master
# Cherry pick 操作
$ git cherry-pick f
```

上面的操作完成以后，代码库就变成了下面的样子。

```sh
a - b - c - d - f   Master
         \
           e - f - g Feature
```

`git cherry-pick` 命令的参数，不一定是提交的哈希值，分支名也是可以的，表示转移该分支的最新提交。

```sh
$ git cherry-pick feature
# 上面代码表示将feature分支的最近一次提交，转移到当前分支。
```

## 转移多个提交

Cherry pick 支持一次转移多个提交。

```sh
git cherry-pick <HashA> <HashB>
```

上面的命令将 A 和 B 两个提交应用到当前分支。这会在当前分支生成两个对应的新提交。

如果想要转移一系列的连续提交，可以使用下面的简便语法。

```sh
git cherry-pick A..B
```

上面的命令可以转移从 A 到 B 的所有提交。它们必须按照正确的顺序放置：提交 A 必须早于提交 B，否则命令将失败，但不会报错。

注意，使用上面的命令，提交 A 将不会包含在 Cherry pick 中。如果要包含提交 A，可以使用下面的语法。

```sh
git cherry-pick A^..B
```
