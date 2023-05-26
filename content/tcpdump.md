---
weight: 70
title: tcpdump
---

# tcpdump

> 抓http header

```bash

# 抓所有去往或者来自10.10.10.1:8080的http包的header
tcpdump -i any -A -s 10240 'tcp port 8080 and host 10.10.10.1 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)' \
    | egrep --line-buffered "^........(GET |HTTP\/|POST |HEAD )|^[A-Za-z0-9-]+: " \
    | sed -r 's/^........(GET |HTTP\/|POST |HEAD )/\n\1/g'

```


**参数解释**

参考 `man tcpdump`

`tcpdump`
| 参数 | 描述 |
| --- | ---- |
| -i | i指nterface 指定网络接口, `-i any` 表示所有端口 |
| -A | A指ASCII, 用ASCII打印每个包，抓取web请求非常方便 |
| -s | s 指snapshot length，`-s 10240` 指限制为10240 bytes，详细见man |

<aside class="notice">
注意：在不同OS实现、不同工具版本 参数略有不同，实际使用参考man手册
</aside>