---
title: 在Lua中将两个字符串合并为一个字符串
date: 2022-06-04 16:21:07
---

官网的例子，已经非常直接了：

```lua
print("Hello " .. "World")  --> Hello World
print(0 .. 1)               --> 01
```

再来一个实际例子，获取``packer``的安装地址，先确定neovim的数据目录是哪里：

```lua
:lua print(vim.fn.stdpath('data'))
$HOME/.local/share/nvim
```

``packer``的安装地址

```lua
local install_path = vim.fn.stdpath('data') .. '/site/pack/packer/start/packer.nvim'
```
