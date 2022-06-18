---

title: installing-nginx-on-macos-monterey-with-homebrew

---

## 安装准备

需要先删除`/usr/local/var/www`目录，因为没有删除，后续引发了bug。

之前mac是有Apache的，也用了`/usr/local/var/www`这个目录，但是没有`index.html`，而是把`index.html`放到了`htdocs`子目录下，而根据`https://github.com/Homebrew/homebrew-core/blob/master/Formula/nginx.rb`的安装脚本，如果有www目录，则不会复制新的www目录，默认没有开启autoindex，自然就会报403错误，这是一系列连锁反应。

```python
def post_install
  (etc/"nginx/servers").mkpath
  (var/"run/nginx").mkpath

  # nginx's docroot is #{prefix}/html, this isn't useful, so we symlink it
  # to #{HOMEBREW_PREFIX}/var/www. The reason we symlink instead of patching
  # is so the user can redirect it easily to something else if they choose.
  html = prefix/"html"
  dst = var/"www"

  if dst.exist?
    html.rmtree
    dst.mkpath
  else
    dst.dirname.mkpath
    html.rename(dst)
  end

  prefix.install_symlink dst => "html"

  # for most of this formula's life the binary has been placed in sbin
  # and Homebrew used to suggest the user copy the plist for nginx to their
  # ~/Library/LaunchAgents directory. So we need to have a symlink there
  # for such cases
  sbin.install_symlink bin/"nginx" if rack.subdirs.any? { |d| d.join("sbin").directory? }
end
```

所以一开始一定要删除这个目录，当然要先备份：

```shell
rm -rf /usr/local/var/www
```

## 安装

用Homebrew安装软件前，先用`brew info nginx`查看一下包的信息，会是很好的习惯：

```text
nginx: stable 1.21.6 (bottled), HEAD
HTTP(S) server and reverse proxy, and IMAP/POP3 proxy server
https://nginx.org/
Not installed
From: https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/nginx.rb
License: BSD-2-Clause
==> Dependencies
Required: openssl@1.1 ✔, pcre2 ✔
==> Options
--HEAD
	Install HEAD version
==> Caveats
Docroot is: /usr/local/var/www

The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.

nginx will load all files in /usr/local/etc/nginx/servers/.

To restart nginx after an upgrade:
  brew services restart nginx
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/nginx/bin/nginx -g daemon off;
==> Analytics
install: 25,326 (30 days), 90,398 (90 days), 452,778 (365 days)
install-on-request: 25,289 (30 days), 90,205 (90 days), 451,843 (365 days)
build-error: 35 (30 days)
```

比如上面这段内容，明确了版本是`1.21.6`，我们可以知道这个版本不是最新版本。

比如这里明确了Docroot是`/usr/local/var/www`，这是非常关键的信息。

```shell
brew install --verbose nginx
```

备忘一下安装日志，后续可能有用

```text
Running `brew update --auto-update`...
==> Auto-updated Homebrew!
Updated 1 tap (homebrew/core).

You have 6 outdated formulae and 1 outdated cask installed.
You can upgrade them with brew upgrade
or list them with brew outdated.

==> Downloading https://ghcr.io/v2/homebrew/core/nginx/manifests/1.21.6_1
Already downloaded: /Users/cuimingda/Library/Caches/Homebrew/downloads/4fb2767b05e700f1cd175f9f7ac8ce64a15a9f87c8cf8ff9b9897aea93965bce--nginx-1.21.6_1.bottle_manifest.json
==> Downloading https://ghcr.io/v2/homebrew/core/nginx/blobs/sha256:dba847687a67f4a25202f00c27821d2d39f0eddeb28fa8d3d1126e574706b042
Already downloaded: /Users/cuimingda/Library/Caches/Homebrew/downloads/acde7c45c2479896faa512a9a4fd8ad6f4aa21cffb866e9b9dea7b4e1ab88db0--nginx--1.21.6_1.monterey.bottle.tar.gz
==> Verifying checksum for 'acde7c45c2479896faa512a9a4fd8ad6f4aa21cffb866e9b9dea7b4e1ab88db0--nginx--1.21.6_1.monterey.bottle.tar.gz'
==> Pouring nginx--1.21.6_1.monterey.bottle.tar.gz
tar --extract --no-same-owner --file /Users/cuimingda/Library/Caches/Homebrew/downloads/acde7c45c2479896faa512a9a4fd8ad6f4aa21cffb866e9b9dea7b4e1ab88db0--nginx--1.21.6_1.monterey.bottle.tar.gz --directory /private/tmp/d20220618-6989-1hogpg9
cp -pR /private/tmp/d20220618-6989-1hogpg9/nginx/. /usr/local/Cellar/nginx
chmod -Rf +w /private/tmp/d20220618-6989-1hogpg9
==> Finishing up
ln -s ../Cellar/nginx/1.21.6_1/bin/nginx nginx
mkdir -p /usr/local/share/man/man8
ln -s ../../../Cellar/nginx/1.21.6_1/share/man/man8/nginx.8 nginx.8
ln -s ../Cellar/nginx/1.21.6_1/share/nginx nginx
==> Caveats
Docroot is: /usr/local/var/www

The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.

nginx will load all files in /usr/local/etc/nginx/servers/.

To restart nginx after an upgrade:
  brew services restart nginx
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/nginx/bin/nginx -g daemon off;
==> Summary
🍺  /usr/local/Cellar/nginx/1.21.6_1: 26 files, 2.2MB
==> Running `brew cleanup nginx`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
```

## 运行

先检查当前的运行状态

```shell
brew services info nginx
```

运行的命令是start，但是第一次安装报了一个错，大概率是因为之前卸载了重新安装的关系，稳妥一些，执行restart。

```shell
brew services restart nginx
```

## 显示nginx进程

这里将`n`用中括号圈起来，可以过滤掉`grep`本身的进程，在后面加一个冒号，是为了规避正在编辑中的文件名包含nginx字样。

```shell
ps -fe | grep '[n]ginx:'
```

当然，用homebrew命令也可以

```shell
brew services list
```

## 异常

访问8080端口，现在提示403 Forbidden，网上各种解决方案，其中有设置777权限的，这种方案通常都不应该建议；有修改nginx.conf中user属性的，Homebrew已经把这块封装好了，就是当前用户；有说到用sudo运行nginx的，这个是最不推荐的，相当于晚上睡觉不锁门。根本的问题只是nginx和apache在mac上的目录冲突了，仅此而已。

## 参考资料

* <https://www.cyberciti.biz/tips/grepping-ps-output-without-getting-grep.html>
