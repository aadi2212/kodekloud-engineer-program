# Host Static Website on HTTPD using Docker Compose

This documentation explains the setup and deployment of an HTTPD container using Docker Compose on **App Server 2 (Stratos DC)**, as per Nautilus DevOps task requirements.

---

## 📌 Task Requirements

1. Create a Docker Compose file at:
/opt/docker/docker-compose.yml



2. Deploy a container named **httpd** using **httpd:latest** image.  
3. Map **host port 3000** → **container port 80**.  
4. Mount host directory **/opt/itadmin** → **/usr/local/apache2/htdocs** (Apache document root).  
5. Use Docker Compose (service name can be anything).

---

## 📝 Docker Compose File  
**Correct file:** `/opt/docker/docker-compose.yml`

```yaml
version: "3"

services:
  web:
    image: httpd:latest
    container_name: httpd
    ports:
      - "3000:80"
    volumes:
      - /opt/itadmin:/usr/local/apache2/htdocs
    restart: always


🧩 Explanation of Compose Fields
1. services.web:
   Arbitrary service name (web is used here).

2. image:
   Pulls the latest httpd image.

3. container_name:
   Names the container httpd (required).

4. ports:
   Maps 3000 → 80 for web access.

5. volumes:
   Maps host folder /opt/itadmin to Apache document root.

6. restart: always:
   Ensures container restarts after reboot or failure.


🚀 Commands Used
1️⃣ Navigate to directory
cd /opt/docker


2️⃣ Bring down old containers (if any)
docker compose down


3️⃣ Start container using Docker Compose
docker compose -f /opt/docker/docker-compose.yml up -d

Note: docker compose (with space) is used because modern Docker versions use Compose V2.
docker-compose (hyphen) is deprecated.


4️⃣ Verify running container
docker ps


5️⃣ Verify volume mount
docker exec -it httpd ls /usr/local/apache2/htdocs


6️⃣ Test website access
curl http://localhost:3000


⚠️ Errors Faced & Lessons Learned
❌ 1. Incorrect Key: docker_container
Error:
-bash: docker-compose: command not found

Fix:
Use container_name: instead.


❌ 2. Wrong Command: docker-compose
Error:
-bash: docker-compose: command not found

Reason:
Docker Compose V2 uses docker compose.


❌ 3. Wrong Volume Path (from earlier attempts)

Wrong: /opt/devops

Correct: /opt/itadmin

Lesson: Exact paths matter — graders verify them strictly.


🎉 Final Outcome

HTTPD container httpd deployed successfully.

Static website content hosted via volume /opt/itadmin.

Accessible at:
http://<server-ip>:3000


Deployment validated via Docker Compose and curl testing.


