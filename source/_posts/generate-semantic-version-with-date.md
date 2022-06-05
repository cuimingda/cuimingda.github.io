---

title: 根据当前日期的年月日生成语义化的版本号
date: 2022-06-05 23:28:16

---

如果今天是2022年5月2日，那么希望生成`22.5.2`这种版本号，并且包装成一个`package.json`中的`script`命令。

首先要生成日期的年份：

```shell
echo $(date +"%Y")
" 2022
```

为了截取日期年份字符串的后两位，我们用到`cut`命令，第一个字符的序号是`1`：

```shell
echo $(cut -c 3-4 <<< $(date +"%Y"))
" 22
```

然后是获取当前日期的月份

```shell
echo $(date +"%m")
" 05
```

为了成为版本号，需要将前置的`0`去掉，这里用`sed`来做一个正则表达式替换：

```shell
echo $(date +"%m") | sed "s/^0*//"
" 5
```

最后是获取日期是第几天，当然也需要把前置`0`去掉：

```shell
echo $(date +"%d") | sed "s/^0*//"
" 2
```

组合起来，需要将`"`转换成`\"`，最后的结果就是`22.5.2`：

```json
"scripts": {
  "version:date": "npm version $(cut -c 3-4 <<< $(date +\"%Y\")).$(echo $(date +\"%m\") | sed \"s/^0*//\").$(echo $(date +\"%d\") | sed \"s/^0*//\")"
},
```

## References

* <https://www.theunixschool.com/2013/03/how-to-remove-leading-zeros-in-string.html>
* <https://phoenixnap.com/kb/linux-date-command>
