---
title: 如何清空ssh私钥对应的passphrase？
date: 2022-05-23 20:26:48
tags:
---

第一次创建ssh key的时候，提示可以设置一个passphrase，抱着更安全一点总是没有问题的心思，就设置了一个很长的密码短语，结果每一次ssh登录服务器，都需要输入，本来是为了方便，结果变成了每一次都很不方便。

其实这个passphrase是保存在私钥中的，可以通过``ssh-keygen``删除，并且不会影响到公钥。

具体先执行下面命令

```shell
ssh-keygen -p
```

这时候会先选择修改哪个密钥，然后会提示输入之前的passphrase，然后输入新的就好，这里因为我们是要删除，只要连续输入两个回车，就可以清空passphrase了。

#### References

* <https://www.simplified.guide/ssh/set-remove-passphrase>
