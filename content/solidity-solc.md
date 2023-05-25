---
weight: 160
title: solc
---

## solc

> solc 常用命令选项

```shell

# 编译一个contract的abi, bin, opcode , 方法签名 等信息
solc src/MyToken.sol --abi --bin --opcode --hashes -o out

# 编译的时候map好lib路径
# import { ERC20 } from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
# contract MyToken is ERC20  {}
solc @openzeppelin=lib/openzeppelin-contracts src/MyToken.sol --abi --bin -o out
Compiler run successful. Artifact(s) can be found in directory "out".

```