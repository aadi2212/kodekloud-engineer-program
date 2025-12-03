# Create Docker Image From Running Container

## 📘 Task Overview
A Nautilus developer made changes inside a running container and requested the DevOps team to preserve those changes by creating a new Docker image.

This task is performed on **Application Server 2**.

---

## 🎯 Requirements
- Source container: **ubuntu_latest**
- Target image name: **games**
- Target image tag: **datacenter**
- Create image from the running container

---

## 🚀 Steps Performed

### 1. Verify Running Containers
Check if the container `ubuntu_latest` is running:

```bash
docker ps

Confirmed that the container ubuntu_latest is active.


2. Create Image from Container

Use docker commit to save the current state of the container:
docker commit ubuntu_latest games:datacenter


This command creates a new image:
Repository: games
Tag: datacenter


3. Verify the Newly Created Image
List available images:
docker images

Example output:
REPOSITORY   TAG          IMAGE ID       CREATED          SIZE
games        datacenter   ff6f25964b13   6 seconds ago    132MB
ubuntu       latest       802541663949   3 weeks ago      78.1MB

The image games:datacenter appears as expected.


✅ Final Outcome
1.Successfully created Docker image games:datacenter from the running container ubuntu_latest.

2.The developer’s container changes are now safely preserved as a new reusable image.
