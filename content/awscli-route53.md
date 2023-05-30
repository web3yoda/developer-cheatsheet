---
weight: 31
title: aws route53
---

## aws route53

**aws route53 常用子命令**
> zone managment
```shell

# list zones
aws route53 list-hosted-zones | jq
# create zone
aws route53 create-hosted-zone --name example.com --caller-reference 2014-04-01-18:47 | jq
# get zone id by zone name
aws route53 list-hosted-zones \
  --query 'HostedZones[?Name==`example.com.`] | [0].Id' 
  --output text

```

> 资源集
```shell
# list record sets by zone name
aws route53 list-resource-record-sets \
  --hosted-zone-id $(aws route53 list-hosted-zones \
                      --query 'HostedZones[?Name==`example.com.`] | [0].Id' \
                      --output text) \
  | jq
# list CNAME record sets by zone name 
aws route53 list-resource-record-sets \
  --hosted-zone-id $(aws route53 list-hosted-zones \
                      --query 'HostedZones[?Name==`example.com.`] | [0].Id' \
                      --output text) \
  --query 'ResourceRecordSets[?Type==`CNAME`] \ 
  | jq
# create record set


```