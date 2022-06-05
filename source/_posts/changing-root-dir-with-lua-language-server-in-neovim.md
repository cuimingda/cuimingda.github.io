---
title: 如何修改Lua Language Server运行时的根目录
date: 2022-06-05 11:24:38
---

打开`lua`文件的时候，提示了一个错误，大意就是工作目录被设置到了用户根目录，Lua LS拒绝加载此目录，让用户检查配置，还给出了[官网的说明页面](https://github.com/sumneko/lua-language-server/wiki/Why-scanning-home-folder)

在[nvim-lspconfig的配置页面](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md#sumneko_lua)，找到了`summeko_lua`的默认配置：

```lua
root_pattern(".luarc.json", ".luacheckrc", ".stylua.toml", "selene.toml", ".git")
```

我在项目目录确实使用了`ln -s`，前面几个没有匹配到，但原则上应该可以匹配到`.git`，既然没有匹配到，让我们收到修改为根据`package.json`来匹配根目录：

```lua
root_dir = root_pattern("package.json"),
```

这里使用了`root_pattern`函数，记得要引用进来

```lua
local root_pattern = require('lspconfig.util').root_pattern
```

## References

* <https://github.com/sumneko/lua-language-server/wiki/Why-scanning-home-folder>
* <https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md#sumneko_lua>
