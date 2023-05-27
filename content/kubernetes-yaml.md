---
weight: 23
title: yaml
---

## yaml

**一些常用的yaml资源模板或者片段 复制过来改改直接用**

> deployment 模板，直接复制改改即可用

```yaml
# deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: default
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80 
```

> statefulset
```yaml
# statefulset
apiVersion: apps/v1
kind: Statefulset
metadata:
  name: nginx-statefulset
  namespace: default
  labels:
    app: nginx
spec:
  serviceName: nginx
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi     

```

> service
```yaml
# service
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    name: web
  selector:
    app: nginx   

```

> ingress
```yaml
# ingress
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations: {}
  name: nginx
  namespace: default
  labels:
    app: nginx
spec:
  rules:
  - host: www.example.com
    http:
      paths:
      - backend:
          service:
            name: nginx
            port:
              number: 80
        path: /
        pathType: ImplementationSpecific
```

> configmap
```yaml

# configmap
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx
  namespace: default
data:
  key1: value1
  nginx.conf: |
    events {
      worker_connections  1024;
    }
    worker_processes auto;
    error_log /dev/stdout notice;
    pid /var/run/nginx.pid;
    http {
      server {
        listen 80 default_server;
        server_name _;
        return 301 https://$host$request_uri;
      }
    }
    stream {
      server {
        listen 443;
        resolver 172.20.0.10;
        proxy_pass 127.0.0.1:443;
      }
    }           

```

> secret
```yaml
# secret
apiVersion: v1
kind: Secret
metadata:
  name: nginx
  namespace: default
type: Opaque
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm

```

> snippet livenessProbe and readinessProbe with http protocol
```yaml
# livenessProbe and readinessProbe with http protocol
spec:
  containers:
  - name: app
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
        httpHeaders:
        - name: Custom-Header
          value: Awesome
      initialDelaySeconds: 3
      periodSeconds: 3
    readinessProbe:
      httpGet:
        path: /healthz
        port: 8080
        httpHeaders:
        - name: Custom-Header
          value: Awesome
      initialDelaySeconds: 3
      periodSeconds: 3          

```
> snippet livenessProbe and readinessProbe with TCP protocol
```yaml
# livenessProbe and readinessProbe with TCP protocol
spec:
  containers:
  - name: app
    readinessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 10
    livenessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 15
      periodSeconds: 20        

```
> mount a configmap
```yaml

# mount a configmap
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: default
spec:
  containers:
    - name: nginx
      image: nginx:latest
      volumeMounts:
      - name: config-volume
        mountPath: /etc/nginx
        subPath:
  volumes:
    - name: config-volume
      configMap:
        name: nginx        

```
> env from configmap or secret
```yaml
# env from configmap or secret
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: default
spec:
  containers:
    - name: nginx
      image: nginx:latest
      env:
      - name: SECRET_USERNAME
        valueFrom:
          configMapKeyRef:
            name: nginx
            key: username
            optional: false
      - name: SECRET_PASSWORD
        valueFrom:
          secretKeyRef:
            name: nginx
            key: password
            optional: false
      envFrom:
      - configMapRef:
          name: nginx
      - secretRef:
          name: nginx              

```

> snippet，affinity，同一个服务的不同pod调度到不同的node上
```yaml
spec:
  affinity:
    podAntiAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchExpressions:
          - key: app
            operator: In
            values:
            - nginxsniproxy
        topologyKey: "kubernetes.io/hostname"
```

> clusterrole
```yaml
```

> clusterrole binding
```yaml
```

> 
```yaml
```

> 
```yaml
```

> 
```yaml
```
