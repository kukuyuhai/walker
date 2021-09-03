---
title: 常用命令
date: 2021-6-8 09:20
tags: ssh node docker mysql
---

```sh

locate  #查看相似文件
which nginx #在PATH变量指定的路径中,搜索某个系统命令的位置,并且返回第一个搜索结果。
systemctl 
```

### git

```sh
  //生成ssh秘钥对
  ssh-keygen -t rsa -C "mail@example.com"
  cd ~/.ssh
  ls
  vim id_rsa.pub
  //复制秘钥添加到github
```

> 打开 github -> settings -> SSH and GPG keys -> 添加公钥
> 即可使用 ssh 协议 pull and push

#### Git回滚代码到某个commit

##### 回退命令：

```sh
$ git reset --hard HEAD^ #回退到上个版本
$ git reset --hard HEAD~3 #回退到前3次提交之前，以此类推，回退到n次提交之前
$ git reset --hard commit_id #退到/进到 指定commit的sha码
```
##### 强推到远程：

```
$ git push origin HEAD --force
```

### nginx

##### 查看conf位置

```sh
root@yuhai:/etc/nginx/conf.d# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

```
`/etc/nginx/nginx.conf`即是配置文件位置，可通过include拓展config

```sh

nginx -s relaod #重启nginx服务

```

### mysql

##### Mysql 允许所有 IP 访问

```sh
$ mysql -u root -p
Enter password:

mysql> use mysql

#mysql> GRANT ALL ON *.* to root@'192.168.1.4' IDENTIFIED BY 'your-root-password';

mysql> GRANT ALL ON *.* to root@'%' IDENTIFIED BY 'root';

mysql> FLUSH PRIVILEGES;
```
