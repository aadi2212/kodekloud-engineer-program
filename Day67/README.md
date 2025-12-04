# Deploy Guestbook Application on Kubernetes

## 📝 Task Overview
Deploy the Guestbook application on a Kubernetes cluster using a 3-tier architecture consisting of:
- **Redis Master**
- **Redis Slave**
- **Frontend PHP Application**

Kubectl is already configured on the jump host.

---

## 📦 1️⃣ Redis Master

### **Deployment**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-master
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis-master
  template:
    metadata:
      labels:
        app: redis-master
    spec:
      containers:
        - name: master-redis-devops
          image: redis
          resources:
            requests:
              cpu: "100m"
              memory: "100Mi"
          ports:
            - containerPort: 6379


Service:

apiVersion: v1
kind: Service
metadata:
  name: redis-master
spec:
  selector:
    app: redis-master
  ports:
    - port: 6379
      targetPort: 6379


📚 2️⃣ Redis Slave
Deployment

apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-slave
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis-slave
  template:
    metadata:
      labels:
        app: redis-slave
    spec:
      containers:
        - name: slave-redis-devops
          image: gcr.io/google-samples/gb-redisslave:v3
          resources:
            requests:
              cpu: "100m"
              memory: "100Mi"
          env:
            - name: GET_HOSTS_FROM
              value: dns
          ports:
            - containerPort: 6379


Service

apiVersion: v1
kind: Service
metadata:
  name: redis-slave
spec:
  selector:
    app: redis-slave
  ports:
    - port: 6379
      targetPort: 6379


🎨 3️⃣ Frontend
Deployment

apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
        - name: php-redis-devops
          image: gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff
          ports:
            - containerPort: 80
          resources:
            requests:
              cpu: "100m"
              memory: "100Mi"
          env:
            - name: GET_HOSTS_FROM
              value: dns


Service (NodePort)

apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: NodePort
  selector:
    app: frontend
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30009


🚀 4️⃣ Deployment Steps

Apply all files:

kubectl apply -f redis-master-deployment.yaml
kubectl apply -f redis-master-service.yaml
kubectl apply -f redis-slave-deployment.yaml
kubectl apply -f redis-slave-service.yaml
kubectl apply -f frontend-deployment.yaml
kubectl apply -f frontend-service.yaml


Check created resources:
kubectl get pods
kubectl get svc


Access the frontend:
http://<node-ip>:30009


⚠️ 5️⃣ Issues & Resolutions

| Issue                                 | Description                                                                                                                      |
| ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Wrong selector in frontend deployment | Selector mismatch caused service to not route traffic                                                                            |
| Fix                                   | Corrected `matchLabels` to match deployment pod labels                                                                           |
| Others                                | All resource requests configured (100m CPU, 100Mi Memory), environment variable `GET_HOSTS_FROM=dns` added for service discovery |



🎯 6️⃣ Final Outcome

✔ Redis Master deployed successfully

✔ Redis Slaves (2 replicas) deployed and running

✔ Frontend (3 replicas) deployed and exposed via NodePort 30009

✔ Guestbook application fully functional and accessible



