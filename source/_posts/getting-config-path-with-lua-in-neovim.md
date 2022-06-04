---
title: 如何在Neovim中获取数据目录和配置目录
date: 2022-06-04 20:15:36
---

关键就是``stdpath``函数，下面是获取数据目录，对应``$HOME/.local/share/nvim``目录，注意最后是没有``/``的，使用``..``叠加子目录或者文件名的时候，记得在前面加``/``：

```lua
print(vim.fn.stdpath('data'))
```

下面的目录会输出``Neovim``配置的跟目录，就是``init.lua``所在的那级目录

```lua
print(vim.fn.stdpath('config'))
```

## References

* <https://neovim.io/doc/user/nvim.html>
