title: 在Neovim中配置YAML开发环境1
date: 2022-06-06 00:23:44
---

安装服务端

```shell
npm install -g yaml-language-server
```

## 配置

增加一个配置文件`lua/servers/yamlls.lua`

```lua
require 'lspconfig'.yamlls.setup {}
```

在入口文件中增加引用：

```lua
require('servers.yamlls')
```

## References

* <https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md#yamlls>
