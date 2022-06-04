---
title: 如何在Google表格中计算平均值的时候忽略N/A
date: 2022-06-03 12:44:30
---

如果要在Google Spreadsheets中计算平均值，可以使用``average``函数，这个函数会忽略所有的空值，但是如果要计算平均值的部分，有通过公式生成的结果，并且公式没有得出结果，是``N/A``的话，最后``average``的计算结果也会是``N/A``。

要解决这个问题，我们需要在计算平均值的时候，将所有的``N/A``忽略掉，不能直接使用``average``函数，而要使用``averageif``函数：

```excel
=averageif(E:E, "<>#N/A")
```

## References
* <https://en.wikipedia.org/wiki/N/A>
* <https://www.get-digital-help.com/average-ignore-na/>
