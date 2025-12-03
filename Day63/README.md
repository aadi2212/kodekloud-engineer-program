# Deploy IRON Gallery App on Kubernetes

## 🎯 Objective
Deploy the **Iron Gallery App (frontend)** and **Iron DB (backend)** on a Kubernetes cluster inside a dedicated namespace.  
Configure deployments, volumes, environment variables, resource limits, and services as required.

---

# 🧱 Step 1: Create Namespace

**File:** `namespace.yml`

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: iron-namespace-xfusion

Apply:
kubectl apply -f namespace.yml
kubectl get namespaces


🌐 Step 2: Iron Gallery Deployment (Frontend)
File: iron-gallery-deployment.yml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-gallery-deployment-xfusion
  namespace: iron-namespace-xfusion
  labels:
    run: iron-gallery
spec:
  replicas: 1
  selector:
    matchLabels:
      run: iron-gallery
  template:
    metadata:
      labels:
        run: iron-gallery
    spec:
      containers:
        - name: iron-gallery-container-xfusion
          image: kodekloud/irongallery:2.0
          resources:
            limits:
              memory: "100Mi"
              cpu: "50m"
          volumeMounts:
            - name: config
              mountPath: /usr/share/nginx/html/data
            - name: images
              mountPath: /usr/share/nginx/html/uploads
      volumes:
        - name: config
          emptyDir: {}
        - name: images
          emptyDir: {}

Apply:
kubectl apply -f iron-gallery-deployment.yml
kubectl get pods -n iron-namespace-xfusion


Step 3: Iron DB Deployment (Backend)
File: iron-db-deployment.yml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-db-deployment-xfusion
  namespace: iron-namespace-xfusion
  labels:
    db: mariadb
spec:
  replicas: 1
  selector:
    matchLabels:
      db: mariadb
  template:
    metadata:
      labels:
        db: mariadb
    spec:
      containers:
        - name: iron-db-container-xfusion
          image: kodekloud/irondb:2.0
          env:
            - name: MYSQL_DATABASE
              value: database_host
            - name: MYSQL_ROOT_PASSWORD
              value: "rootpass123"
            - name: MYSQL_PASSWORD
              value: "userpass456"
            - name: MYSQL_USER
              value: "customuser"
          volumeMounts:
            - name: db
              mountPath: /var/lib/mysql
      volumes:
        - name: db
          emptyDir: {}

Apply:
kubectl apply -f iron-db-deployment.yml
kubectl get pods -n iron-namespace-xfusion


Step 4: Iron DB Service (ClusterIP)
File: iron-db-service.yml

apiVersion: v1
kind: Service
metadata:
  name: iron-db-service-xfusion
  namespace: iron-namespace-xfusion
spec:
  selector:
    db: mariadb
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
  type: ClusterIP


Apply:
kubectl apply -f iron-db-service.yml
kubectl get svc -n iron-namespace-xfusion


🌍 Step 5: Iron Gallery Service (NodePort)
File: iron-gallery-service.yml

apiVersion: v1
kind: Service
metadata:
  name: iron-gallery-service-xfusion
  namespace: iron-namespace-xfusion
spec:
  selector:
    run: iron-gallery
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 32678
  type: NodePort


Apply:
kubectl apply -f iron-gallery-service.yml
kubectl get svc -n iron-namespace-xfusion


🌐 Access the Iron Gallery App
Once the service is up, access via:
http://<Node-IP>:32678

This should display the Iron Gallery installation page.


🔍 Final Verification
kubectl get all -n iron-namespace-xfusion


You should see:
2 Deployments (frontend + DB)

2 Pods (one each)

2 Services (ClusterIP + NodePort)

Namespace active and running


⚙️ Troubleshooting Tips
| Issue                    | Cause                     | Fix                              |
| ------------------------ | ------------------------- | -------------------------------- |
| Namespace already exists | Pre-created               | Continue deployment              |
| YAML indentation errors  | Misaligned spacing        | Validate YAML before applying    |
| DB CrashLoopBackOff      | Wrong env/config          | Fix env variables                |
| NodePort inaccessible    | Port not open or wrong IP | Check cluster node port mappings |


🧠 Summary
Successfully deployed Iron Gallery frontend and Iron DB backend inside a dedicated namespace.

Configured volumes, resource limits, and environment variables as required.

Exposed the frontend using NodePort 32678.

Verified all Kubernetes objects are running correctly.


The Iron Gallery installation page is now accessible, completing the deployment successfully.



