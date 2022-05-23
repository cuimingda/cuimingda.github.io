---
title: 如何在git中同时推送commit和tag？
date: 2022-05-23 21:27:50
tags:
---

一直有一个困惑，就是执行``git push``命令的时候，只能将所有的commits推送到服务端，如果要想把tags也推送上去，需要再执行一个``git push --tags``命令。这样很麻烦。

发现``git``有一个参数``--follow-tags``可以解决这个困惑，带上这个参数，可以把这个commit对应的tag一起推送上去。

```shell
git push --follow-tags
```

如果每一次都这样操作，其实也很麻烦，毕竟这个参数很长，我们可以在全局打开这个配置开关：

```shell
git config --global push.followTags true
```

#### References

* [Push git commits & tags simultaneously](https://stackoverflow.com/questions/3745135/push-git-commits-tags-simultaneously)
* <https://www.bookstack.cn/read/git-tutorial/docs-tag.md>
