---
weight: 26
title: dockerfile
---

## Dockerfile

**一些有用的小技巧**

* 将entrypoint脚本集成进Dockerfile，这样仅用一个Dockerfile即可

> 将entrypoint脚本集成进Dockerfile

```shell

# syntax=docker/dockerfile:1
FROM busybox:latest
COPY --chmod=755 <<EOF /app/run.sh
#!/bin/sh
while true; do
  echo -ne "The time is now $(date +%T)\\r"
  sleep 1
done
EOF

ENTRYPOINT /app/run.sh

```