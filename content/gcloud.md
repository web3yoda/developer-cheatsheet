---
weight: 35
title: gcloud
---

# gcloud

> auth

```shell

CLOUDSDK_CORE_ACCOUNT=email1@domain1.com gcloud ...
CLOUDSDK_CORE_ACCOUNT=email2@domain2.com gcloud ...

gcloud config configurations create my-project1-config
gcloud config configurations activate my-project1-config
gcloud auth login  # or activate-service-account
gcloud config set project project1  # and any other configuration you need to do

gcloud config configurations create my-project2-config
gcloud config configurations activate my-project2-config
gcloud auth login  # or activate-service-account
gcloud config set project project2  # and any other configuration you need to do

CLOUDSDK_ACTIVE_CONFIG_NAME=my-project1-config gcloud ...
CLOUDSDK_ACTIVE_CONFIG_NAME=my-project2-config gcloud ...

```