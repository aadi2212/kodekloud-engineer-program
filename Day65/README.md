# Deploy Redis on Kubernetes

## 📝 Task Overview
The Nautilus DevOps team wants to test Redis as an in-memory caching layer to improve application performance. Your task is to deploy Redis on a Kubernetes cluster with specific configurations.

---

## 📌 1️⃣ Create Redis ConfigMap

Create a ConfigMap named **my-redis-config** with `maxmemory 2mb`.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-redis-config
data:
  redis-config: |
    maxmemory 2mb

Apply:
kubectl apply -f redis-configmap.yml


📦 2️⃣ Create Redis Deployment

A deployment named redis-deployment with:

Image: redis:alpine

Container name: redis-container

Replicas: 1

CPU request: 1

EmptyDir volume → /redis-master-data

ConfigMap volume → /redis-master

Exposed port: 6379


apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
        - name: redis-container
          image: redis:alpine
          ports:
            - containerPort: 6379
          resources:
            requests:
              cpu: "1"
          volumeMounts:
            - name: data
              mountPath: /redis-master-data
            - name: redis-config
              mountPath: /redis-master
      volumes:
        - name: data
          emptyDir: {}
        - name: redis-config
          configMap:
            name: my-redis-config
            items:
              - key: redis-config
                path: redis.conf

Apply:
kubectl apply -f redis-deployment.yml


🔍 3️⃣ Verification
Check if Redis pod is up and running:
kubectl get pods


Expected output example:
NAME                                 READY   STATUS    RESTARTS   AGE
redis-deployment-7d4b8d7d5f-b5k6r    1/1     Running   0          1m


Describe pod for volume + config checks:
kubectl describe pod <pod-name>


📘 Notes
ConfigMap provides Redis memory configuration.

EmptyDir provides temporary data storage for Redis.

ConfigMap Volume mounts Redis config file under /redis-master.

CPU request ensures at least 1 CPU is allocated.

Port 6379 is the default Redis port.

