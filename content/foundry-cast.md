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
# goerli rpc 设置为默认rpc
export ETH_RPC_URL=https://rpc.ankr.com/eth_goerli

# 列出 块号8000000 的所有交易
cast bl 8000000 -j  | jq -r .transactions
[
  "0x416fa2558422a5838140557b02ebc21c42444f503df8a2ba5020805ee420df68",
  "0xa4f0d3ef0c47315158b08810a20d07f32ab13cf3b3ef33e52426e85a6355b678",
  "0x5f61721fc92b1175cf2f41a391688aef1b9d6acd4ed07a3591d329d82f95418d",
  ...omit
]

# 查看上面的第一笔交易内容
cast tx -j 0x416fa2558422a5838140557b02ebc21c42444f503df8a2ba5020805ee420df68 | jq
{
  "hash": "0x416fa2558422a5838140557b02ebc21c42444f503df8a2ba5020805ee420df68",
  "nonce": "0x39",
  "blockHash": "0x2ae83825ac6b2a2b2509da8617cf31072a5628e9a818f177316f4f4bcdfafd06",
  "blockNumber": "0x7a1200",
  "transactionIndex": "0x0",
  "from": "0x1f452ea54d4d934afadc300c61ca0a3b1bbde958",
  "to": "0x8efe26d6839108e831d3a37ca503ea4f136a8e73",
  "value": "0x0",
  "gasPrice": "0x54c32a27e4",
  "gas": "0xb640",
  "input": "0x39509351000000000000000000000000d179c5bed30cade4e62d53dd89240745fb4c0cc20000000000000000000000000000000000000000000000001bc16d674ec80000",
  "v": "0x1",
  "r": "0xe6c51797979239ec7408fdc065082ab2757c29554513f536b32aecf3953004bb",
  "s": "0x25f3e739d9b459aea524b201fc3c3947f2168b35638f047929b433a242eb30f8",
  "type": "0x2",
  "accessList": [],
  "maxPriorityFeePerGas": "0x54c32a27e4",
  "maxFeePerGas": "0x54c32a27e4",
  "chainId": "0x5"
}

# cast 4b解析一下上面输出里input字段的method selector对应的signature这是笔什么交易
cast 4b 0x39509351
increaseAllowance(address,uint256)

# 也到etherscan浏览器里面看一下
open https://goerli.etherscan.io/tx/0x416fa2558422a5838140557b02ebc21c42444f503df8a2ba5020805ee420df68

```

> call合约查询数据(只读)，用google搜索bitdao erc20找到主网bit合约地址 0x1A4b46696b2bB4794Eb3D4c26f1c55F9170fa4C5

```shell

# cast命令call合约的格式如下,  METHOD_SIGNATURE即方法签名 包含方法名、入参类型列表、返回类型列表，如sum(int, int)(int)
cast call ${CONTRACT_ADDRESS} ${METHOD_SIGNATURE} ${ARG1} ${ARG2} ...

# eth mainnet rpc 设置为默认rpc
export ETH_RPC_URL=https://rpc.ankr.com/eth

# 查看最大发行量, 用cast fw将结果单位wei转换为bit，也就是除以10^18次方
cast call 0x1A4b46696b2bB4794Eb3D4c26f1c55F9170fa4C5 "totalSupply()(uint256)" | cast fw

# 查看token的symbol
cast call 0x1A4b46696b2bB4794Eb3D4c26f1c55F9170fa4C5 "symbol()(string)"
BIT

# 查看黑洞账号(燃烧账号)的bit余额(注意跟cast b的native token余额区分开)
cast call 0x1A4b46696b2bB4794Eb3D4c26f1c55F9170fa4C5 "balanceOf(address)(uint256)" 0x000000000000000000000000000000000000dEaD | cast fw
780683232.168659763560000000

# 查看BitDAO国库账号的bit余额(注意跟cast b的native token余额区分开)
cast call 0x1A4b46696b2bB4794Eb3D4c26f1c55F9170fa4C5 "balanceOf(address)(uint256)" 0x78605Df79524164911C144801f41e9811B7DB73D | cast fw
6019959375.550189111112685594

# 以上都是erc20合约的标准方法名
```

> 发送普通转账交易

```shell
# 转账要花钱，所以用本地节点，启动anvil, 会显示
anvil --accounts 2
Available Accounts
==================

