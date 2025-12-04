# Init Containers in Kubernetes  
### Using Init Containers for Pre-Configuration Tasks

This guide walks through deploying an application on Kubernetes using **init containers** to perform setup tasks before the main application container runs. This setup is useful when configuration changes cannot be embedded directly into the container image.

---

## ✅ **Objective**

Deploy a Kubernetes application that:

- Uses an **init container** to generate a configuration file.
- Shares that configuration with the **main container** through an `emptyDir` volume.
- Continuously prints the configuration file content.

---

## 🧩 **Requirements**

| Item | Value |
|------|-------|
| Deployment Name | `ic-deploy-devops` |
| Replicas | `1` |
| Label | `app=ic-devops` |
| Init Container | `ic-msg-devops` |
| Main Container | `ic-main-devops` |
| Volume | `ic-volume-devops` (`emptyDir`) |
| Volume Mount Path | `/ic` |

### **Init Container Task**
Writes the message

Init Done - Welcome to xFusionCorp Industries



to `/ic/official`

### **Main Container Task**
Reads and prints `/ic/official` every 5 seconds.

---

## 📄 **Step 1: Deployment YAML**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ic-deploy-devops
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ic-devops
  template:
    metadata:
      labels:
        app: ic-devops
    spec:
      initContainers:
        - name: ic-msg-devops
          image: ubuntu:latest
          command: ["/bin/bash", "-c", "echo Init Done - Welcome to xFusionCorp Industries > /ic/official"]
          volumeMounts:
            - name: ic-volume-devops
              mountPath: /ic
      containers:
        - name: ic-main-devops
          image: ubuntu:latest
          command: ["/bin/bash", "-c", "while true; do cat /ic/official; sleep 5; done"]
          volumeMounts:
            - name: ic-volume-devops
              mountPath: /ic
      volumes:
        - name: ic-volume-devops
          emptyDir: {}

Save this as ic-deploy-devops.yml


🚀 Step 2: Apply the Deployment
kubectl apply -f ic-deploy-devops.yml


Expected Output:
deployment.apps/ic-deploy-devops created


📌 Step 3: Check Pod Status
kubectl get pods


You should see something like:
ic-deploy-devops-xxxxx   1/1   Running


📝 Step 4: Verify Init Container Output
kubectl logs <pod-name>


Expected logs:
Init Done - Welcome to xFusionCorp Industries

Repeated every 5 seconds by the main container.


📂 Step 5: Verify Shared Volume Content
kubectl exec -it <pod-name> -- cat /ic/official


Expected:
Init Done - Welcome to xFusionCorp Industries

This confirms the init container successfully wrote the file, and the main container can access it.


📘 Key Concepts Learned
1. Init Containers
   Run before the main container and perform setup tasks.

2. emptyDir Volume
   Provides temporary shared storage for all containers inside a pod.

3. VolumeMounts
   Ensures both containers access the same shared location.

4. Accuracy Matters
   Names and spec values must match exactly for automated task checkers.


⚠️ Common Issues & Fixes
| Issue                            | Cause                | Fix                                             |
| -------------------------------- | -------------------- | ----------------------------------------------- |
| `unknown field "initcontainers"` | Wrong capitalization | Use `initContainers`                            |
| `unknown field "volumeMount"`    | Wrong key name       | Use `volumeMounts`                              |
| File missing in main container   | Volume not mounted   | Ensure same volume name used in both containers |
| Task fails                       | Name mismatch        | Use exact names given in task                   |


🎉 Conclusion
This task demonstrates how init containers + emptyDir volumes can prepare required files before the main app starts. The setup is clean, stable, and recommended for pre-deployment configuration tasks.


✅ Outcome: Deployment with init container successfully implemented.
Writes the message:

