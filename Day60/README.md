# Persistent Volumes in Kubernetes  
### Deploying a Web Application Using PV, PVC, Pod & NodePort Service

This guide explains how to deploy an **HTTP web server** on Kubernetes using a **PersistentVolume (PV)** and **PersistentVolumeClaim (PVC)** for storage, and expose it externally using a **NodePort service**.

---

## ✅ Objective

Set up:

- A PersistentVolume (`pv-xfusion`)
- A PersistentVolumeClaim (`pvc-xfusion`)
- A Pod running `httpd:latest` with the PVC mounted to document root
- A NodePort Service (`web-xfusion`) exposed on port **30008**

---

## 📁 Task Requirements

### **1] PersistentVolume**

| Field | Value |
|-------|-------|
| Name | `pv-xfusion` |
| StorageClass | `manual` |
| Capacity | `5Gi` |
| Access Mode | `ReadWriteOnce` |
| Type | `hostPath` |
| Host Path | `/mnt/security` |

---

### **2] PersistentVolumeClaim**

| Field | Value |
|-------|-------|
| Name | `pvc-xfusion` |
| StorageClass | `manual` |
| Requested Storage | `1Gi` |
| Access Mode | `ReadWriteOnce` |

---

### **3] Pod**

| Field | Value |
|-------|-------|
| Pod Name | `pod-xfusion` |
| Container Name | `container-xfusion` |
| Image | `httpd:latest` |
| PVC Mount Path | `/usr/local/apache2/htdocs` |
| Label | `app: xfusion` |

---

### **4] NodePort Service**

| Field | Value |
|-------|-------|
| Name | `web-xfusion` |
| Type | `NodePort` |
| NodePort | `30008` |
| Selector | `app: xfusion` |

---

# 📄 YAML Files

---

## **1] pv-xfusion.yml**

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-xfusion
spec:
  storageClassName: manual
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/security


2] pvc-xfusion.yml

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-xfusion
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi


3] pod-xfusion.yml

apiVersion: v1
kind: Pod
metadata:
  name: pod-xfusion
  labels:
    app: xfusion
spec:
  containers:
    - name: container-xfusion
      image: httpd:latest
      volumeMounts:
        - name: xfusion-volume
          mountPath: /usr/local/apache2/htdocs
  volumes:
    - name: xfusion-volume
      persistentVolumeClaim:
        claimName: pvc-xfusion


4] web-xfusion.yml

apiVersion: v1
kind: Service
metadata:
  name: web-xfusion
spec:
  type: NodePort
  selector:
    app: xfusion
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30008


🚀 Deployment Steps
1] Apply PV & PVC
kubectl apply -f pv-xfusion.yml
kubectl apply -f pvc-xfusion.yml


2] Deploy Pod
kubectl apply -f pod-xfusion.yml


3] Create NodePort Service
kubectl apply -f web-xfusion.yml


4] Verify PV & PVC Binding
kubectl get pv,pvc


5] Check Pod Status
kubectl get pod pod-xfusion -o wide


6] Add Test HTML File
kubectl exec -it pod-xfusion -- /bin/sh
echo '<h1>Hello Xfusion HTTPD!</h1>' > /usr/local/apache2/htdocs/index.html
exit


7] Test Web Server
curl http://<NODE_IP>:30008


📝 Important Notes
1. The PVC mount path must match httpd's document root:
/usr/local/apache2/htdocs

2. NodePort must be accessed using Node IP, not Pod IP.

3. PV mounted over document root overrides default httpd content.

4. Add your own HTML file to verify successful mounting.


🎉 Outcome:
✔ PV (pv-xfusion) and PVC (pvc-xfusion) bound successfully
✔ Pod (pod-xfusion) running with httpd:latest and PV storage
✔ Service (web-xfusion) exposes the Pod on NodePort 30008
✔ Web page served correctly from persistent storage
