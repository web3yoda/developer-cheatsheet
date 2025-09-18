---
weight: 14
title: function
---

## function

**有用的函数或者代码段**

* 只对单个命令设置 http_proxy https_proxy all_proxy no_proxy 代理环境变量，避免了忘记set，或者set了以后忘记 unset

> 只对单个命令设置 _proxy 代理环境变量
```shell
# 可能需要sudo执行,  pw = proxy wrapper
cat <<EOF > /tmp/pw
#!/bin/zsh
export http_proxy=http://127.0.0.1:1080 https_proxy=http://127.0.0.1:1080 all_proxy=socks5://127.0.0.1:1087 no_proxy=10.0.0.0/8,127.0.0.1,localhost
"\$@"
unset http_proxy https_proxy all_proxy no_proxy
EOF

# mv to PATH and add x permission
sudo mv /tmp/pw /usr/local/bin/pw && chmod +x /usr/local/bin/pw

# 执行需要代理的命令的时候，前面加上 pw
pw git clone https://github.com/web3yoda/example.git
pw brew install
pw npm install

```

* 只对单个命令设置 http_proxy https_proxy all_proxy no_proxy 代理环境变量，避免了忘记set，或者set了以后忘记 unset

> 有时候写bash脚本需要做到跨平台，需要判断当前os是那种

```shell

if [[ "$OSTYPE" == "linux-gnu"* ]]; then
        # ...
elif [[ "$OSTYPE" == "darwin"* ]]; then
        # Mac OSX
elif [[ "$OSTYPE" == "cygwin" ]]; then
        # cygwin for Windows
elif [[ "$OSTYPE" == "msys" ]]; then
        # MinGW for Windows 
elif [[ "$OSTYPE" == "freebsd"* ]]; then
        # freebsd
else
        # Unknown.
fi

```
