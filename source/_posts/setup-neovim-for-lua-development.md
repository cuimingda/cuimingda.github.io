---
title: 在Neovim中配置Lua开发环境
date: 2022-06-04 23:40:37
---

安装``LSP``服务端：

```shell
brew install lua-language-server
```

创建一个配置目录：

```shell
mkdir -p lua/lspconfig/sumneko_lua && cd $_
```

创建``init.lua``配置

```lua
require 'lspconfig'.sumneko_lua.setup {
  cmd = { "lua-language-server", "--locale=zh-CN" },
  settings = {
    Lua = {
      format = {
        defaultConfig = {
          indent_style = "space",
          indent_size = "2",
          tab_width = "2",
          continuation_indent_size = "2"
        }
      },
      diagnostics = {
        globals = { "vim" }
      }
    }
  }
}
```

在``init.lua``增加配置文件的引用：

```lua
require('lspconfig.sumneko_lua')
```

## References

* <https://github.com/sumneko/lua-language-server>
