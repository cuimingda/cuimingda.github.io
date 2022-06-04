---
title: 用腾讯云的web函数操作cynosdb数据库
date: 2022-05-29 13:43:32
---

## 准备

* 首先要有一个cynosdb数据库，在[控制台](https://console.cloud.tencent.com/cynosdb)直接可以购买，我选择的是serverless版本。
* 要有一个VPC和一个子网，这样才能用云函数访问这个数据库。
* 要创建一个vpc网关，但不用关联任何api，这样就有了apigw的参数


## 环境变量

```shell
$ node -v
v16.15.0

$ npm -v
8.8.0
```

## 初始化

```shell
mkdir myfunc && cd $_
```

## serverless.yml

第一个级别是几个meta级别的属性

```YAML
component: scf
name: componentInstanceName
app: appName
inputs:
```

### inputs参数

肯定要把``.env``排除，``node_modules``不排除，而是改成``npm install --production``搞定。

```YAML
src:
  src: ./
  exclude:
    - .env
```

type只有两个类型：web和event，我们是用来做api，这里就是web

```YAML
type: web
```

当类型是web的时候，可以设置``entryFile``参数，这样就不需要在代码里添加到``scf_bootstrap``文件了，会根据``entryFile``属性动态增加。

```YAML
entryFile: app.js
```

Web函数只在广州、上海和北京的特定区才有，所以要指定区域

```YAML
region: ap-guangzhou
```

运行环境只有几个特定的环境

```YAML
runtime: Nodejs16.13
```

在超时这个事情上，我设置了三个级别，连接数据库最多1秒，初始化3秒，运行20秒。实际上这些也太高了。

```YAML
timeout: 20
initTimeout: 3
```

因为将``.env``排除了，所以在``app.js``中是无法读取到``.env``中的变量的，在这里要转换一下。

```YAML
environment:
  variables:
    DATABASE_HOST: ${env:DATABASE_HOST}
    DATABASE_USER: ${env:DATABASE_USER}
    DATABASE_PASSWORD: ${env:DATABASE_PASSWORD}
    DATABASE_DATABASE: ${env:DATABASE_DATABASE}
```

对于api网关，最主要的是要指定``serviceId``，否则每次``deploy``都会创建新的，但这里也遗留一个问题，就算这样写，每一次都会有一个解绑和重新绑定的过程，觉得是可以省略的。另外``method``只要一个``ANY``，就可以满足所有情况了，如果在这里指定``GET``，反而需要每个路由单独设置。

```YAML
events:
  - apigw:
      parameters:
        serviceId: service-XXXXXXXX
        protocols:
          - https
        environment: release
        endpoints:
          - path: /
            method: ANY
```

一定要设置vpc，这样才能从云函数中访问同一个vpc的数据库。vpcId可以在[私有网络控制台](https://console.cloud.tencent.com/vpc)中看到，subnetId可以在[子网控制台](https://console.cloud.tencent.com/vpc/subnet)中看到。

```YAML
vpcConfig:
  vpcId: vpc-XXXXXXXX
  subnetId: subnet-XXXXXXXX
```

下面是一个完整版本

```YAML
component: scf
name: thinking
app: notes

inputs:
  src:
    src: ./src
    exclude:
      - .env
  type: web
  entryFile: app.js
  region: ap-guangzhou
  runtime: Nodejs16.13
  timeout: 20
  initTimeout: 3
  environment:
    variables:
      DATABASE_HOST: ${env:DATABASE_HOST}
      DATABASE_USER: ${env:DATABASE_USER}
      DATABASE_PASSWORD: ${env:DATABASE_PASSWORD}
      DATABASE_DATABASE: ${env:DATABASE_DATABASE}
  events:
    - apigw:
        parameters:
          serviceId: ${env:APIGW_SERVICE_ID}
          protocols:
            - https
          endpoints:
            - path: /
              method: ANY
  vpcConfig:
    vpcId: ${env:VPC_ID}
    subnetId: ${env:SUBNET_ID}
```

准备``.env``文件

```
DATABASE_HOST=
DATABASE_USER=
DATABASE_PASSWORD=
DATABASE_DATABASE=

VPC_ID=
SUBNET_ID=
APIGW_SERVICE_ID=
```

然后创建``src``目录
```
mkdir src && cd $_
npm init -y
```

安装express和mysql

```shell
npm install express mysql
```

## app.js

对于express来说，都是基础用法，这里最重要的点，是必须运行在9000端口号上，其他的都不行。

```JavaScript
const express = require('express')
const app = express()
const port = 9000

app.get('/', (req, res) => {
})

app.post('/notes', function(req, res) {
})

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`)
})
```

如果要返回json数据，只要基于res对象操作就可以

```JavaScript
res.send([
 {
  foo: "bar"
 }
])
```

## 数据库操作

引入

```JavaScript
const mysql = require('mysql');
```

创建数据库连接，这里用到了env中定义的变量，总体的思路就是先连接，连接成功操作，然后关闭。

```JavaScript
const connection = mysql.createConnection({
  connectTimeout: 1000,
  host     : process.env.DATABASE_HOST,
  user     : process.env.DATABASE_USER,
  password : process.env.DATABASE_PASSWORD,
  database : process.env.DATABASE_DATABASE
});

connection.connect(function() {
  connection.query();
  connection.end();
});
```

查询一个表，喜欢iu需要用query执行SELECT，results是查询结果，fields对应字段列表

```JavaScript
connection.query('SELECT * from notes', function (error, results, fields) {
});
```

数据库连接如果失败，对应的error对象有以下关键信息：

* error.stack 错误堆栈
* error.code 错误代码
* error.errno 一个数值类的错误代码
* error.fatal 是否是一个致命错误
* error.sql 出错的时候，执行的SQL命令
* error.sqlMessage 错误提示

## 部署

最后一步就是将云函数部署到服务器

```shell
sls deploy
```

## References

* <https://console.cloud.tencent.com/vpc/subnet>
* <https://console.cloud.tencent.com/vpc/vpc>
