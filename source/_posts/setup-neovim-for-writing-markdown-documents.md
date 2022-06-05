---

title: 在Neovim中配置markdown书写环境
date: 2022-06-05 12:00:11

---

## 安装服务端

```shell
npm install -g remark-language-server
```

## 配置客户端

增加一个配置文件`lua/servers/remark_ls.lua`

```lua
require 'lspconfig'.remark_ls.setup {}
```

在`init.lua`中增加引用

```lua
require('servers.remark_ls')
```

## 项目配置文件

在每一个项目中，都要增加对应的一个配置

```shell
npm install remark-preset-lint-markdown-style-guide
```

并且在根目录增加一个`.remarkrc.json`

```json
{
  "plugins": [
    "remark-preset-lint-markdown-style-guide",
    ["remark-lint-list-item-indent", "space"],
    ["remark-lint-unordered-list-marker-style", "*"],
    ["remark-lint-heading-style", false],
    ["remark-lint-maximum-line-length", false],
    ["remark-lint-maximum-heading-length", false],
    ["remark-lint-fenced-code-marker", "consistent"]
  ],
  "settings": {
    "rule": "-",
    "ruleRepetition": 3,
    "listItemIndent": 1,
    "bullet": "*"
  }
}
```

## References

* <https://github.com/remarkjs/remark/tree/main/packages/remark-stringify#api>
* <https://github.com/remarkjs/remark-lint#rules>
* <https://www.npmjs.com/package/remark-preset-lint-markdown-style-guide>
* <https://www.npmjs.com/package/remark-preset-lint-consistent>
* <https://www.npmjs.com/package/remark-preset-lint-recommended>
* <https://github.com/op3/boris/blob/master/.remarkrc.json>
