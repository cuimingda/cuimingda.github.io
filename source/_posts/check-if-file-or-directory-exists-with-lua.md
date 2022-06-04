---
title: 如何在Lua中检测文件或者目录是否存在
date: 2022-06-04 20:05:39
---

检查目录是否存在

```lua
local is_directory_exists = function(dir)
  return vim.fn.empty(vim.fn.glob(dir)) == 0
end
```

比如我们可以用来检测``packer``是否已经安装，如果没有安装，就用``git clone``下载安装

```lua
local install_path = fn.stdpath('data') .. '/site/pack/packer/start/packer.nvim'

if not is_directory_exists(install_path) then
  -- do something
end
```

检查文件是否存在

```lua
local file_exists = function(name)
  local file = io.open(name, "r")
  if file ~= nil then
    io.close(file)
    return true
  else
    return false
  end
end
```

比如可以用来判断``packer``是否已经编译，如果没有编译就执行同步进行编译：

```lua
local packer_compiled_path = fn.stdpath('config') .. '/plugin/packer_compiled.lua'

if not file_exists(packer_compiled_path) then
  require('packer').sync()
end
```

## References

* <https://stackoverflow.com/questions/1340230/check-if-directory-exists-in-lua>
