# Fix Python App Deployed on Kubernetes Cluster

## 📝 Task Overview
A Python Flask application deployed on the Kubernetes cluster was failing due to misconfiguration. The goal was to fix the Deployment and Service so that the app becomes accessible through a NodePort.

---

## 🎯 Objectives

- Fix the existing **python-deployment-xfusion**
- Ensure the pod runs successfully using a valid container image
- Expose the application using **NodePort 32345**
- Map `targetPort` to Flask’s default port (**5000**)
- Verify the application end-to-end

---

## 🛠️ 1️⃣ Corrected Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: python-deployment-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      app: python-app
  template:
    metadata:
      labels:
        app: python-app
    spec:
      containers:
        - name: python-container
          image: app:latest
          ports:
            - containerPort: 5000


✔ Key Fixes
1. Corrected the container image (app:latest)

2. Ensured deployment labels match service selectors

3. Exposed Flask’s port (5000) correctly

4. Kept replicas: 1 for testing


🌐 2️⃣ Corrected Service YAML (NodePort)

apiVersion: v1
kind: Service
metadata:
  name: python-service-xfusion
spec:
  type: NodePort
  selector:
    app: python-app
  ports:
    - port: 5000
      targetPort: 5000
      nodePort: 32345


✔ Key Fixes
Set service type to NodePort

Exposed Flask app externally using nodePort: 32345

Correctly mapped targetPort → 5000



🔧 3️⃣ Steps Performed to Fix the Issue

# 1. Delete faulty deployment
kubectl delete deployment python-deployment-xfusion

# 2. Apply corrected deployment
kubectl apply -f python-deployment.yml

# 3. Delete faulty service
kubectl delete service python-service-xfusion

# 4. Apply corrected service
kubectl apply -f python-service.yml


Verification
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>

kubectl get svc python-service-xfusion


Access the app:
http://<NodeIP>:32345


Expected output:
Hello World Pyvo 1!


🧠 4️⃣ Troubleshooting Summary

| Issue               | Cause                         | Fix                                   |
| ------------------- | ----------------------------- | ------------------------------------- |
| ImagePullBackOff    | Wrong image name              | Updated image to `app:latest`         |
| 502 Bad Gateway     | Flask bound to localhost only | Ensure Flask binds to `0.0.0.0:5000`  |
| Service not working | Label/selector mismatch       | Synced Deployment labels with Service |


✅ 5️⃣ Verification Checklist

| Parameter       | Expected Value            | Status |
| --------------- | ------------------------- | ------ |
| Deployment Name | python-deployment-xfusion | ✔️     |
| Pod Status      | Running                   | ✔️     |
| Container Image | app:latest                | ✔️     |
| Container Port  | 5000                      | ✔️     |
| Service Type    | NodePort                  | ✔️     |
| NodePort        | 32345                     | ✔️     |
| App Output      | Hello World Pyvo 1!       | ✔️     |


🚀 6️⃣ Key Takeaways
1. Always validate container image availability before deployment.

2. Flask applications must bind to 0.0.0.0 for Kubernetes access.

3. NodePort allows external access to cluster services.

4. Matching selectors between Deployment & Service is critical.


