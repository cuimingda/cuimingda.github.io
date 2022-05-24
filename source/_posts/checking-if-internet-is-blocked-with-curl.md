---
title: 基于curl判断当前网络是否处于封锁状态
date: 2022-05-23 22:00:55
tags:
---

``curl``是一个超级厉害的网络工具，我们可以基于``curl``抓取远程的文件，同时我们也可以基于``curl``判断当前网络的连接情况，判断是否被封锁。

具体是下面这个命令：

```shell
curl --location --head --connect-timeout 3 --verbose https://google.com
```

``--location``可以缩写为``-L``，因为网站有跳转，可以跳转到最终的页面。
``--head``可以缩写为大写的``-I``，只显示请求头，我们只是用来判断，显示头部信息就足够了。
``--connect-timeout``设置了一个超时时间，如果网络被屏蔽，这个参数就发挥作用了，直接中断请求。
``--verbose``可以显示更多的信息。

最后注意这里要使用``https://google.com``，而不是``google.com``，如果是后者，实际测试超时时间会无效，一直处于连接状态。

#### References

* <https://linuxtect.com/how-to-set-timeout-for-curl-command/>
* <https://linuxhandbook.com/curl-timeout/>
