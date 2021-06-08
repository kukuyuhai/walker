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
