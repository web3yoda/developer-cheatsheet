---
weight: 121
title: rpc
---

## rpc

> 通过 curl 命令call rpc

```shell
# 查看最新高度
curl https://rpc.ankr.com/eth -s -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' | jq
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x1085fa8"
}

# 查看eth和goerli的 chainId
curl https://rpc.ankr.com/eth -s -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"eth_chainId","params":[],"id":1}'  | jq -r .result | xargs printf '%d\n'
1

curl https://rpc.ankr.com/eth_goerli -s -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"eth_chainId","params":[],"id":1}'  | jq -r .result | xargs printf '%d\n'
5

# 查看gasPrice
curl https://rpc.ankr.com/eth -s -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"eth_gasPrice","params":[],"id":1}'  | jq -r .result | xargs printf '%d\n' 
35690022251

# 查看finalized safe latest 三种状态高度
for i in finalized safe latest; do \
    curl -s https://rpc.ankr.com/eth -H "Content-Type: application/json" \
    --data '{"method":"eth_getBlockByNumber","params":["'$i'",false],"id":1,"jsonrpc":"2.0"}' \
    | jq -r .result.number | xargs printf '%d\n' 
done
17325938
17325970
17326018

# 
```