# 🛠️ Fixing Nginx–PHP-FPM Shared Volume Issue in Kubernetes

This document explains how the Nautilus DevOps team investigated and fixed a multi-container Kubernetes Pod issue where Nginx failed to serve PHP files correctly due to mismatched document root paths.

---

## 📌 Scenario Overview

A Pod running **Nginx** and **PHP-FPM** stopped serving the PHP application. The task was to identify the root cause, fix it, recreate the Pod, and ensure the website loads correctly.

### 🔹 Pod Details
- **Pod Name:** `nginx-phpfpm`
- **ConfigMap Name:** `nginx-config`
- **Containers:**
  - `nginx-container` → Web server
  - `php-fpm-container` → PHP processor

---

## 🔍 Root Cause Analysis

Both containers used the same shared volume, but mounted it at **different paths**:

| Container          | Mount Path             | Volume Name   |
|--------------------|------------------------|---------------|
| php-fpm-container  | `/var/www/html`        | shared-files  |
| nginx-container    | `/usr/share/nginx/html`| shared-files  |

### ❗ What Went Wrong
1. PHP-FPM created and processed files inside:  
   `/var/www/html`
2. Nginx was serving files from:  
   `/usr/share/nginx/html` → **an empty directory**
3. Result: Nginx could not find PHP files → **404/403 errors**

---

## ✅ Solution Steps

### **1. Export Existing Pod Definition**
```sh
kubectl get pod nginx-phpfpm -o yaml > /home/thor/definition.yml


2. Edit the Pod YAML
vi /home/thor/definition.yml


Locate the nginx-container → volumeMounts section and change:


Original:
mountPath: /usr/share/nginx/html
name: shared-files


Updated:
mountPath: /var/www/html
name: shared-files


✔ Ensure BOTH containers use:
volumeMounts:
  - mountPath: /var/www/html
    name: shared-files


3. Recreate the Pod (because volumeMounts are immutable)
kubectl delete pod nginx-phpfpm
kubectl apply -f /home/thor/definition.yml


4. Verify Pod Status
kubectl get pods


Both containers should show Running.


5. Copy PHP File to Document Root
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html -c nginx-container


Verify the file inside container:
kubectl exec -it nginx-phpfpm -c nginx-container -- ls /var/www/html


6. Test the Website
Use the lab’s Website button.
✔ The PHP page should now load correctly.


📚 Key Learnings

✔ 1. Immutable Fields in Kubernetes

Fields like volumeMounts and volumes cannot be edited.
You must delete & recreate the Pod.


✔ 2. Matching Document Roots

For Nginx + PHP-FPM:

Both must point to the same directory

Especially when using a shared volume


✔ 3. Multi-Container Pods Require Consistency

Shared volumes must be mounted identically across containers.


🎉 Final Outcome

The issue was resolved by aligning the shared volume mount paths between Nginx and PHP-FPM.
The website began loading successfully again.

Task Status: Completed ✅


sh
kubectl get pod nginx-phpfpm -o yaml > /home/thor/definition.yml
