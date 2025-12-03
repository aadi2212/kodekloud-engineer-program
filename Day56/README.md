# Deploy Nginx Webserver on Kubernetes Cluster
### Task: Highly Available Nginx Deployment with NodePort Service

The Nautilus DevOps team is deploying a static website on Kubernetes.  
To ensure high availability and scalability, the team will use a **Deployment** with 3 replicas and expose it externally using a **NodePort Service**.

---

## 📌 Task Requirements

1. Create a Deployment named **nginx-deployment**
2. Use image **nginx:latest**
3. Container name must be **nginx-container**
4. Number of replicas: **3**
5. Create a Service named **nginx-service**
6. Service type MUST be **NodePort**
7. The nodePort should be **30011**

---

## 🚀 Step 1: Create the Deployment YAML

Create a file named **nginx-deployment.yaml**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
          ports:
            - containerPort: 80

Apply the deployment:
kubectl apply -f nginx-deployment.yaml


Verify:
kubectl get deployments
kubectl get pods



🚀 Step 2: Create the NodePort Service YAML
Create a file named nginx-service.yaml:

apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30011

Apply the service:
kubectl apply -f nginx-service.yaml


Verify:
kubectl get svc nginx-service


🚀 Step 3: Verification

| Checkpoint         | Command                   | Expected Result                 |
| ------------------ | ------------------------- | ------------------------------- |
| Deployment created | `kubectl get deployments` | nginx-deployment (3/3 READY)    |
| Pods running       | `kubectl get pods`        | 3 Pods in Running state         |
| Service created    | `kubectl get svc`         | nginx-service on NodePort 30011 |
| App accessible     | Browser                   | Default **Nginx Welcome Page**  |


Access NGINX in browser:
http://<node-ip>:30011


📘 Concepts Learned
Deployment ensures declarative updates & scaling

ReplicaSets maintain desired number of Pods

NodePort Service exposes application externally

Labels & Selectors link Deployments and Services


⚡ Common Issues & Fixes
❌ Error
Service in version "v1" cannot be handled as a Service:
strict decoding error: unknown field "spec.ports[0].NodePort"


✅ Cause
Incorrect field name → used NodePort instead of nodePort.


🧩 Fix
nodePort: 30011


Reapply:
kubectl apply -f nginx-service.yaml


Verification:
kubectl get svc nginx-service


Expected:
80:30011/TCP


🎉 Your Nginx Deployment and Service are now successfully configured!





