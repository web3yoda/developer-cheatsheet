---
weight: 15
title: misc
---

## misc

**一些有用的杂七杂八的命令**

> 生成16位仅包含大小写字母+数字的密码(兼容macos和linux)

```shell
echo `date +%s`$RANDOM | sha256sum | base64 | head -c16; echo
NzY5YjgxMDc1OTFl

openssl rand -hex 32 | base64 | head -c32
Mzg4ZjcwZTAyYThkZTBkNjdjYmYwYTNk

```

> 一条命令下载tar.gz包并解压到指定目录 还可以去掉打包父目录, dockerfile里常用
```shell
curl -s https://gethstore.blob.core.windows.net/builds/geth-alltools-linux-amd64-1.11.6-ea9e62ca.tar.gz \
    | tar xzf - -C /usr/local/bin/ --strip-components=1
```

> 跑一个循环间隔1s检查一个服务是否启动成功

```shell

while true; do curl http://localhost:8080 >/dev/null && break; sleep 1; done

```

> parameter substitution https://tldp.org/LDP/abs/html/parameter-substitution.html

```shell

$ a='hello:world'

$ b=${a%:*}
$ echo "$b"
hello

$ a='hello:world:of:tomorrow'

$ echo "${a%:*}"
hello:world:of

$ echo "${a%%:*}"
hello

$ echo "${a#*:}"
world:of:tomorrow

$ echo "${a##*:}"
tomorrow

```