# Manage Secrets in Kubernetes

## 🧠 Task: Store License Information Using Kubernetes Secrets

The Nautilus DevOps team needs to securely store license-based tool credentials inside the Kubernetes cluster.  
Kubernetes **Secrets** allow sensitive information (passwords, API keys, license numbers) to be stored securely.

---

## 🎯 Objectives

1. Create a **Secret** named `media` using the file `/opt/media.txt`.
2. Create a **Pod** named `secret-nautilus`.
3. Use container:
   - **Name:** `secret-container-nautilus`
   - **Image:** `fedora:latest`
   - **Command:** `sleep 4800`
4. Mount the secret at:  
   **`/opt/apps`** inside the container.
5. Verify the secret is accessible inside the pod.

---

## 🧩 Specifications

| Component        | Details                       |
|------------------|-------------------------------|
| Secret Name      | `media`                       |
| Secret File      | `/opt/media.txt`              |
| Pod Name         | `secret-nautilus`             |
| Container Name   | `secret-container-nautilus`   |
| Image            | `fedora:latest`               |
| Mount Path       | `/opt/apps`                   |
| Command          | `sleep 4800`                  |

---

## 🛠️ Step 1: Create the Secret

Use the file `/opt/media.txt` to generate a Kubernetes secret:


✔️ Verify the Secret
kubectl get secrets
kubectl describe secret media



🛠️ Step 2: Create the Pod YAML File
Create a file named secret-nautilus.yml and add the following spec:

apiVersion: v1
kind: Pod
metadata:
  name: secret-nautilus
spec:
  containers:
    - name: secret-container-nautilus
      image: fedora:latest
      command: ["sleep", "4800"]
      volumeMounts:
        - name: secret-volume
          mountPath: /opt/apps
  volumes:
    - name: secret-volume
      secret:
        secretName: media


🛠️ Step 3: Apply the Pod Configuration
kubectl apply -f secret-nautilus.yml
kubectl get pods

Wait until the pod shows Running status.


Step 4: Verify the Secret Inside the Pod
kubectl exec -it secret-nautilus -- /bin/bash


Then inside the container:
cd /opt/apps
ls
cat media.txt

You should now see the license/password from the original media.txt file.


✅ Final Outcome
Secret media is securely stored inside Kubernetes.

Pod secret-nautilus mounts and consumes the secret.

Secret data is available at /opt/apps inside the container.

Sensitive license information remains secure within the cluster.


```bash
kubectl create secret generic media --from-file=/opt/media.txt
