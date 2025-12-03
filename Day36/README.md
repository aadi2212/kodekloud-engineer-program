# Deploy Nginx Container on Application Server 2

## 📝 Task Description

The Nautilus DevOps team requested deployment of an **nginx container** on **Application Server 2** for application testing. The requirements were:

- Use the image **nginx:alpine**
- Create the container with name **nginx_2**
- Ensure the container is **running**

---

## ✅ Steps Performed

### 1️⃣ Login to Application Server 2
```bash
ssh user@appserver2


2️⃣ Verify Docker installation
docker --version
systemctl status docker


If Docker was not running:
sudo systemctl start docker
sudo systemctl enable docker


3️⃣ Pull the nginx:alpine image
sudo docker pull nginx:alpine


4️⃣ Create and start the container
sudo docker run -d --name nginx_2 nginx:alpine


Flags used:
-d → Run in detached mode

--name nginx_2 → Assign container name

nginx:alpine → Lightweight Nginx image based on Alpine Linux



5️⃣ Verify the container is running
sudo docker ps


Expected Output:
CONTAINER ID   IMAGE          COMMAND                  STATUS        NAMES
abcd1234efgh   nginx:alpine   "/docker-entrypoint.…"   Up X minutes  nginx_2


6️⃣ Optional: Check container logs
sudo docker logs nginx_2


🎉 Final Outcome
Container Name: nginx_2

Image Used: nginx:alpine

Status: Running successfully on Application Server 2

The deployment meets all requested conditions.



