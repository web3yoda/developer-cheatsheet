---
weight: 11
title: awk
---

## awk

> 间隔1s统计各个状态的TCP链接数

```shell
# macos
while true; do
    netstat -ntf inet | awk '/^tcp/{++s[$NF]}END{for(a in s){print a, s[a]}}'
    sleep 1
done

# linux
while true; do 
    netstat -n | awk '/^tcp/{++s[$NF]}END{for(a in s){print a, s[a]}}'
    sleep 1
done

CLOSE_WAIT 45
ESTABLISHED 303
TIME_WAIT 874
SYN_SENT 1

```
> 大小写转换
```shell
echo "Web3Yoda" | awk '{print(tolower($0))}'

web3yoda


echo "Web3Yoda" | awk '{print(toupper($0))}'

WEB3YODA
```

**参数解释**


`awk`
| 参数 | 描述 |
| --- | ---- |
| -F | 指定字段分隔符 |

`netstat`
| 参数 | 描述 |
| --- | ---- |
| -n | 不对主机IP反向DNS解析主机名 |
| -t | 仅显示TCP |


<aside class="notice">
注意：在不同OS实现、不同工具版本 参数略有不同，实际使用参考man手册
</aside>