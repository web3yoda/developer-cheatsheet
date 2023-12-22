---
weight: 21
title: kubectl
---

## kubectl

熟练掌握好kubectl的各种用法，可以让你在跟k8s打交道工作中事半功倍，以免花大量时间在google搜索的结果上

> 工欲善其事必先利其器，首先掌握几个子命令用法

```shell
# 查看所有resource类型,包括crd
kubectl api-resources

# sort by name
kubectl api-resources --sort-by=name

# select api group
kubectl api-resources --api-group=networking.k8s.io

# 查看你想用的yaml类型的所有可用字段
kubectl explain deployments --recursive | less

# 尤其是CRD资源, 有些比较小众，google搜不到例子，或者api版本变更导致搜到的例子都是错的，可以直接查看
kubectl explain certificates --recursive | less
```

> jsonpath 的使用, 如果get -o yaml 输出内容太多，可以用jsonpath选择单个字段输出
> 详细参考 https://github.com/json-path/JsonPath
```shell
# 只查看一个指定jsonpath
kubectl get configmap nginx -o jsonpath='{.data.nginx\.conf}'

# 不太了解整个数据结构 写不出jsonpath？
kubectl get pods -o json | jq -c 'paths|join(".")' | less

```

> 查看pod资源实际使用量
```shell
# cpu top 10
kubectl -ndefault top pod --sort-by=cpu --no-headers | head

# mem top 10
kubectl -ndefault top pod --sort-by=memory --no-headers | head

# --containers 显示每个容器
kubectl -ndefault top pod --containers --sort-by=cpu --no-headers | head
```

> 查看 node资源实际使用量

```shell

# cpu top 10
kubectl top node --sort-by=cpu --no-headers | head

# mem top 10
kubectl top node --sort-by=memory --no-headers | head

```

> 查看 event

```shell

kubectl get event -n default --field-selector involvedObject.name=nginx-0

```

> ad-hoc方式修改运行时的资源

```shell

# 修改replicas字段
kubectl scale --replicas=2 deploy/nginx

# 修改单个环境变量, 可以 kubectl set env -h 查看所有用法例子
kubectl set env sts/nginx NGINX_PORT=8080

# 查看一个pod注入的所有环境变量，注意这里虽然是set子命令但是 不会做改动
kubectl set env sts/nginx --list

```

> 临时运行一个 debugger pod 做操作，比如连数据库
```shell

# mysql
kubectl run mysql-client --rm -i --tty --image mysql:5.7 -- bash

# pgclient  
kubectl run pg-client --rm -i --tty --image postgres:13-alpine -- sh

# netshoot swiss-knife
kubectl run debugger --rm -i --tty --image nicolaka/netshoot:latest -- bash

```

> ad-hoc 方式创建一个资源
```shell
# 创建一个deployment，理论上只需要一个image即可
kubectl create deployment nginx --image=nginx

# 使用--from-literal创建一个secret
kubectl create secret generic nginx --from-literal=basic_password=Mzk5YWY1YzNjMWEy

# 使用--from-literal创建一个configmap
kubectl create configmap nginx --from-literal=basic_username=admin

```

> 用patch方式修改资源

```shell
# scale with kubctl patch
kubectl patch deploy nginx --patch '{"spec": {"replicas": 2}}'

# disable healthcheck if you want to troubleshoot healthcheck failure
# --type json, use json patch(RFC 6902), check official docs
kubectl patch deployment nginx --type json -p='[{"op": "remove", "path": "/spec/template/spec/containers/0/livenessProbe"}]'

# increase initialDelaySeconds to 3600
kubectl patch deploy nginx --type json \
  -p='[{"op": "replace", "path": "/spec/template/spec/containers/0/livenessProbe/initialDelaySeconds", "value": 3600}]'
kubectl patch deploy nginx --type json \
  -p='[{"op": "replace", "path": "/spec/template/spec/containers/0/readinessProbe/initialDelaySeconds", "value": 3600}]'

# change "command" to "sleep 1000" to troubleshoot entrypoint command error
kubectl patch deployment nginx --type json -p='[{"op": "replace", "path": "/spec/template/spec/containers/0/command", "value":["sleep", "1000"]}]'

# change image to troubleshoot software version issue
kubectl patch deployment nginx --type='json' -p='[{"op": "replace", "path": "/spec/template/spec/containers/0/image", "value":"nginx:1.21"}]'

# or with set image sub cmd
kubectl set image deployment/nginx nginx=nginx:1.21 

```

> 从标准输出apply一个yaml文件

```shell
kubectl apply -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: busybox-sleep
spec:
  containers:
  - name: busybox
    image: busybox:1.28
    args:
    - sleep
    - infinity
EOF
```

> 滚动方式重启一个deployment

会先起新pod，ready之后再删旧pod，比较安全

```shell
kubectl -n default rollout restart deploy nginx 
```

> 通过kubectl在本地和容器之间传输文件

```shell
# cp to /tmp
kubectl cp /tmp/hello.txt debugger-0:/tmp/hello.txt
# cp to current workDir
kubectl cp /tmp/hello.txt debugger-0:hello.txt

# check
kubectl exec -it debugger-0 -- sh -c " pwd && ls -lh && ls /tmp -lh && cat hello.txt "

kubectl cp debugger-0:/tmp/hello.txt /tmp/world.txt
tar: removing leading '/' from member names # this is fine
```