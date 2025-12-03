# Kubernetes Sidecar Containers — Nginx Log Shipping

This guide demonstrates how to implement the **Sidecar Pattern** in Kubernetes by deploying a Pod with two containers:
- **Main container:** Runs Nginx (`nginx-container`)
- **Sidecar container:** Ships logs (`sidecar-container`)

Both containers share an `emptyDir` volume to access Nginx access and error logs.

---

## 🎯 Objective

1. Create a Pod named **webserver**
2. Add an **emptyDir** volume named **shared-logs**
3. Create two containers:
   - **nginx-container** → image: `nginx:latest`
   - **sidecar-container** → image: `ubuntu:latest`
4. Sidecar container must run this command:
sh -c "while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done"


5. Mount the shared volume at `/var/log/nginx` in BOTH containers
6. Ensure the Pod runs successfully with both containers running

---

## 🚀 Step 1: Pod Manifest (webserver.yaml)

Create the file **webserver.yaml**:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webserver
spec:
  volumes:
    - name: shared-logs
      emptyDir: {}
  containers:
    - name: nginx-container
      image: nginx:latest
      volumeMounts:
        - name: shared-logs
          mountPath: /var/log/nginx

    - name: sidecar-container
      image: ubuntu:latest
      command: ["sh", "-c", "while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done"]
      volumeMounts:
        - name: shared-logs
          mountPath: /var/log/nginx

---

## 🚀 Step 2: Apply the Pod
kubectl apply -f webserver.yaml

Verify the Pod:
kubectl get pods


Describe to check two containers:
kubectl describe pod webserver

---

## 🚀 Step 3: Validate Volume & Log Shipping
List logs from both containers:
kubectl exec -it webserver -c nginx-container -- ls /var/log/nginx
kubectl exec -it webserver -c sidecar-container -- ls /var/log/nginx

---

Check log shipping activity:
kubectl logs -f webserver -c sidecar-container

You should see logs printed every 30 seconds.


📘 Key Concepts

emptyDir Volume
Exists as long as the Pod exists

Used for temporary data such as logs

Ideal for sharing files between containers in the same Pod


Sidecar Pattern
Promotes separation of concerns

Main container: Nginx serves web traffic

Sidecar container: Ships logs continuously


✔️ Expected Configuration Summary

| Component         | Value                                    |
| ----------------- | ---------------------------------------- |
| Pod Name          | `webserver`                              |
| Volume            | `shared-logs` (emptyDir)                 |
| Container 1       | `nginx-container` → nginx:latest         |
| Container 2       | `sidecar-container` → ubuntu:latest      |
| Shared Mount Path | `/var/log/nginx`                         |
| Sidecar Command   | `while true; do cat ...; sleep 30; done` |
| Pod Status        | Running                                  |


🎉 Your Sidecar logging architecture is now successfully implemented!
