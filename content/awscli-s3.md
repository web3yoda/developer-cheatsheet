---
weight: 31
title: aws s3
---

## aws s3

**aws s3 常用子命令**

> 重命名一个子目录
```shell

# list bucket
aws s3 ls
                                                                                           
# rename folder
aws s3 --recursive mv s3://your-bucket/path/to/foo s3://your-bucket/path/to/bar


```