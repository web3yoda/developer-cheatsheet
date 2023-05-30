---
weight: 31
title: aws acm
---

## aws acm

**aws acm 常用子命令**
```shell

# describe certificates
for c in $(aws acm list-certificates \
          --query 'CertificateSummaryList[].CertificateArn' --output text)
do aws acm describe-certificate --certificate-arn $c \
    --query 'Certificate.{CertificateArn:CertificateArn, DomainName:DomainName,SubjectAlternativeNames:SubjectAlternativeNames, Status:Status,NotAfter:NotAfter}' | jq
done

```