(0) 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 (10000 ETH)
(1) 0x70997970c51812dc3a010c7d01b50e0d17dc79c8 (10000 ETH)

Private Keys
==================

(0) 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
(1) 0x59c6995e998f97a5a0044966f0945389dc9e86dae88c7a8412f4603b6b78690d

# unset ETH_RPC_URL 确保rpc是默认 http://localhost:8545
unset ETH_RPC_URL

# cast b查看确认一下余额
cast b 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 | cast fw
10000.000000000000000000
cast b 0x70997970c51812dc3a010c7d01b50e0d17dc79c8 | cast fw
10000.000000000000000000

# 0号测试账号往1好账号转1000个ETH
cast send --from 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 --private-key 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80 --value $(bc<<<1000*10^18) 0x70997970c51812dc3a010c7d01b50e0d17dc79c8

blockHash               0x9cf5b57e15730866219f8bd63c557acbce673e0bf5c942fe5d480054d2e33110
blockNumber             1
contractAddress
cumulativeGasUsed       21000
effectiveGasPrice       3875175000
gasUsed                 21000
logs                    []
logsBloom               0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
root
status                  1
transactionHash         0xa61eaa5d124567eddd119b8ffc157c15749c228d1e9d77808745cfd19dbecb3b
transactionIndex        0
type

# 再次观察余额, 账号0花了1000加一点点手续费，账号1收到1000
cast b 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 | cast fw
8999.999916000000000000
cast b 0x70997970c51812dc3a010c7d01b50e0d17dc79c8 | cast fw
11000.000000000000000000

# 查看高度
cast bn 
1
# 查看块内容
cast bl 1
# 参考前面的命令查看交易内容
cast tx ...

```

> 发送合约调用交易

```shell
# 重新启动anvil
anvil --accounts 2
# 首先部署erc20合约 参考 https://github.com/shidaxi/web3-dev-example
git clone https://github.com/shidaxi/web3-dev-example.git
yarn install
npx hardhat run scripts/deployMySimpleToken.ts --network localhost
Deployed 0x5FbDB2315678afecb367f032d93F642f64180aa3

# 查看 totalSupply symbol owner(也就是0号测试账号), 初始供应量1000000
cast call 0x5FbDB2315678afecb367f032d93F642f64180aa3 "totalSupply()(uint256)" | cast fw
1000000.000000000000000000

cast call 0x5FbDB2315678afecb367f032d93F642f64180aa3 "symbol()(string)" 
MST

cast call 0x5FbDB2315678afecb367f032d93F642f64180aa3 "owner()(address)"
0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266

# 0号测试账号是owner有铸币权，为1号账号铸币10个MST， mint 铸币，也就是产生币的过程
cast send --from 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 --private-key 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80  0x5FbDB2315678afecb367f032d93F642f64180aa3 "mint(address,uint256)" 0x70997970c51812dc3a010c7d01b50e0d17dc79c8 $(bc<<<10^19)

# 再看totalSupply以及0号1号测试账号的余额变化
cast call 0x5FbDB2315678afecb367f032d93F642f64180aa3 "totalSupply()(uint256)" | cast fw
1000010.000000000000000000

cast call 0x5FbDB2315678afecb367f032d93F642f64180aa3 "balanceOf(address)(uint256)" 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 | cast fw
1000000.000000000000000000

cast call 0x5FbDB2315678afecb367f032d93F642f64180aa3 "balanceOf(address)(uint256)" 0x70997970c51812dc3a010c7d01b50e0d17dc79c8 | cast fw
10.000000000000000000

# erc20 转账, 1号给0号账号转5个 然后查看余额
cast send --from 0x70997970c51812dc3a010c7d01b50e0d17dc79c8 --private-key 0x59c6995e998f97a5a0044966f0945389dc9e86dae88c7a8412f4603b6b78690d 0x5FbDB2315678afecb367f032d93F642f64180aa3 "transfer(address, uint256)" 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 $(bc<<<5*10^18)

cast call 0x5FbDB2315678afecb367f032d93F642f64180aa3 "balanceOf(address)(uint256)" 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 | cast fw
1000005.000000000000000000

cast call 0x5FbDB2315678afecb367f032d93F642f64180aa3 "balanceOf(address)(uint256)" 0x70997970c51812dc3a010c7d01b50e0d17dc79c8 | cast fw
5.000000000000000000

