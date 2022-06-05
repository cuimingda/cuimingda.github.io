---

title: 在Neovim中基于LSP配置Json开发环境
date: 2022-06-05 18:14:14

---

## 安装服务端

```shell
npm i -g vscode-langservers-extracted
```

## 配置

新增`lua/servers/jsonls.lua`

```lua
require 'lspconfig'.jsonls.setup {}
```

在`init.lua`中增加引用

```lua
require('servers.jsonls')
```

## References

* <https://github.com/neovim/nvim-lspconfig>
