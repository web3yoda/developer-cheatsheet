---
weight: 31
title: aws ec2
---

## aws ec2

**aws ec2 常用子命令**

> 使用 --filters 查询ec2

```shell

# find ec2 with --filters
aws ec2 describe-instances \
    --filters Name=instance-type,Values=t2.micro,t3.micro 
              Name=availability-zone,Values=us-east-2c                                        

# find ec2 with --filters by tag
aws ec2 describe-instances \
    --filters Name=tag:Owner,Values=my-team

# find ec2 with --filters by tag
aws ec2 describe-instances \
    --filters "Name=tag:Owner,Values=my-team"

```

> 结合 --filter 和 --query 只输出有用的内容

```shell


# get only useful information of ec2 instances wich both --filters and --query
aws ec2 describe-instances \
    --filters "Name=tag:Owner,Values=my-team" \
    --query 'Reservations[*].Instances[*].{Instance:InstanceId, Name:Tags[?Key==`Name`]|[0].Value}'

# --output table makes outputs more human readable.
aws ec2 describe-instances \
    --filters Name=tag-key,Values=Name \
    --query 'Reservations[*].Instances[*].{Instance:InstanceId,AZ:Placement.AvailabilityZone,Name:Tags[?Key==`Name`]|[0].Value}' \
    --output table

-------------------------------------------------------------
|                     DescribeInstances                     |
+--------------+-----------------------+--------------------+
|      AZ      |       Instance        |        Name        |
+--------------+-----------------------+--------------------+
|  us-east-2b  |  i-057750d42936e468a  |  my-prod-server    |
|  us-east-2a  |  i-001efd250faaa6ffa  |  test-server-1     |
|  us-east-2a  |  i-027552a73f021f3bd  |  test-server-2     |
+--------------+-----------------------+--------------------+

```