# 🔄 Kubernetes Deployment Rollback  
### Reverting `nginx-deployment` to the Previous Working Version

This document explains the steps followed to rollback the **nginx-deployment** after a customer reported a bug in the newly deployed version.

---

## 📌 Scenario

The Nautilus DevOps team recently deployed a new application version using a Kubernetes Deployment named **nginx-deployment**.  
A customer reported issues in the new release, and the team needed to revert the Deployment to its previous stable revision.

The `kubectl` client on the jump_host is already configured for the cluster.

---

## 🛠️ Steps to Perform Rollback

---

### **1. Check Deployment Rollout History**

```sh
kubectl rollout history deployment nginx-deployment

This retrieves the revision history and verifies that a previous version exists to rollback to.


2. Rollback to the Previous Revision
kubectl rollout undo deployment nginx-deployment

This reverts nginx-deployment to the last successful revision.



3. Verify Rollout Status
kubectl rollout status deployment nginx-deployment

Ensures the rollback completed successfully and that all pods are updated and stable.


4. Validate Deployment and Pods
kubectl get pods
kubectl describe deployment nginx-deployment


These commands confirm:
Pods are running with the previous stable version

Deployment configuration is correct

Application stability is restored



✅ Outcome
nginx-deployment successfully rolled back to the previous revision

All pods are running the older, stable image

The application is functioning normally again

The faulty update has been safely reverted



Rolling back deployments is a critical skill for ensuring stability in production environments. Kubernetes makes this process safe and efficient using rollout history and undo commands.



