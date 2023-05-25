---
weight: 31
title: aws ecr
---

## aws ecr

**aws ecr 常用子命令**
```shell

# 查看当前role
export AWS_PROFILE=PROFILE_NAME
aws ecr get-login-password \
    | docker login --username AWS --password-stdin 111111111111.dkr.ecr.ap-southeast-1.amazonaws.com

```