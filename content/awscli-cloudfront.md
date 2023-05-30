---
weight: 31
title: aws cloudfront
---

## aws cloudfront

**aws cloudfront 常用子命令**

> 列出所有 distribution
```shell


# list distributions with id, aliases, domian name and origin
aws cloudfront list-distributions \
  | jq  '.DistributionList.Items[] | {"Id": .Id, "Aliases": .Aliases.Items, "Domain": .DomainName, "Origin": .Origins.Items[0].DomainName}'
```

> 创建一个invalidation
```shell
# create an invalidation, clear cache
aws cloudfront create-invalidation --distribution-id E3NXXXXXXXXXXX --paths "/*"

```

