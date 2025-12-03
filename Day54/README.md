# Kubernetes Volume Sharing — Multi-Container Pod (emptyDir)

This guide demonstrates how to create a Kubernetes Pod with two containers that share a common `emptyDir` volume.  
Files written in one container will instantly appear in the other.

---

## 🎯 Objective

1. Create a Pod named **volume-share-datacenter**
2. Add two containers:
   - **volume-container-datacenter-1** → fedora:latest, mounts `/tmp/beta`
   - **volume-container-datacenter-2** → fedora:latest, mounts `/tmp/games`
3. Use a shared volume named **volume-share** of type `emptyDir`
4. Keep both containers running using:  
   `sleep infinity`
5. Create a file in container 1 and verify that it appears in container 2

---

## 📦 Correct Kubernetes Manifest (volume-share-datacenter.yaml)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-datacenter
spec:
  containers:
    - name: volume-container-datacenter-1
      image: fedora:latest
      command: ["sleep", "infinity"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/beta

    - name: volume-container-datacenter-2
      image: fedora:latest
      command: ["sleep", "infinity"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/games

  volumes:
    - name: volume-share
      emptyDir: {}


🚀 Step 1: Apply the Pod Manifest
kubectl apply -f volume-share-datacenter.yaml


Check pod status:
kubectl get pods


🚀 Step 2: Create File in Container 1
Open a shell inside the first container:
kubectl exec -it volume-share-datacenter -c volume-container-datacenter-1 -- bash


Create the test file:
echo "This is a test file for shared volume" > /tmp/beta/beta.txt
ls -l /tmp/beta


Exit container:
exit


🚀 Step 3: Verify File in Container 2
Enter the second container:
kubectl exec -it volume-share-datacenter -c volume-container-datacenter-2 -- bash


Check shared volume:
ls -l /tmp/games
cat /tmp/games/beta.txt


Expected output:
This is a test file for shared volume


🧠 Notes & Lessons Learned
✔ Shared Volume Behavior
emptyDir exists as long as the Pod is running

Data is visible to all containers mounting the same volume

Useful for temporary file sharing between containers


✔ YAML Best Practices
Use correct key: volumeMounts (case-sensitive)

Maintain proper spacing: name: volume-share

Avoid indentation mistakes


✅ Expected Result

| Action                                   | Result                                               |
| ---------------------------------------- | ---------------------------------------------------- |
| Create file in `/tmp/beta` (container 1) | File instantly appears in `/tmp/games` (container 2) |
| Volume type                              | emptyDir (ephemeral shared storage)                  |
| Both containers running                  | Yes                                                  |


🎉 You have successfully created a multi-container pod with a shared volume in Kubernetes!
    - name: volume-share
      emptyDir: {}
