---
title: 如何在Lua中处理require依赖不存在的问题
date: 2022-06-04 20:08:55
---

如果像下面这样引入``packer``，并且``packer``还没有安装，那是会报错的

```lua
local packer = require('packer')
```

这时可以借助``pcall``函数，这样就可以区分是否``require``成功了，返回结果有两个值，第一个是``Boolean``，后面一个如果成功返回对象，如果失败返回``nil``

```lua
local require_packer_succeed, packer = pcall(require, "packer")

if require_packer_succeed then
  return packer.startup(packer_startup)
end
```

## References

* <https://github.com/nanotee/nvim-lua-guide>
