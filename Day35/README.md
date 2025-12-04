# Install Docker Packages and Start Docker Service  
**Environment:** App Server 3 – Stratos Datacenter  

---

## 📌 **Task Objective**

Install Docker CE and Docker Compose on **App Server 3**, then start and verify the Docker service.

### Requirements:
1. Install **docker-ce** and **docker compose**.
2. Start and enable the Docker service.
3. Verify installations.

---

## 🚀 **Steps Performed**

---

## **1️⃣ Install Docker CE**

```bash
sudo yum install -y yum-utils

sudo yum-config-manager \
    --add-repo https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install -y docker-ce docker-ce-cli containerd.io


2️⃣ Install Docker Compose
❌ Option 1: Using package manager
sudo yum install -y docker-compose

This option did not work, so the manual installation method was used.


✅ Option 2: Install Docker Compose binary
sudo curl -L "https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-$(uname -s)-$(uname -m)" \
    -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version



3️⃣ Start & Enable Docker Service
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker


start → Immediately starts Docker

enable → Ensures Docker autostarts on boot

status → Confirms active running status



4️⃣ Verify Docker Installation
Check Docker engine version:
docker version


View Docker daemon info:
docker info


Run a test container:
docker run hello-world


Successful output confirms Docker is working.


📝 Notes / Observations
Docker service must be running before using Docker Compose.

The binary method is recommended when yum provides an outdated Compose version.

After installation, the server is ready for containerization tasks.


✅ Final Outcome
Docker CE installed successfully

Docker Compose installed using official binary

Docker service started and enabled

hello-world container executed successfully


App Server 3 is now fully prepared for Docker-based application testing.



