# ⚙️ Set Resource Limits in Kubernetes Pods  
### Task: Configure Resource Requests & Limits for `httpd-pod`

This document describes the steps taken to create a Kubernetes Pod with proper CPU and memory resource constraints to prevent performance issues caused by unbounded resource usage.

---

## 📌 Objective

The Nautilus DevOps team identified performance issues across several applications running in Kubernetes due to uncontrolled resource consumption.  
To address this, the goal was to:

- Create a Pod named **httpd-pod**
- Use container:
  - **Name:** `httpd-container`
  - **Image:** `httpd:latest`
- Apply resource constraints:

| Resource Type | Memory | CPU   |
|---------------|---------|-------|
| **Requests**  | 15Mi    | 100m  |
| **Limits**    | 20Mi    | 100m  |

The `kubectl` utility on the jump_host is already configured for cluster access.

---

## 🛠️ Steps Performed

---

### **1. Created Pod Manifest File**

Created the file **httpd-pod.yml** with the following initial configuration:

```yaml
apiVersion: v1
Kind: Pod    # ❌ Incorrect (uppercase "K")
metadata:
  name: httpd-pod
spec:
  containers:
    - name: httpd-container
      image: httpd:latest
      resources:
        requests:
          memory: "15Mi"
          cpu: "100m"
        limits:
          memory: "20Mi"
          cpu: "100m"



2. Error Encountered
Applied the manifest:
kubectl apply -f httpd-pod.yml


Error shown:
error: error validating "httpd-pod.yml": error validating data: kind not set;
if you choose to ignore these errors, turn validation off with --validate=false


Root Cause
Kubernetes fields are case-sensitive.
The YAML used Kind instead of kind → causing validation failure.


3. Fixed the Manifest
Corrected YAML:

apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
    - name: httpd-container
      image: httpd:latest
      resources:
        requests:
          memory: "15Mi"
          cpu: "100m"
        limits:
          memory: "20Mi"
          cpu: "100m"


4. Applied the Corrected Configuration
kubectl apply -f httpd-pod.yml


5. Verification
Check pod status:
kubectl get pods


Verify resource limits:
kubectl describe pod httpd-pod | grep -A5 "Limits"


✅ Outcome
The httpd-pod was successfully created.

CPU and memory requests and limits were applied properly.

The issue was resolved by fixing the YAML kind field.


💡 Benefits
Prevents the container from consuming excessive CPU or memory.

Protects other workloads by enforcing fair resource allocation.

Improves cluster stability and performance.



Acknowledgement
Special thanks to KodeKloud for providing practical, real-world DevOps challenges that strengthen troubleshooting skills.



