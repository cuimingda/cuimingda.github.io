---
title: setting-default-branch-name-for-git-init
date: 2022-05-24 21:24:41
---

当用``git init``初始化一个项目的时候，现在会有以下提示：

```text
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint:  git config --global init.defaultBranch name
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint:  git branch -m name
```

这其实涉及到社区里对于master和slave的嫌弃，这个提示中已经说的很详细了，默认创建的项目分支名称还是``master``，但是我们可以用下面的方式改名：

```shell
git branch -m main
```

当然更直接的，就是加一个全局设置，以后git初始化就都是新的分支名称了，这个设置功能是从2.28版本加进来的。

```shell
git config --global init.defaultBranch main
```

通过``--list``参数，或者只查询这一个属性，都可以确认是不是已经设置成功

```shell
git config --list
git config init.defaultBranch
```

#### References

* <https://git-scm.com/docs/git-config#Documentation/git-config.txt-initdefaultBranch>
