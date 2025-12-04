# 🚀 Deploy Applications with Kubernetes Deployments  
## KodeKloud Task Documentation – HTTPD Deployment

This document explains how the Nautilus DevOps team deployed an **httpd application** using a **Kubernetes Deployment** while troubleshooting YAML validation errors.

---

## 📌 Objective

Create a **Deployment** named `httpd` to deploy an application using the image:

- **Image:** `httpd:latest`  
- **Container name:** `httpd-container`  
- **Label:** `app=httpd_app`  

The `kubectl` utility on the jump_host is already configured to interact with the Kubernetes cluster.

---

## 🛠️ Steps Followed

---

### **1. Initial Deployment YAML (Incorrect Version)**

The following YAML was applied initially but contained multiple errors:

```yaml
appVersion: /apps/v1
kind: Deployment
metadata:
  name: httpd
spec:
  replicas: 1
  selector:
    matchlabels:
      app: httpd_app
  template:
    metadata:
      labels:
        app: httpd_app
    spec:
      containers:
         - name: httpd-container
          image: httpd:latest


❌ Errors Encountered
When applying:
kubectl apply -f httpd-deployment.yaml


The following error occurred:
error: error validating "httpd-deployment.yaml": error validating data: apiVersion not set


🔍 Root Causes
1. appVersion was incorrectly used → should be apiVersion

2. matchlabels was incorrectly written → should be matchLabels (camelCase)

3. Wrong apiVersion format


✅ 2. Corrected Deployment YAML
After fixing all errors, the correct manifest is:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd
spec:
  replicas: 1
  selector:
    matchLabels:
      app: httpd_app
  template:
    metadata:
      labels:
        app: httpd_app
    spec:
      containers:
        - name: httpd-container
          image: httpd:latest


📥 3. Apply the Correct Configuration
kubectl apply -f httpd-deployment.yaml


🔎 4. Verification Steps
Check deployment status
kubectl get deployment httpd


Check pods created by the deployment
kubectl get pods -l app=httpd_app


Describe deployment for detailed information
kubectl describe deployment httpd


🎉 Final Outcome
✅ The httpd deployment was successfully created.

✅ Pod(s) are running using httpd:latest.

✅ YAML structural and spelling errors were corrected.

✅ Deployment verified using kubectl get and kubectl describe.

   
        - name: httpd-container
          image: httpd:latest
