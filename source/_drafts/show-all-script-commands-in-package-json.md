---

title: 如何显示package.json中包含了哪些用script定义的命令？

---

当folk下来一个别人的项目，或者通过脚手架创建了一个空项目，第一反应就是查看`package.json`中有哪些命令可以运行，最笨的方案就是用`cat`或者任意编辑器查看。

```shell
cat package.json
```

当然我们也可以聪明一些，借助`jq`这种命令，只查看`package.json`中的特定部分。

```shell
jq .scripts package.json
```

其实在这里我们可以直接使用`npm`命令查看，只要不带任何参数，就可以列出所有的命令。

```shell
npm run
```

关于上面这条命令，在`npm help run`中是有一些说明的：

```text
This runs an arbitrary command from a package's "scripts" object.  If no "command" is provided, it will list the available scripts.
```

## References

* <https://stackoverflow.com/questions/59613870/how-to-list-all-the-commands-available-in-package-json>
* <https://docs.npmjs.com/cli/v8/commands/npm-completion>
