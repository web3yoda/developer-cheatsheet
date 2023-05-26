---
weight: 111
title: cast
---

## cast

cast是一个跟链rpc交互的工具，可以十分方便查询链上的各种数据，以及发送交易，call合约等

通过 `cast -h` 查看帮助操作， 几乎所有命令支持缩写，比如 `cast block-number` 可以写为 `cast bn`， 下面例子均使用缩写，请结合`cast -h`帮助理解

> 首先起一个 anvil 节点用来做实验
```shell
anvil

```

> cast相关环境变量

```shell

# 指定 rpc endpoint, 等价于 --rpc-url , 默认使用 http://127.0.0.1:8545
cast cid --rpc-url https://rpc.ankr.com/eth_goerli
5

export ETH_RPC_URL=https://rpc.ankr.com/eth_goerli
cast cid
5

```

> 查询链信息

```shell
# 查询 chain-id
cast cid 
31337

cast cid --rpc-url https://rpc.ankr.com/eth_goerli
5

# 查看 rpc server的 eth client版本
cast client
anvil/v0.1.0

cast client --rpc-url https://rpc.ankr.com/eth_goerli
erigon/2.43.0/linux-amd64/go1.19.3

# 查看当前最新高度
cast bn
1

cast bn --rpc-url https://rpc.ankr.com/eth_goerli
8000001

# 查看当前的gasprice
cast g
2000000000

cast g --rpc-url https://rpc.ankr.com/eth_goerli
32145765
```

> 查看块数据
```shell
# 从这里开始将 ETH_RPC_URL设置为 https://rpc.ankr.com/eth_goerli
export ETH_RPC_URL=https://rpc.ankr.com/eth_goerli

# 查看指定块数据
cast bl 8000000

# 查看最高块数据
cast bl latest

# 查看指定块hash的块数据
cast bl 0x2ae83825ac6b2a2b2509da8617cf31072a5628e9a818f177316f4f4bcdfafd06

# 结合jq使用查看块数据的某个字段
cast bl 8000000 -j | jq -r .hash
0x2ae83825ac6b2a2b2509da8617cf31072a5628e9a818f177316f4f4bcdfafd06

cast bl 0x2ae83825ac6b2a2b2509da8617cf31072a5628e9a818f177316f4f4bcdfafd06 -j | jq .number
8000000

# 查看块交易列表
cast bl 8000000 -j | jq -r .transactions

# 查看块stateroot
cast bl 8000000 -j | jq -r .stateRoot

# 查看 gasLimit 与 gasUsed 评估块被填满程度
cast bl 8000000 -j | jq -r .gasUsed  | xargs cast 2d
cast bl 8000000 -j | jq -r .gasLimit | xargs cast 2d

# 查看extraData 看看block builder都留言了啥内容
cast bl 8000000 -j  | jq -r .extraData | xargs cast 2as
Made on the moon by Blocknative

```

> 查看交易数据

```shell

```

> call合约查询数据(只读)

```shell

```

> 发送转账交易

```shell

```

> 发送合约交易

```shell

```

> 数据转换

```shell

```

> 钱包操作

```shell



```