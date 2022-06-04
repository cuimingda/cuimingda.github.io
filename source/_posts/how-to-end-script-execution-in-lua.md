---
title: 如何终止Lua脚本的执行
date: 2022-06-04 20:08:09
---

在执行``Lua``脚本的过程中，判断了一个前置条件，如果这个条件不满足，那么后续的代码就都不执行了，比如一个场景的场景就是判断``Neovim``的版本，如果不是``0.7.0``，就不执行后面的配置，提示先更新``Neovim``的版本。关键的语法就是``return end``：

```lua
if not is_directory_exists(install_path) then
  do return end
end
```

## References

* <https://stackoverflow.com/questions/20188458/how-to-exit-a-lua-scripts-execution>
