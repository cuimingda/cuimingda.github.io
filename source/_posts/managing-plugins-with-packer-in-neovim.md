---
title: 用packer.nvim来管理Neovim的插件
date: 2022-06-04 20:11:29
---

第一步是安装

```shell
git clone --depth 1 https://github.com/wbthomason/packer.nvim \
          ~/.local/share/nvim/site/pack/packer/start/packer.nvim
```

然后在``Neovim``配置目录，创建``lua``子目录，并且增加一个``plugins.lua``：

```lua
local fn = vim.fn
local install_path = fn.stdpath('data') .. '/site/pack/packer/start/packer.nvim'
local packer_compiled_path = fn.stdpath('config') .. '/plugin/packer_compiled.lua'

local is_directory_exists = function(dir)
  return fn.empty(fn.glob(dir)) == 0
end

local file_exists = function(name)
  local file = io.open(name, "r")
  if file ~= nil then
    io.close(file)
    return true
  else
    return false
  end
end

local packer_startup = function(use)
  use "wbthomason/packer.nvim"
  use 'neovim/nvim-lspconfig'

  if not file_exists(packer_compiled_path) then
    require('packer').sync()
  end
end

if not is_directory_exists(install_path) then
  print('Installing packer plugin...')
  fn.system({ 'git', 'clone', '--depth', '1', 'https://github.com/wbthomason/packer.nvim', install_path })
  print('All done, please restart Neovim.')
  do return end
end

local require_packer_succeed, packer = pcall(require, "packer")
if require_packer_succeed then
  return packer.startup(packer_startup)
end
```

最后在``init.lua``中增加引用：

```lua
require('plugins')
```

## References

* <https://github.com/wbthomason/packer.nvim>
