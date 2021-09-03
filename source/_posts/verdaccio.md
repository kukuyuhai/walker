---
title: 如何有verdaccio构建一个私有npm仓库？
date: 2021-1-28 15:46:20
tags: npm 私有库 verdaccio
---

### 什么是verdaccio？

> Verdaccio是一个简单的、零配置要求的本地私有 npm 注册表。无需整个数据库即可开始！Verdaccio 开箱即用，带有 自己的小型数据库，并且能够代理其他注册表（例如 npmjs.org），并在此过程中缓存下载的模块。对于那些希望扩展其存储功能的人，Verdaccio 支持各种社区制作的插件，以连接到服务，例如 Amazon 的 s3、Google Cloud Storage或创建您自己的插件。

是Fork自sinopia，由于sinopia很长时间无人维护，于是verdaccio保持的原始sinopia的几乎相同的配置。 并不断维护修复issus。

#### 安装

```sh
npm install verdaccio
#or
yarn global add verdaccio
```

#### 创建config.yaml

```sh
which verdaccio
#/usr/local/bin/verdaccio

#创建项目目录，一般存放在/data/verdaccio下

mkdir /data/verdaccio

updatedb && locate config.yaml
#/root/.config/verdaccio/config.yaml找到配置文件

cp /root/.config/verdaccio/config.yaml /data/verdaccio/

sed -ri 's#(.*url: ).*#\1https://registry.npm.taobao.org#' /data/verdaccio/config.yaml

#更新config.yaml，添加 listen:0.0.0.0:4873

```

详细配置见官网：[https://verdaccio.org/zh-CN/docs/configuration](https://verdaccio.org/zh-CN/docs/configuration)

#### 配置verdaccio服务

一般我们可以同systemctl启动服务，你也可以使用service、nohup等后台挂起服务。
```sh
# 我使用yarn安装的

cp /usr/local/share/.config/yarn/global/node_modules/verdaccio/systemd/verdaccio.service /etc/systemd/system/verdaccio.service

sed -ri 's@^(ExecStart=).*@\1/usr/bin/verdaccio --config /data/verdaccio/config.yaml@' !$
```

#### 配置完如下

```s
[Unit]
Description=Verdaccio lightweight npm proxy registry

[Service]
Type=simple
Restart=on-failure
#User=root 这里一定主要user
ExecStart=/usr/bin/verdaccio --config /data/verdaccio/config.yaml

[Install]
WantedBy=multi-user.target

```

#### 启动verdaccio服务

```sh
systemctl enable verdaccio.service
systemctl start verdaccio.service
systemctl status verdaccio.service
```

#### 配置nginx

```conf
server {
    listen          80;             # the port nginx is listening on
    server_name     xxxxx.lpniu.com;    # setup your domain here

    gzip            on;
    gzip_types      text/plain application/xml text/css application/javascript;
    gzip_min_length 1000;

    location / {
        expires $expires;

        proxy_redirect                      off;
        proxy_set_header Host               $host;
        proxy_set_header X-Real-IP          $remote_addr;
        proxy_set_header X-Forwarded-For    $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto  $scheme;
        proxy_read_timeout          1m;
        proxy_connect_timeout       1m;
        proxy_pass                          http://127.0.0.1:4873;
    }
  }
```

预览如下
![WechatIMG23](https://github.com/kukuyuhai/walker/blob/master/source/images/WechatIMG23.png)

### 如何使用

### npm (5.x, 6.x)[#](https://verdaccio.org/zh-CN/docs/cli-registry#npm-5x-6x)

To set the registry in the `.npmrc` file use the following:

```bash
npm set registry http://localhost:4873/ #替换成nginx配置的servername或者ip
```

```bash
npm install --registry http://localhost:4873 #替换成nginx配置的servername或者ip
```

在你自己的`.npmrc`文件中，自定义一个`registry`字段

.npmrc

```bash
registry=http://localhost:4873
```


> Since `npm@5.x` [ignores the `resolve` field in defined in the lock files](https://medium.com/verdaccio/verdaccio-and-deterministic-lock-files-5339d82d611e), while `pnpm@4.x` and `yarn@1.x` does the opposite.

或者在你的`package.json` 添加 `publishConfig`

```json
{  "publishConfig": {    "registry": "http://localhost:4873"  }}
```

> 通过使用`publishConfig`，前两个选项将被忽略，仅当您希望确保包不会在其他任何地方发布时才使用此选项。

