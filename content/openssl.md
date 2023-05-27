---
weight: 70
title: openssl
---

# openssl

## cert

## key

## hash

## s_connect

> 查看服务器证书

```shell

openssl s_client -showcerts -servername httpbin.org \
    -connect httpbin.org:443 </dev/null

```
