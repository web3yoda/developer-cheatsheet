---
weight: 81
title: homebrew
---

## homebrew

**brew 基本命令**

> 查找一个包
```shell

brew search cowsay
==> Formulae
cowsay ✔
```

> 当你记不清完整包名的时候可以用正则
```shell
pxyw brew search /^postg/  
==> Formulae
postgis                   postgresql@10             postgresql@12             postgresql@14 ✔           postgresql@9.4            postgrest ✔
```

> 查看一个包信息
```shell
brew info cowsay
==> cowsay: stable 3.04 (bottled)
Configurable talking characters in ASCII art
https://github.com/tnalpgge/rank-amateur-cowsay
/opt/homebrew/Cellar/cowsay/3.04_1 (63 files, 82.8KB) *
  Poured from bottle on 2021-11-15 at 20:51:11
From: https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/cowsay.rb
License: GPL-3.0
```

> 安装包的时候禁用自动更新
```shell
HOMEBREW_NO_AUTO_UPDATE=1 brew install cowsay

```

> 有些包有多个版本，比如node，将某个版本设置为系统默认版本

```shell
# 同时安装了node@14 node@16
pxyw brew search /^node/ 
==> Formulae
node ✔                    node-sass                 node@14 ✔                 node@18
node-build                node@10                   node@16 ✔                 node_exporter ✔

# 将当前版本设置为node@16
brew unlink node
brew link node@16
```