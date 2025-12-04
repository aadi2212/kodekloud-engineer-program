# Deploy MySQL on Kubernetes

## 📝 Task Overview
Deploy a MySQL database server on a Kubernetes cluster using persistent storage, secrets, environment variables, and expose it using a NodePort service.

---

## 📦 1️⃣ Create Kubernetes Secrets

### **mysql-root-pass**
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysql-root-pass
type: Opaque
stringData:
  password: YUIidhb667


mysql-user-pass

apiVersion: v1
kind: Secret
metadata:
  name: mysql-user-pass
type: Opaque
stringData:
  username: kodekloud_aim
  password: GyQkFRVNr3


Apply secrets:
kubectl apply -f mysql-secrets.yaml


🗄️ 2️⃣ Create PersistentVolume

apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 250Mi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /mnt/data/mysql

Apply:
kubectl apply -f mysql-pv.yaml


📁 3️⃣ Create PersistentVolumeClaim

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 250Mi

Apply:
kubectl apply -f mysql-pvc.yaml


🐬 4️⃣ Create MySQL Deployment

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql-container
          image: mysql:8.0
          ports:
            - containerPort: 3306
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-root-pass
                  key: password

            - name: MYSQL_DATABASE
              valueFrom:
                secretKeyRef:
                  name: mysql-db-url
                  key: database

            - name: MYSQL_USER
              valueFrom:
                secretKeyRef:
                  name: mysql-user-pass
                  key: username

            - name: MYSQL_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-user-pass
                  key: password

          volumeMounts:
            - name: mysql-storage
              mountPath: /var/lib/mysql

      volumes:
        - name: mysql-storage
          persistentVolumeClaim:
            claimName: mysql-pv-claim

Apply:
kubectl apply -f mysql-deployment.yaml


🌐 5️⃣ Create NodePort Service

apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  type: NodePort
  selector:
    app: mysql
  ports:
    - port: 3306
      targetPort: 3306
      nodePort: 30007

Apply:
kubectl apply -f mysql-service.yaml


6️⃣ Verification Commands

kubectl get pods
kubectl get pv
kubectl get pvc
kubectl get svc mysql
kubectl describe pod mysql-deployment-<pod-name>


Expected output:
Pod is Running

PVC is Bound

PV is available

NodePort 30007 exposed successfully

Environment variables correctly injected via secrets


⚠️ Common Errors & Fixes

| Issue                      | Cause                                      | Fix                                      |
| -------------------------- | ------------------------------------------ | ---------------------------------------- |
| CreateContainerConfigError | Typo: `valueForm` instead of `valueFrom`   | Correct field spelling                   |
| Secret not working         | Wrong YAML format (`key:` / `value:` used) | Use `stringData:` with `database: value` |
| PVC not mounted            | Wrong claimName                            | Ensure `claimName: mysql-pv-claim`       |
| Pod stuck in Init          | PV path missing                            | Create hostPath directory                |


💡 Lessons Learned:
1. Secrets must map keys directly under stringData.

2. Always verify secrets using:
kubectl describe secret <name>

3. Correct indentation is crucial in YAML.

4. Use kubectl logs and kubectl describe pod for debugging.

5. PVC names and secret keys are case-sensitive.

