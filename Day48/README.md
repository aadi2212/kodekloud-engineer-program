# Deploy Pod in Kubernetes Cluster

This document explains the steps followed to create a Kubernetes Pod as per the Nautilus DevOps team requirements.

---

## 📌 **Task Objective**

Create a Pod named **pod-httpd** using the **httpd:latest** image.

### **Requirements**
1. **Pod name:** `pod-httpd`  
2. **Container name:** `httpd-container`  
3. **Label:** `app=httpd_app`  
4. **Image tag:** `httpd:latest` (must be explicitly specified)

---

## ✅ **Steps Followed**

### **1. Created a Pod Manifest File: `pod-httpd.yml`**

Incorrect YAML caused validation errors. The issue was:
- `apiversion` was written instead of `apiVersion`
- `metadata` was written as a list (`- name:`)

### ❌ **Error Encountered**


error: error validating "pod-httpd.yml": error validating data: apiVersion not set;
if you choose to ignore these errors, turn validation off with --validate=false



### 🔍 **Root Cause**
- Wrong key: `apiversion`  
- Incorrect structure under `metadata`

---

## ✅ **Corrected Pod YAML (Final Working Version)**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-httpd
  labels:
    app: httpd_app
spec:
  containers:
    - name: httpd-container
      image: httpd:latest


🚀 2. Applied the Manifest
kubectl apply -f pod-httpd.yml


Output:
pod/pod-httpd created


🔎 3. Verification
Check pod using label:
kubectl get pods -l app=httpd_app


Expected Output:
NAME        READY   STATUS    RESTARTS   AGE
pod-httpd   1/1     Running   0          10s


📄 4. Describe Pod for Detailed Info
kubectl describe pod pod-httpd


This confirms:
Pod labels → app=httpd_app

Container name → httpd-container

Image → httpd:latest

Events → scheduling, image pulling, container start


🎉 Final Outcome
Pod pod-httpd created successfully

Container httpd-container running with httpd:latest image

YAML syntax issue resolved

Verified using kubectl get and kubectl describe


if you choose to ignore these errors, turn validation off with --validate=false
