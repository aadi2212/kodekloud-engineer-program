# 🚀 Rolling Update for Nginx Deployment  
### Updating `nginx-deployment` to Image `nginx:1.17`

This document outlines the steps performed to apply a rolling update to the Kubernetes Deployment **nginx-deployment**, updating its container image to **nginx:1.17** while ensuring zero downtime.

---

## 📌 Objective

The Nautilus application team released a new version of the Nginx-based application using the image **nginx:1.17**.  
The goal was to update the deployment in a rolling fashion and ensure all pods are running and stable after the update.

The `kubectl` client on the jump_host is already configured to communicate with the Kubernetes cluster.

---

## 🛠️ Steps Performed

---

### **1. Verify Existing Deployment**

```sh
kubectl get deployments
kubectl describe deployment nginx-deployment


Confirmed that the nginx-deployment exists.
Checked the current container image being used.


2. Execute Rolling Update
kubectl set image deployment/nginx-deployment nginx-container=nginx:1.17


Updated the container image to nginx:1.17.

Kubernetes automatically triggered a rolling update.



3. Monitor Rollout Progress
kubectl rollout status deployment/nginx-deployment


Ensured all pods updated successfully.

Waited until the deployment reached the "successfully rolled out" status.


4. Verify Updated Pods
kubectl get pods
kubectl describe pod <pod-name>


Confirmed the pods are running with the new image.

Verified all pods are in Running and Ready state.


✅ Outcome
The nginx-deployment was successfully updated to nginx:1.17.

Rolling update occurred with zero downtime.

All pods are healthy and operational after the update.


💡 Benefits of Rolling Updates

No service interruption during deployment.

Gradual update ensures stability and smooth transitions.

Ability to roll back instantly if issues occur.





