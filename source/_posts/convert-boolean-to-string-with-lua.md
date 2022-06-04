---
title: convert-boolean-to-string-with-lua
date: 2022-06-04 20:18:33
---

如果直接用``..``将一个字符串和一个Boolean类型进行连接，会报错的，提示无法转换。这时候需要我们手动将Boolean类型转换成字符串，关键就是``tostring``函数：

```lua
local booleanValue = true
local stringValue = tostring(booleanValue)
```

## References

* <https://onelinerhub.com/lua/convert-boolean-value-to-string>
