---
weight: 31
title: aws sts
---

## aws sts

**aws sts 常用子命令**

> 查看当前role
```shell

# 设置环境变量
export AWS_PROFILE=PROFILE_NAME
# 查看当前role
aws sts get-caller-identity

```

> 将assume role的临时ak sk token 注入环境变量
```shell
# assume role and set credentials to env variables, with jq
ROLE_ARN=arn:aws:iam::000000000000:role/your-role
eval $(aws sts assume-role \
        --role-arn ${ROLE_ARN} \
        --role-session-name sess-name \
        | jq -r '.Credentials | "export AWS_ACCESS_KEY_ID=\(.AccessKeyId) AWS_SECRET_ACCESS_KEY=\(.SecretAccessKey) AWS_SESSION_TOKEN=\(.SessionToken)"'
      )
# assume role and set credentials to env variables, with sed & awk
ROLE_ARN=arn:aws:iam::000000000000:role/your-role
eval $(aws sts assume-role \
        --role-arn ${ROLE_ARN} \
        --role-session-name sess-name \ 
        | egrep '(SecretAccessKey|SessionToken|AccessKeyId)' \
        | awk -F'"' '{print "export AWS"toupper(gensub(/([A-Z])/, "_\\1", "g",$2))"="$4}'
      )
```

