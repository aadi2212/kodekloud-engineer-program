# Docker Port Mapping – Nginx Container Setup

## Task Overview
The Nautilus DevOps team needs to host an application using an **nginx-based container**.  
This document outlines the steps to deploy an nginx container on **Application Server 2** with proper port mapping.

---

## 🚀 Steps to Complete the Task

### 1. SSH into Application Server 2
```bash
ssh steve@172.16.238.11


2. Pull the nginx:alpine Docker Image
docker pull nginx:alpine

This pulls a lightweight Alpine Linux–based nginx image.



3. Create and Run a Container Named games
docker run -d --name games -p 6300:80 nginx:alpine


Explanation:
-d → Run container in detached mode

--name games → Assigns a custom container name

-p 6300:80 → Maps host port 6300 → container port 80



4. Verify the Running Container
docker ps


Expected Output Example:
CONTAINER ID   IMAGE          COMMAND                  PORTS                  NAMES
<id>           nginx:alpine   "nginx -g 'daemon of…"   0.0.0.0:6300->80/tcp   games


5. Test Nginx Page (Optional)
curl http://localhost:6300

You should receive the nginx default welcome HTML response.


📝 Notes & Learning
nginx:alpine is lightweight, ideal for quick deployments.

Port mapping (-p host:container) is essential for exposing container services.

Naming containers improves usability for logs, restarts, and exec commands.


✔ Task Completed Successfully.
