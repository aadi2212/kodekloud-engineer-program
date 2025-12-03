# Troubleshoot Redis Deployment Issues in Kubernetes  
### Fixing Pods Stuck in Pending / ImagePullBackOff States

This guide explains how to diagnose and fix a faulty Redis Deployment (`redis-deployment`) that went down due to incorrect configuration changes.

---

## 🛑 Problem Summary

A Redis Deployment (`redis-deployment`) that was previously running suddenly stopped working after incorrect updates were applied.

Running:

```sh
kubectl get pods


Showed:
redis-deployment-54cdf4f76d-tb5zq     Pending
redis-deployment-5bcd4c7d64-8z44n     ImagePullBackOff / ContainerCreating


🔍 Issues Identified
1] ConfigMap Not Found
Error:
MountVolume.SetUp failed for volume "config": configmap "redis-conig" not found


Cause:
❌ Deployment references ConfigMap redis-conig
✔ Correct name should be redis-config


2] Wrong Container Image
Error:
ImagePullBackOff


Cause:
❌ Image name used: redis:alpin
✔ Correct image: redis:alpine


Step-by-Step Troubleshooting
1] Inspect Deployment
kubectl get deployment redis-deployment -o yaml

Identify incorrect fields:

❌ Wrong ConfigMap Reference

containers:
  - name: redis-container
    image: redis:alpin


🛠️ Fix the Deployment
2] Edit Deployment
kubectl edit deployment redis-deployment

Apply corrections:

✅ Fix ConfigMap name

volumes:
  - name: config
    configMap:
      name: redis-config

✅ Fix Redis image

containers:
  - name: redis-container
    image: redis:alpine


🔄 Apply Changes
3] Delete old pods to force recreation
kubectl delete pod -l app=redis

Deployment will automatically create new pods using the corrected configuration.


✔️ Verify Fix
4] Check new pod status
kubectl get pods


Expected:
redis-deployment-xxxxxxx     Running
redis-deployment-yyyyyyy     Running


Check rollout:
kubectl rollout status deployment/redis-deployment


📄 Optional Debug
5] Check Redis logs
kubectl logs <pod-name>


Expected:
* Ready to accept connections


🎉 Final Outcome

✔ Incorrect ConfigMap reference fixed
✔ Wrong Redis image corrected
✔ Redis pods recreated successfully
✔ Application fully restored and running in the cluster

This concludes the Redis deployment troubleshooting task.



