---

title: mapping-domain-to-localhost-port

---

既然要将一个域名指向运行在本地端口的特定服务，实际上是将某个特定域名的80端口代理到本地的另外一个端口，所以首先要在本地的某个端口运行一个服务，比如我们拿Hexo举例，默认就会运行在4000端口上。

```shell
hexo server
```

为了实现代理，下一步我们要安装nginx，这个参考另外一篇文章。

然后在`/usr/local/etc/nginx/servers`新增一个配置文件`hexo.conf`，逻辑很清晰，监控`mingda.dev.test`这个域名的`80`端口，并将`/`目录代理到`127.0.0.1`的`80`端口。

```text
server {
  listen 80;
  server_name mingda.dev.test;

  location / {
    proxy_pass http://127.0.0.1:4000;
  }
}
```

下一步就是修改本地的hosts

```shell
sudo vi /etc/hosts
```

然后将这个域名指向本地就好

```text
127.0.0.1 mingda.dev.test
```

这个时候我们已经可以使用`mingda.dev.text`来访问部署在本地`4000`端口的Hexo服务了。

## 参考资料

* <https://www.linode.com/docs/guides/how-to-configure-nginx/#nginx-configuration-of-reverse-proxy>
* <https://gist.github.com/soheilhy/8b94347ff8336d971ad0>
* <https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/>
