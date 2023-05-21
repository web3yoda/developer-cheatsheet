---
weight: 15
title: misc
---

## misc

**一些有用的杂七杂八的命令**

> 生成16位仅包含大小写字母+数字的密码(兼容macos和linux)

```shell
echo `date +%s`$RANDOM | sha256sum | base64 | head -c16; echo

NzY5YjgxMDc1OTFl
```

> 