```

> 数据转换

```shell

# 最常用hex转decimal
echo 0xa | xargs cast 2d


```

> 钱包操作

```shell
# 随机生成一个测试私钥
cast w n
Successfully created new keypair.
Address: 0xa60A3fF7D6E77306bc29b344cB887E6af351AA6B
Private Key: 0x1d430a0e2e062bf342fd25816f4765f0b245999cda731fc73e0e1c4c96e21b0d

# 私钥推导地址
cast w a 0x1d430a0e2e062bf342fd25816f4765f0b245999cda731fc73e0e1c4c96e21b0d
0xa60A3fF7D6E77306bc29b344cB887E6af351AA6B

```

> 数据编码解码

```shell
# 根据方法signature计算selector
cast k 'transfer(address,uint256)'
0xa9059cbb2ab09eb219583f4a59a5d0623ade346d962bcd4e46b11da047c9049b
# 4byte解码出方法签名
cast 4b 0xa9059cbb
transfer(address,uint256)

```

> 理解nonce 以及合约地址如何生成。nounce是账号交易计数器，保存在state里面，目的防止双重支付

```shell
# 启动一个新的anvil
anvil -a 2

# 查看账号0的nounce
cast n 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266
0

# 部署合约(用的hardhat 0号账号)
npx hardhat run scripts/deployMySimpleToken.ts --network localhost
Deployed 0x5FbDB2315678afecb367f032d93F642f64180aa3

# 查看nounce变为1
cast n 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266                 
1

# 相同合约再部署一次，合约是一个新地址
npx hardhat run scripts/deployMySimpleToken.ts --network localhost
Deployed 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512

# 查看nounce变为2
cast n 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266                 
2

# 合约地址仅和address和nounce有关，因此可以提前计算出来
cast ca 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266
Computed Address: 0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0

# 再部署一次看看是否为0x9f
npx hardhat run scripts/deployMySimpleToken.ts --network localhost
Deployed 0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0

```

> 查看 eip1967 proxy的实现 和 admin
```shell
ProxyFooAddress=0x
# cast keccak eip1967.proxy.implementation - 1
_IMPLEMENTATION_SLOT=0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc
# get implementation of a proxy
cast storage ${ProxyFooAddress} ${_IMPLEMENTATION_SLOT}

# cast keccak eip1967.proxy.admin - 1
export _ADMIN_SLOT=0xb53127684a568b3173ae13b9f8a6016e243e63b6e8ee1178d6a717850b5d6103
# get admin of a proxy
cast storage ${ProxyFooAddress} ${_ADMIN_SLOT}

```

> abi encode和decode
```shell

# 方法签名 获取 selector

 cast sig 'deposit(address,uint256,string)'

0xbfe07da6

# selector 查询 方法签名
cast 4b 0xbfe07da6

deposit(address,uint256,string)

# abi encode

cast abi-encode "deposit(address,uint256,string)" 0x1111111111111111111111111111111111111111 1000 "hello"

0x000000000000000000000000111111111111111111111111111111111111111100000000000000000000000000000000000000000000000000000000000003e80000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000000568656c6c6f000000000000000000000000000000000000000000000000000000

# 手动解析 abi encoded data, 每32字节换一行

echo 000000000000000000000000111111111111111111111111111111111111111100000000000000000000000000000000000000000000000000000000000003e80000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000000568656c6c6f000000000000000000000000000000000000000000000000000000 | fold -w64

0000000000000000000000001111111111111111111111111111111111111111
00000000000000000000000000000000000000000000000000000000000003e8
0000000000000000000000000000000000000000000000000000000000000060
0000000000000000000000000000000000000000000000000000000000000005
68656c6c6f000000000000000000000000000000000000000000000000000000

# 加上偏移标注
echo 000000000000000000000000111111111111111111111111111111111111111100000000000000000000000000000000000000000000000000000000000003e80000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000000568656c6c6f000000000000000000000000000000000000000000000000000000 | fold -w64 | nl -v0 -nrz -w2 -s': '

00: 0000000000000000000000001111111111111111111111111111111111111111
01: 00000000000000000000000000000000000000000000000000000000000003e8
02: 0000000000000000000000000000000000000000000000000000000000000060
03: 0000000000000000000000000000000000000000000000000000000000000005
04: 68656c6c6f000000000000000000000000000000000000000000000000000000


```