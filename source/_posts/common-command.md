---
title: 常用命令
date: 2021-6-8 09:20
tags: ssh node docker mysql
---

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

```
$ git reset --hard HEAD^ 回退到上个版本
$ git reset --hard HEAD~3 回退到前3次提交之前，以此类推，回退到n次提交之前
$ git reset --hard commit_id 退到/进到 指定commit的sha码
```
##### 强推到远程：

```
$ git push origin HEAD --force
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
