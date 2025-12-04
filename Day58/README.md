# Deploy Grafana on Kubernetes Cluster  
### Grafana Deployment + NodePort Exposure (Port 32000)

This guide walks through deploying **Grafana** on a Kubernetes cluster using a Deployment and exposing it via a NodePort service on port **32000**.

---

## 📌 Requirements

- **Deployment Name:** `grafana-deployment-devops`
- **Image:** `grafana/grafana:latest` (any Grafana image is allowed)
- **Service Type:** NodePort
- **NodePort:** `32000`
- **Goal:** Successfully access Grafana login page in browser
- **kubectl:** Already configured on jump_host

---

## 🚀 Step 1: Create the Grafana Deployment

### **grafana-deployment.yml**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-devops
  labels:
    app: grafana
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
        - name: grafana-container
          image: grafana/grafana:latest
          ports:
            - containerPort: 3000

Apply Deployment:
kubectl apply -f grafana-deployment.yml


Verify Deployment:
kubectl get deployments
kubectl get pods


🌐 Step 2: Create the NodePort Service
grafana-service.yml

apiVersion: v1
kind: Service
metadata:
  name: grafana-service
spec:
  type: NodePort
  selector:
    app: grafana
  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 32000

Apply Service:
kubectl apply -f grafana-service.yml


Verify Service:
kubectl get svc


Expected output:
grafana-service   NodePort   <Cluster-IP>   <none>   3000:32000/TCP   AGE


🌍 Step 3: Access Grafana
Get Node IP:
kubectl get nodes -o wide


Open in browser:
http://<NODE-IP>:32000

You should see the Grafana Login Page 🎉


📄 Optional: Check Logs
kubectl logs -f <grafana-pod-name>


Expected in logs:
HTTP Server Listen: http://0.0.0.0:3000


✅ Verification Checklist

| Requirement                                              | Status |
| -------------------------------------------------------- | ------ |
| Grafana Deployment created (`grafana-deployment-devops`) | ✔️     |
| Pod running successfully                                 | ✔️     |
| NodePort service exposed on `32000`                      | ✔️     |
| Able to access Grafana login page                        | ✔️     |


📘 Summary
Grafana was successfully deployed on the Kubernetes cluster using a Deployment and exposed externally via NodePort 32000. Accessing the Grafana login page confirms that the application is running correctly and reachable.

