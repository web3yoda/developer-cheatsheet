---
weight: 121
title: websocket
---

## websocket

通过websocat请求call eth rpc，查询数据

> 通过 curl 命令call rpc

```shell
# 查看最新高度
echo eth_blockNumber | websocat --jsonrpc -n1 wss://wss.hyperspace.node.glif.io/apigw/lotus/rpc/v1 \
    | jq
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x1085fa8"
}

# 查看 chainId
echo eth_chainId | websocat --jsonrpc -n1 wss://wss.hyperspace.node.glif.io/apigw/lotus/rpc/v1 \
    | jq -r .result \
    | xargs printf '%d\n'
3141
# 
```